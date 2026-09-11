Search is the foundation of AI planning — given a start state and a goal, find a sequence of actions that gets you there. It starts simple (reflex agents that just react) and builds up to planning agents that think ahead.

**The Intuition:** A reflex agent is like a person who only reacts to what's in front of them — they don't plan. A planning agent is like someone using a GPS: they consider "what if I take this road?" and evaluate consequences before moving. Search is the formal framework for that "what if?" reasoning.

**The Math:**

**Reflex Agents:**
- Choose actions based on current percept (and maybe memory)
- May have a model of the world's current state
- Do NOT consider future consequences
- Consider how the world IS
- Cannot be rational (no lookahead)

**Planning Agents:**
- Ask "What if?" — decisions based on consequences of actions
- Must have a model of how the world evolves
- Must formulate a goal (test)
- Consider how the world WOULD BE
- Can be optimal or complete; can plan or replan

**Search Problem Components:**
- **State space** — every detail of the environment (abstracted for planning)
- **Successor function** — actions and their costs
- **Start state** and **goal test**
- **Solution** — a sequence of actions (plan) from start to goal

**State Space Graph:** Nodes are abstracted world configurations, arcs represent successor actions, goal nodes form the goal test set.

**Example — Pathing:**
- States: $(x, y)$ location
- Actions: move to adjacent cells
- Successor: updates location
- Goal test: is $(x, y) = \text{END}$?
