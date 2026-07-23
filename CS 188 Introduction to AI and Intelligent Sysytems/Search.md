## Reflex Agent

- Reflex agents:
	- Choose actions based on current percept (and maybe memory).
	- May have memory or model of world's current state.
	- Do not consider the future consequences of their actions.
	- **Consider How world IS.**

- Can a reflex agent be rational ?
	No
## Planning Agents

- Planning agents:
	- Asks "What if?".
	- Decisions based on (hypothesized) consequences of actions.
	- Must have a model how the world evolves in response to actions.
	- Must formulate a goal (test).
	- **Consider how the world would be**.

- Optimal vs complete planning
- Planning vs replanning


## Search Problems

- A search problem consists of:
	- A state space
	- A successor function (with actions, cost)
	- A start state and a goal test.
- A solution is a sequence of actions (a plan) which transforms the start state to a goal state.

Search problems are models

### What is in a state space?

- The world states includes every last detail of the environment.
- A search state keeps only the details needed for planning (Abstraction).
	- Problem : Pathing
		- States: (x ,y) location.
		- Actions : Next state where you are
		- Successor: Updates location only.
		- Goal test : is (x ,y)=END.

## State Space Graphs and Search Trees.

 - State space graph : A mathematical representation of a search problem.
	 - Nodes are (abstracted) world configs.
	 - Arcs represent successors (action results).
	 - The goal test is a set of goal nodes (maybe only one).