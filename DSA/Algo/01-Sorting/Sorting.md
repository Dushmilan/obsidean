Sorting is the fundamental algorithmic operation of arranging elements in a specific order — usually ascending or descending. It's a prerequisite for more efficient algorithms like binary search, and most real-world data processing starts here.

**The Intuition:** Think of sorting like organizing a deck of cards. You pick up one card at a time and place it in the right position relative to the ones you've already arranged. Some methods (like bubble sort) swap adjacent cards repeatedly, while others (like merge sort) split the deck in half, sort each pile separately, then merge them back together.

**The Math:**

| Algorithm | Best | Average | Worst | Space | Stable |
|-----------|------|---------|-------|-------|--------|
| Bubble | $O(n)$ | $O(n^2)$ | $O(n^2)$ | $O(1)$ | Yes |
| Selection | $O(n^2)$ | $O(n^2)$ | $O(n^2)$ | $O(1)$ | No |
| Insertion | $O(n)$ | $O(n^2)$ | $O(n^2)$ | $O(1)$ | Yes |
| Merge | $O(n \log n)$ | $O(n \log n)$ | $O(n \log n)$ | $O(n)$ | Yes |
| Quick | $O(n \log n)$ | $O(n \log n)$ | $O(n^2)$ | $O(\log n)$ | No |
| Heap | $O(n \log n)$ | $O(n \log n)$ | $O(n \log n)$ | $O(1)$ | No |

**Key Patterns:** [[01-Sorting-Patterns#Bubble Sort|Bubble sort]], [[01-Sorting-Patterns#Selection Sort|Selection sort]], [[01-Sorting-Patterns#Insertion Sort|Insertion sort]], [[01-Sorting-Patterns#Merge Sort|Merge sort]], [[01-Sorting-Patterns#Quick Sort|Quick sort]], [[01-Sorting-Patterns#Heap Sort|Heap sort]], [[01-Sorting-Patterns#Built-in Sort|Built-in sort usage]]

**Applications:** Data processing and reporting, binary search prerequisite, database ORDER BY operations, finding duplicates efficiently.

---
**See also:** [[../02-Searching/Searching.md|Searching]], [[../06-Divide-Conquer/Divide-Conquer.md|Divide & Conquer]]
