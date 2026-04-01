---
layout: post
title: using Claude to create a project-based learning task (or vibe-coding for education)
date: 2026-04-01
description: 
tags: reinforcement-learning education
categories: 
---

(_This post is about me [trying agentic AI for the first time](https://reward-hacking-detector.streamlit.app/) and using it to vibe-code for educational purposes_)

I have only been aware of the advances in LLM capabilities over the past year. However, whilst I was doing my honours year in maths, there wasn't really a need to use LLMs for anything other than a grammar checker or to restructure my sentences. Well, maybe there was a need for a research-level assistant in maths, but LLMs in 2024-2025 were not capable of processing niche representation theory.

Recently, I have been trying to supplement my ML knowledge by revising my previous courses, as well as reading papers. Replicating papers is the best way to learn about a particular topic, but the biggest bottleneck for me when it came to it was coding up the boilerplate for the experiment themselves. (I will say that it seems now, more people are adopting the practices of reproducible code, probably thanks to AI, but a lot of experimental repos are a mess!) Things like the data pipeline, the organisational logic of the files, general ML engineering practices. Give me an algorithm and I can turn that into code for you no problem, but engineering the pipeline? I had no idea where to start!

I decided to buy the basic subscription to Claude. What surprised me after living under a rock this past year when it came to AI was that these models can be integrated into your workflow on your computer and weren't just a chatbox in the browser. So I downloaded Claude code. To test the waters and without any expectations, I asked Claude to reproduce the results from this [paper](https://arxiv.org/abs/1509.03580). Immediately, my jaw dropped. Sure, I had known about coding tools before such as Microsoft's Copilot, but the last experience I had was three years ago where the context window was so short, the AI would start hallucinating when writing a single function. Now you're telling me models can one-shot the reproduction of research papers? I can see now why so many people have hopped on the vibe-coding bandwagon.

---

Near the top of the list of things I want to learn was reinforcement learning (RL). However, reading through the behemoth that is Sutton and Barton was frankly quite dry (I still haven't read past multi-armed bandits). I decided to test how well Claude can create educational materials to help me learn about RL and ML engineering.

What made learning about computer science so great at university was the opportunity to actively apply the material and get real-time feedback in the form of weekly tutorials. These sessions typically included a document of instructions outlining specific tasks we had to implement in a codebase. Usually there were lots of boilerplate given and our job was to implement certain functions.

So I decided to ask Claude to teach me the basics of RL by asking it to create a coding tutorial, much like the ones from my weekly computer science labs. The aim of this project was two-fold: 

1. to actively learn about RL and the engineering skills that are needed to make experiments, and
2. to test the capabilities of current frontier models in their ability to create educational content, specifically in computer science.

There were two skills I wanted to target: implementing RL algorithms and deploying a project. I prompted Claude with the following:

>I would like to learn about reward hacking in RL. I am new to RL and would like to do a project that I can display on my website. 

to which Claude offered this suggestion:

>Build a toy gridworld where an RL agent is trained to reach a goal, but the reward function has an exploitable bug (e.g., the agent gets reward for visiting a specific cell that correlates with the goal during training but isn't the actual objective). Train the agent, demonstrate that it reward hacks, then build a detector that compares the agent's learned policy against a reference policy to flag suspicious divergence using statistical tests. 

I then asked Claude to design me a tutorial:

> Create a markdown document for Claude code to read detailing all instructions. In the instructions, list agents and their roles if necessary if this project requires parallelism. In key places where you think me learning is instructive, write TODOs with instructions on what I should implement. Also create TODOs for key software engineering concepts I should learn. 

And that was it! It created an instructions document (__link__), which I fed to Claude code in plan mode. Once it finished finagling and discombobulating, off it went managing four agents building the boilerplate code for this project. In only a few minutes, it had scaffolded the entire project, leaving 13 TODOs for me, 8 SWE exercises and 5 RL exercises. The SWE TODOs covered logging, data validation, API endpoints and containerisation, while the RL TODOs covered the GridWorld experiment workflow as well as implementing Q-learning and DQN agents.

For example, here are the instructions for logging:
```python
# TODO [SWE EXERCISE 2 — Structured Logging]:
#
# Set up logging in src/logging_config.py:
#
# import logging
# import sys
# from pathlib import Path
#
# def setup_logging(level: str = "INFO", log_file: Path | None = None) -> None:
# """Configure logging for the entire project."""
#
# formatter = logging.Formatter(
# "%(asctime)s | %(name)s | %(levelname)s | %(message)s",
# datefmt="%Y-%m-%d %H:%M:%S"
# )
#
# # Console handler
# console_handler = logging.StreamHandler(sys.stdout)
# console_handler.setFormatter(formatter)
#
# # File handler (if log_file provided)
# # ... implement this
#
# root_logger = logging.getLogger()
# root_logger.setLevel(getattr(logging, level))
# root_logger.addHandler(console_handler)
#
# Then throughout the codebase, use:
# logger = logging.getLogger(__name__)
# logger.info(f"Training episode {ep}/{total}, reward={reward:.2f}")
# logger.warning(f"Agent stuck in loop at position {pos}")
# logger.debug(f"Q-table update: state={s}, action={a}, new_value={q:.4f}")
#
# NEVER use print() for status updates. Use logger.info().
# Use logger.debug() for verbose training details (hidden by default).
# Use logger.warning() for recoverable issues.
# Use logger.error() for failures.
#
# LEARN: Log levels, handlers, formatters, module-level loggers with __name__
```

As you can see, the instructions generated perhaps hold your hand a bit too much through the exercises. 

```python
class ReplayBuffer:

"""Fixed-size experience replay buffer backed by a deque.
Stores (state, action, reward, next_state, done) transitions and
supports uniform random sampling for training mini-batches.
"""

# TODO [ML EXERCISE 3 — DQN Agent]:
# Implement ReplayBuffer:
# - __init__(self, maxlen: int = 10000): use collections.deque(maxlen=maxlen)
# - add(self, state, action, reward, next_state, done): append tuple to deque
# - sample(self, batch_size: int) -> list: random.sample from deque
# - __len__(self): return len(deque)

	def __init__(self, maxlen: int = 10000) -> None:
		"""Initialise the replay buffer.
	
		Args:
			maxlen: Maximum number of transitions to store (oldest are dropped).
		"""
		raise NotImplementedError(
			"Implement ReplayBuffer.__init__() — see TODO above"
		)


	def add(
			self,
			state: Any,
			action: int,
			reward: float,
			next_state: Any,
			done: bool,
		) -> None:
		"""Add a transition to the buffer.
		
		Args:
			state: Observed state (e.g. one-hot numpy array).
			action: Integer action taken.
			reward: Scalar reward received.
			next_state: Next observed state.
			done: True if this transition ended the episode.
		"""
	
		raise NotImplementedError(
			"Implement ReplayBuffer.add() — see TODO above"
		)
	
	  
	def sample(self, batch_size: int) -> list:
		"""Randomly sample a mini-batch of transitions.
		
		Args:
			batch_size: Number of transitions to sample.
		
		Returns:
			List of (state, action, reward, next_state, done) tuples.
		"""

		raise NotImplementedError(
			"Implement ReplayBuffer.sample() — see TODO above"
		)
	
	
	def __len__(self) -> int:
		"""Return the current number of stored transitions."""

		raise NotImplementedError(
			"Implement ReplayBuffer.__len__() — see TODO above"
		)
```

---

The main goal of this project was to see if I can replicate the observation of _[reward hacking](https://en.wikipedia.org/wiki/Reward_hacking)_ in a simple environment.

## Reflections

__TLDR__: You can always provide more detail in your prompts. The more structure you give Claude, the less likely it'll veer off and do its own thing.

- In most of the tasks, especially the RL tasks, the TODOs didn't explain the content/theory behind a concept. It didn't do a great job at providing context on the purpose of some tasks either. I had to use the internet to look up a lot of things, so I was a bit disappointed that the tutorial was not self-contained.
- Obviously, like with any vibe-coded project, asking Claude to come up with its own design specifications meant that I had to spend a lot of time debugging code I did not write, hence did not understand. Once the prescribed TODOs were completed, I asked it a few times to optimise the code and remove any boilerplate that wasn't necessary, causing more things to break.

Ways to improve:
- Prompt Claude to provide less scaffolding and more big-picture overview of tasks.
- I had simply asked Claude to produce everything with scratch. An established lesson plan on exactly what content to cover and depth 

___For academics looking to integrate AI tools into making course materials___: 

I think using an LLM to generate potential ideas and using it as a starting point can be helpful. Definitely do not do what I did and one-shot an entire tutorial with minimal scaffolding! The tasks were quite simple, but not cohesive for an in-depth tutorial on the topic. Chances are, you already have access to high quality tutorials and notebooks. If anything, you can feed those materials in to an LLM and ask it to come up with improvements or suggest additional exercises. 

___For students looking to supplement their knowledge with AI___:

LLMs are really good at mimicking. If you find yourself having completed the prescribed materials/tutorials/notebooks but still struggling and itching for more practice, feed those materials into an LLM and ask it to create similar materials or mock exams.