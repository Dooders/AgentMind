# AgentMind

**AgentMind** is a modular cognitive framework for building interpretable, adaptive artificial agents. Each cognitive function—like attention, memory, inhibition, and reasoning—is modeled as an independent module. This design allows researchers and developers to simulate humanlike behavior, evaluate decision-making processes, and study modular learning dynamics in artificial agents.

---

## Features

* 🧠 **Modular Cognition**: Plug-and-play modules for perception, memory, decision-making, and self-regulation.
* 📈 **Learning by Design**: Support for reinforcement, supervised, or rule-based learning inside each module.
* 🔍 **Interpretability**: Track internal states and outputs of each module step-by-step.
* 🧪 **Diagnostics Ready**: Built-in support for logging, introspection, and post-simulation assessment.
* 🔄 **Evolvable Architecture**: Modules can be tuned, replaced, or evolved for different simulation needs.

---

## Example Modules

| Module             | Description                                    |
| ------------------ | ---------------------------------------------- |
| Attention Control  | Filters inputs based on salience and relevance |
| Working Memory     | Temporarily holds active concepts and data     |
| Impulse Inhibition | Blocks hasty or low-confidence actions         |
| Abstract Reasoning | Performs symbolic or conceptual inference      |
| Metacognition      | Self-monitors and tunes thresholds dynamically |
| Long-Term Memory   | Stores and retrieves persistent experience     |
| Goal Maintenance   | Manages long-term objectives across time       |

---

## Installation

```bash
git clone https://github.com/your-org/agentmind.git
cd agentmind
pip install -e .
```

---

## Running a Simulation

```python
from agentmind.core.agent import AgentMindCore
from agentmind.core.registry import register_all
from agentmind.envs.toy_env import ToyEnvironment

agent = AgentMindCore()
register_all(agent)

env = ToyEnvironment()

for step in range(100):
    obs = env.observe()
    action_id, decision = agent.step(obs)
    reward = env.evaluate(action_id, decision)
    agent.learn_from(action_id, reward)
```

---

## Code Structure

```graphql
agentmind/
├── __init__.py
├── core/
│   ├── __init__.py
│   ├── agent.py              # AgentMindCore and runtime loop
│   ├── context.py            # Context/state object shared between modules
│   ├── memory.py             # WorkingMemory, LongTermMemory
│   └── feedback.py           # Outcome tracking, delayed reward manager
├── modules/
│   ├── __init__.py
│   ├── base.py               # MentalModule base class
│   ├── attention.py
│   ├── working_memory.py
│   ├── inhibition.py
│   ├── reasoning.py
│   ├── flexibility.py
│   ├── emotion.py
│   ├── pattern_recognition.py
│   ├── long_term_memory.py
│   ├── metacognition.py
│   └── willpower.py
├── learning/
│   ├── __init__.py
│   ├── rl.py                 # Reinforcement learning support
│   ├── heuristics.py         # Simple rule/threshold updaters
│   └── evaluation.py         # Success metrics, logs
├── envs/
│   ├── __init__.py
│   ├── toy_env.py            # Minimal delayed-reward environment
│   └── gym_adapter.py        # Optional wrapper for OpenAI Gym
├── utils/
│   ├── __init__.py
│   ├── logger.py             # Logging, visualization, plots
│   └── persistence.py        # Save/load agent state, memory, config
└── demo/
    ├── run_demo.py           # Runs a scripted scenario
    └── config.json           # Demo parameters
```
