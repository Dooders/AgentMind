# Agent Core Module Documentation

## Overview

The **Agent Core** is the central control module responsible for coordinating perception, memory, reasoning, inhibition, and willpower modules to determine and execute an agent's action at each simulation step. It maintains internal state consistency, logs decisions, and provides a unified decision-making interface via the `select_action()` method.

---

## Core Responsibilities

* **Execution loop**: Manages the agent's step-by-step operation.
* **Module coordination**: Ensures perception, memory, reasoning, inhibition, and willpower modules communicate effectively.
* **Logging**: Centralizes structured logs for decisions, states, and outcomes.
* **Decision resolution**: Combines outputs from reasoning, inhibition, and willpower to select and execute the final action.

---

## Public Methods

### `step(observation)`

Processes a single simulation timestep.

**Arguments**:

* `observation` (dict): Current environmental input for the agent.

**Returns**:

* `final_action` (str): The selected action after processing all modules.

### `select_action(proposals, inhibition_mask, willpower_weights)`

Combines action proposals, inhibition, and willpower to select the most appropriate action.

**Arguments**:

* `proposals` (dict): Action candidates from ReasoningModule with scores, e.g., `{"move": 0.8, "wait": 0.2}`
* `inhibition_mask` (dict): Boolean mask `{action: bool}` indicating which actions are allowed.
* `willpower_weights` (dict): Scalar weights modifying the influence of each action.

**Returns**:

* `action` (str): Chosen action based on scoring and constraints.

### `default_action()`

Fallback action used when all proposals are inhibited.

**Returns**:

* `action` (str): Default action string, e.g., "wait"

---

## Module Integration Workflow

### Step-by-Step:

1. **Perception**: Update the internal state from the latest observation.
2. **Memory**: Update or retrieve relevant episodic and semantic data.
3. **Reasoning**: Generate a set of proposed actions with associated confidence scores.
4. **Inhibition**: Evaluate each proposed action and mask any that should be suppressed.
5. **Willpower**: Apply motivational bias to each action, adjusting its priority.
6. **Action Selection**: Use `select_action()` to combine the outputs and choose the final action.
7. **Execution**: Log and return the action.

---

## Example Implementation Skeleton

```python
class AgentCore:
    def step(self, observation):
        self.perception.update(observation)
        self.memory.update_context(self.perception.state)

        proposals = self.reasoning.propose_action(self.perception.state, self.memory)
        inhibition_mask = self.inhibition.evaluate(proposals, self.perception.state)
        willpower_weights = self.willpower.bias(proposals, self.memory)

        final_action = self.select_action(proposals, inhibition_mask, willpower_weights)
        self.log_decision(observation, proposals, inhibition_mask, willpower_weights, final_action)
        return final_action

    def select_action(self, proposals, inhibition_mask, willpower_weights):
        scored = {
            action: score * willpower_weights.get(action, 1.0)
            for action, score in proposals.items()
            if inhibition_mask.get(action, True)
        }
        if not scored:
            return self.default_action()
        return max(scored, key=scored.get)

    def default_action(self):
        return "wait"

    def log_decision(self, observation, proposals, inhibition_mask, willpower_weights, action):
        # Structured logging for audit and training
        pass
```

---

## Design Notes

* **Modularity**: Each cognitive function (reasoning, inhibition, willpower) is encapsulated in its own module.
* **Interpretability**: Logging each module's contribution supports traceability and debugging.
* **Extensibility**: New modules (e.g., emotion, planning, meta-reasoning) can be added without changing the core interface.

---

## Future Extensions

* Priority queuing for multi-step plans
* State-based emotional salience module
* Adaptive select\_action strategies based on context
* Reinforcement learning of module weights or final action scoring function

---

## Dependencies

* `PerceptionModule`
* `MemoryModule`
* `ReasoningModule`
* `InhibitionModule`
* `WillpowerModule`

Each must follow a clearly defined interface contract to remain interoperable with `AgentCore`.
