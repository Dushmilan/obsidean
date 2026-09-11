# File I/O & Build Tools

Modern Java I/O is `java.nio.file` — simple, exception-driven, and safe. Build tools (Maven, Gradle) manage dependencies and the build lifecycle so you never hand-`javac` a classpath again. Together they take you from "compiles" to "real project."

**The Intuition:** Reading a file in Java is one line with `Files.readAllLines` — no manual open/close needed (try-with-resources or the `Files` helpers handle it). The `Path` object is the modern representation of a file location. Build tools are project *managers*: they fetch dependencies from a central repository, compile, test, and package — reproducing the build on any machine.

## Path & Files — the modern API

```java
import java.nio.file.*;
import java.util.List;

// Path — a location (doesn't touch the filesystem)
Path p = Path.of("data", "file.txt");       // platform-correct separators
Path home = Path.of("/home/user");
p.getFileName();        // file.txt
p.getParent();          // data
p.resolve("other.txt")  // data/other.txt
p.toString();

// Files — the operations
Files.exists(p);                        // true/false
Files.isDirectory(p);
Files.size(p);                          // bytes
Files.createDirectories(Path.of("a/b/c"));   // creates parents too
Files.copy(src, dst, StandardCopyOption.REPLACE_EXISTING);
Files.move(src, dst);
Files.delete(path);                     // throws if missing
Files.deleteIfExists(path);             // safe
```

## Reading & writing — the one-liners

```java
// Read all lines — the 90% case
List<String> lines = Files.readAllLines(Path.of("data.txt"));
for (String line : lines) { ... }

// Read whole file as one string
String content = Files.readString(Path.of("data.txt"));

// Write
Files.writeString(Path.of("out.txt"), "Hello\nWorld\n");
Files.write(Path.of("out.txt"), List.of("line1", "line2"));

// Big files — stream lazily (O(1) memory)
try (Stream<String> stream = Files.lines(Path.of("big.txt"))) {
    stream.filter(l -> l.contains("error")).limit(10).forEach(System.out::println);
}
// The try-with-resources closes the file when the stream is done
```

## try-with-resources — for manual streams

```java
// When you need a reader/writer (not the Files helpers):
try (BufferedReader br = Files.newBufferedReader(Path.of("data.txt"));
     BufferedWriter bw = Files.newBufferedWriter(Path.of("out.txt"))) {
    String line;
    while ((line = br.readLine()) != null) {
        bw.write(line.toUpperCase());
        bw.newLine();
    }
}   // both closed automatically — even on exceptions
```

## CSV & structured data

```java
// No built-in CSV — but the pattern is simple for simple files:
List<String[]> rows = new ArrayList<>();
try (var br = Files.newBufferedReader(Path.of("grades.csv"))) {
    String line;
    while ((line = br.readLine()) != null) {
        rows.add(line.split(","));
    }
}

// For JSON — use a library (Gson/Jackson) via your build tool
```

## Maven — the build tool

```xml
<!-- pom.xml — the project definition -->
<project>
  <modelVersion>4.0.0</modelVersion>
  <groupId>com.myapp</groupId>
  <artifactId>myapp</artifactId>
  <version>1.0.0</version>

  <properties>
    <maven.compiler.source>17</maven.compiler.source>
    <maven.compiler.target>17</maven.compiler.target>
  </properties>

  <dependencies>
    <dependency>          <!-- fetched from Maven Central automatically -->
      <groupId>com.google.code.gson</groupId>
      <artifactId>gson</artifactId>
      <version>2.10.1</version>
    </dependency>
  </dependencies>
</project>
```

```bash
mvn compile        # compile
mvn test           # run tests (JUnit in src/test/java)
mvn package        # produce a JAR
mvn clean          # delete build output
mvn install        # put the JAR in the local repo for other projects
```

**The Maven directory layout (convention):**
```
myapp/
├── pom.xml
└── src/
    ├── main/java/...      # production code
    ├── main/resources/... # config, properties
    └── test/java/...      # tests
```

## Gradle — the alternative

```groovy
// build.gradle
plugins { id 'java' }
repositories { mavenCentral() }
dependencies {
    implementation 'com.google.code.gson:gson:2.10.1'
    testImplementation 'org.junit.jupiter:junit-jupiter:5.10.0'
}
```
```bash
gradle build    # compile + test + package
gradle test
gradle run
```

## JUnit — testing

```java
// src/test/java/com/myapp/GradeCalculatorTest.java
import org.junit.jupiter.api.Test;
import static org.junit.jupiter.api.Assertions.*;

class GradeCalculatorTest {
    @Test
    void computesAverage() {
        assertEquals(90.0, GradeCalculator.average(List.of(85, 95)), 0.001);
    }

    @Test
    void rejectsEmptyList() {
        assertThrows(IllegalArgumentException.class,
            () -> GradeCalculator.average(List.of()));
    }
}
// Run with: mvn test  (or gradle test)
```

---

**Setup:** Read a CSV, compute the average score, write the result.

**Solution:**
```java
import java.nio.file.*;
import java.util.List;

public class GradeStats {
    public static void main(String[] args) throws IOException {
        List<String> lines = Files.readAllLines(Path.of("grades.csv"));

        int sum = 0, count = 0;
        for (String line : lines) {            // skip header in real code
            String[] parts = line.split(",");
            sum += Integer.parseInt(parts[1]);
            count++;
        }
        double avg = (double) sum / count;

        Files.writeString(Path.of("result.txt"), "Average: " + avg);
    }
}
```

**Key insight:** `Files.readAllLines` + `split` + `Files.writeString` — a complete data pipeline with zero manual resource management. `throws IOException` declares the checked exception; the whole `main` can let it propagate.

---

**Setup:** Count occurrences of each word across all `.txt` files in a directory.

**Solution:**
```java
try (var paths = Files.walk(Path.of("docs"))) {     // recursive walk, auto-closed
    Map<String, Long> counts = new HashMap<>();
    paths.filter(Files::isRegularFile)
        .filter(p -> p.toString().endsWith(".txt"))
        .forEach(p -> {
            try {
                for (String w : Files.readString(p).split("\\s+"))
                    counts.merge(w, 1L, Long::sum);
            } catch (IOException e) {
                throw new UncheckedIOException(e);
            }
        });
    System.out.println(counts);
}
```

**Key insight:** `Files.walk` recursively traverses; `Files::isRegularFile` filters files. The `merge` idiom counts. The `UncheckedIOException` wrapper converts the checked exception inside the lambda (lambdas can't throw checked exceptions without wrapping).

---

**Setup:** Add a library dependency (Gson) with Maven and use it.

**Solution:**
```xml
<dependency>
    <groupId>com.google.code.gson</groupId>
    <artifactId>gson</artifactId>
    <version>2.10.1</version>
</dependency>
```
```java
import com.google.gson.Gson;

Gson gson = new Gson();
String json = gson.toJson(Map.of("name", "Ada", "score", 92));
// {"name":"Ada","score":92}
Map<?, ?> data = gson.fromJson(json, Map.class);
```
```bash
mvn compile   # downloads Gson automatically, then compiles
```

**Key insight:** The build tool *manages the classpath* — `mvn compile` fetches Gson from Maven Central and puts it on the compile path. This is why modern Java projects don't hand-manage `.jar` files.

---

**Setup:** Why do real projects use Maven/Gradle instead of raw `javac`?

**Solution:** Raw `javac` requires you to: find every dependency, download it, construct the classpath, compile in order, and repeat for tests. Maven/Gradle do all of it *reproducibly* — the `pom.xml`/`build.gradle` declares everything, and any machine with the tool builds the identical artifact.

**Key insight:** "It builds on my machine" is the problem build tools solve. Declarative builds + CI (see CI/CD note) make the build a first-class, repeatable part of the project.

---

## Practice (try before peeking)

1. `Files.readAllLines` — what does it throw, checked or unchecked?
2. How do you read a file too big to hold in memory?
3. `mvn package` — what does it produce?

<details><summary>Answers</summary>

1. `IOException` — checked; the compiler requires `throws` or `try/catch`.
2. `Files.lines(...)` returns a lazy `Stream<String>` — process line by line without loading the whole file.
3. A `.jar` (or `.war`) — the compiled, packaged artifact in `target/`, ready to run (`java -jar`).

</details>

---

**Common traps:**
- Using `File` (old API) instead of `Path`/`Files` (modern) — the old one lacks `readString`, `lines`, `walk`
- Forgetting `throws IOException` on methods using `Files` — compile error until you declare it
- `Files.readAllLines` on huge files — OOM; use `Files.lines` lazily
- Not closing a `Files.lines` stream — file handle leak; use try-with-resources
- Hand-managing JARs/classpath — use a build tool; a hand-rolled classpath is a nightmare to reproduce

---
