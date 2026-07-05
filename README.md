# symbolic-rl-predicate-agent
Hybrid reinforcement learning agent with automatic predicate generation, rule mining, teacher demonstrations and Q-learning.
# Symbolic RL Predicate Agent

Hybrid reinforcement learning project that combines coordinate-based state representation, automatically generated derived predicates, teacher demonstrations, rule mining, and Q-learning.

The goal of the project is to move from raw coordinates to meaningful world relations such as distance, direction, object proximity, action possibility, and causal effects.

## Main Idea

A classic Q-learning agent usually works with raw states:

```python
agent_x = 2
agent_y = 3
key_x = 2
key_y = 1
```

This project adds a predicate generation layer that transforms raw coordinates into semantic features:

```python
key_close = True
key_same_col = True
key_is_up = True
distance_to_key = 2
```

These generated predicates make it easier to mine rules such as:

```text
IF key_in_front = True
ACTION pickup
THEN has_key = True
```

or:

```text
IF has_key = True AND door_in_front = True
ACTION toggle
THEN door_open = True
```

## What the Project Does

The system:

1. Builds a base coordinate state from the environment.
2. Automatically generates derived predicates from object positions.
3. Collects teacher demonstrations using a navigator.
4. Stores transitions in experience memory.
5. Mines symbolic rules from before/action/after transitions.
6. Pretrains a Q-learning agent on collected experience.
7. Tests the trained agent.
8. Saves the learned Q-table and discovered rules.

## Architecture

```text
Environment
    ↓
Base State Builder
    ↓
Predicate Generator
    ↓
Extended State
    ↓
Teacher Navigator
    ↓
Experience Memory
    ↓
Rule Miner
    ↓
Q-Learning Agent
```

## Predicate Generator

The `PredicateGenerator` automatically computes spatial and strategic predicates:

```python
distance_to_key
key_close
key_very_close
key_same_row
key_same_col
key_is_left
key_is_right
key_is_up
key_is_down
key_in_front
can_pickup_key
can_open_door
can_finish
```

These features are not manually assigned for every state. The system computes them from the current coordinates of the agent, key, door, and goal.

## Rule Mining

The rule miner analyzes stored transitions:

```text
before_state → action → after_state
```

and extracts patterns such as:

```text
IF can_pickup_key = True
ACTION pickup
THEN has_key = True
```

This allows the system to discover useful action-effect relations from experience.

## Reinforcement Learning

The project uses Q-learning with pretraining from teacher memory. Instead of learning only through random exploration, the agent first learns from collected expert trajectories.

This makes the learning process more structured and allows the agent to benefit from both:

* symbolic predicates;
* reinforcement learning updates.

## Example Output

Example discovered rule:

```text
IF:
    can_open_door = True

ACTION:
    toggle

THEN:
    door_open = True

support: 24
confidence: 1.0
reward_mean: 0.8
```

## Why This Project Is Interesting

This project explores the connection between symbolic reasoning and reinforcement learning.

Instead of treating the environment as only a set of raw coordinates, the agent receives meaningful relational features:

* what is close;
* what is in front;
* what direction an object is in;
* what action is currently possible;
* what changes after an action.

This makes the agent more interpretable and brings the system closer to model-based reasoning.

## Technologies

* Python
* NumPy
* Gymnasium / MiniGrid-style environment
* Q-learning
* Rule mining
* Symbolic predicates
* Teacher demonstrations

## Project Status

Prototype / research experiment.

Current focus:

* derived predicate generation;
* rule extraction from experience;
* Q-learning pretraining;
* symbolic interpretation of agent behavior.

Future improvements:

* automatic object property discovery;
* causal graph construction;
* planning based on mined rules;
* generalization to new objects and environments.
