<div align="center">

# 🌍 GEM: A Gym for Agentic LLMs

</div>

## Overview

We’re entering the **era of experience**, where large language models (LLMs) learn not just from static datasets, but from *interactive experience* gathered in complex, expressive environments.

As a step toward this, we introduce **GEM** — a **G**eneral **E**xperience **M**aker for LLMs — an open-source environment suite designed for training *agentic LLMs* via online reinforcement learning.

Like [OpenAI Gym](https://github.com/openai/gym) for traditional RL, GEM provides a standardized API and a growing collection of diverse environments. It is **training framework-agnostic** and supports seamless integration with six popular RL training frameworks including [Oat](https://github.com/sail-sg/oat) and [Tinker](https://github.com/thinking-machines-lab/tinker), offering:

* 🧩 Clean, composable environment APIs
* ⚙️ Async vectorized execution for high-throughput simulation
* 🔧 Tool integration & custom wrappers
* 🧠 Multi-environment training
* 🎈 Ready-to-use benchmark environments and algorithms


## Installation

install from source for the latest version:

```bash
git clone <this repo> gem
cd gem
pip install -e .
```

Please check [Getting Started](./GETTING_STARTED.md) for more setup details.

🔥 You can jump into [examples](./examples/) to quickly start your agentic RL training with GEM & your favorite training framework.

## Interface
GEM's interface closely follows OpenAI-Gym's API. Here's an example using the `game:GuessTheNumber-v0` environment:

```python
import gem

# List all supported environments
gem.print_envs()

# Initialize the environment
env = gem.make("game:GuessTheNumber-v0")

# Reset the environment to generate the first observation
observation, info = env.reset()

# Start the agent-environment loop
while True:
    action = env.sample_random_action() # insert policy here, e.g.,
    # (pseudocode) action = llm.generate(observation)

    # apply action and receive next observation, reward
    # and whether the episode has ended
    next_observation, reward, terminated, truncated, info = env.step(action)
    print("OBS", observation)
    print("ACT", action)

    # update the policy (online) here
    # e.g., policy = learn(policy, observation, action, reward, info)

    observation = next_observation
    # Exit when the episode terminates
    if terminated or truncated:
        break
```

## Features

1. Environments consist of tasks and (optional) tools. Tool-calling is achieved via an environment wrapper, as demonstrated [here](./GETTING_STARTED.md#tool-integration-examples).
2. GEM is training framework-agnostic, and we demonstrate its integration with six popular RL training frameworks.
3. We provide implementations and benchmarking results for different algorithms across a diverse set of environments.

### Supported Tasks

<div align="center">

| Category                   | Example Environments                              | Description                                      |
| -------------------------- | ------------------------------------------------- | ------------------------------------------------ |
| **Games**                  | `game:GuessTheNumber-v0-hard`, `game:Sudoku-v0-easy`        | Classic language games   |
| **Math**           | `math:Math12K`, `math:DeepScaleR40K`        | Mathematical reasoning |
| **Code**           | `code:CodeContest`, `code:Taco8k`        | Competitive coding |
| **QA**           | `qa:NaturalQuestions`, `qa:HotpotQA`            | Knowledge-intensive question answering             |
| **ReasoningGym**   | `rg:arc_1d`, `rg:letter_counting`           | Diverse synthetic reasoning tasks       |

</div>

### Supported Tools

<div align="center">

| Tool                            | Description                                      |
| -------------------------- | ------------------------------------------------ |
| **Python**                         | Python code executor that parses code blocks, executes them, and returns outputs   |
| **Search**                | Calls a search engine to retrieve documents for any query
| **MCP**            | Calls the general MCP API to train tool-use agents |

</div>


### Supported Frameworks

<div align="center">

| Framework                            | Description                                      |
| -------------------------- | ------------------------------------------------ |
| **[Oat](https://github.com/sail-sg/oat)**                         | vLLM + DeepSpeed, modular, no ray   |
| **[Tinker](https://github.com/thinking-machines-lab/tinker)**                | SDK provided by Thinking Machines, frees you from infra issues |
| **[Verl](https://github.com/volcengine/verl)**            | Support diverse backends, models, and algorithms |
| **[RL2](https://github.com/ChenmienTan/RL2)**            | SGLang + FSDP, no ray, easy to hack |
| **[ROLL](https://github.com/alibaba/ROLL)**            | Support diverse backends, models, and algorithms |
| **[OpenRLHF](https://github.com/alibaba/ROLL)**            | Support diverse backends, models, and algorithms |

</div>

Examples of training agents on GEM environments with all above frameworks can be found in [here](./examples/)!


### Supported Algorithms

<div align="center">

| Algorithm                            | Description                                      |
| -------------------------- | ------------------------------------------------ |
| **REINFORCE**                         | A general policy gradient algorithm that can be applied to single- and multi-turn environments |
| **GRPO**                | Mainly for bandits (single-turn), using group advantage normalization |
| **PPO**            | Learns a turn-level critic to compute generalized advantage estimation (GAE) |
| **REINFORCE + ReBN**            | REINFORCE with return batch normalization as introduced in our paper |

</div>

Please check out our paper for a more detailed description for each algorithm and empirical results showing their tradeoffs.
