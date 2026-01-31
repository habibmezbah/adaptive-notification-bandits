# Adaptive Notification Scheduling (A/B Testing vs Bandits)

## Overview

This repository contains a **simulation-based learning project** that compares **A/B testing** and **multi-armed bandit algorithms** for **adaptive notification scheduling** under **user fatigue**.

The goal of this project is to **understand and experiment with core concepts** in:

* online decision-making
* experimentation (A/B testing)
* multi-armed bandits
* simulation-based evaluation

by applying them to a realistic product scenario where user behavior evolves over time.

---

## Motivation

Push notifications are widely used to drive user engagement in modern applications.
However, sending notifications too frequently can lead to **notification fatigue**, reducing long-term engagement and increasing the risk of **user opt-outs**.

Two common approaches are used in practice:

* **A/B testing**

  * Statistically clean and unbiased
  * Slow to adapt and inefficient for short-term reward

* **Multi-armed bandits**

  * Adaptively allocate traffic to promising strategies
  * Optimize reward online but may behave poorly in non-stationary environments

This project explores the **trade-offs between these approaches** in a controlled simulation where:

* rewards are stochastic
* user preferences are heterogeneous
* actions have delayed negative effects

---

## Problem Setup

We simulate a platform that sends **at most one notification per user per day**.

Each notification is defined by:

* **Time slot**: `9am`, `1pm`, `6pm`, `9pm`
* **Message type**: `short`, `motivational`, `detailed`

Each `(time slot, message type)` pair corresponds to a **bandit arm**.

### Objectives

The system aims to:

* **maximize user engagement** (clicks)

while controlling:

* **notification fatigue**
* **user opt-out rate**
* **long-term user retention**

---

## Simulation Environment

Since real user data is unavailable, a **custom simulator** is built to model realistic user behavior.

### Users

* Each user belongs to a **latent (hidden) type**
* User type influences:

  * preferred notification time
  * baseline engagement probability
* User types are **not observable** by learning algorithms

### Reward Model

Click probability is modeled using a logistic function:

[
P(\text{click}) = \sigma(
\text{base}_{type}

* \text{time_preference}_{type}
* \text{message_effect}

- \lambda \cdot \text{fatigue}
  )
  ]

where:

* σ is the sigmoid function
* fatigue increases with notifications and decays over time
* rewards are binary (click / no click)

### Fatigue & Opt-Out Dynamics

* Every notification increases user fatigue
* Fatigue:

  * lowers future click probability
  * increases opt-out probability
* Opt-outs are **permanent**

This introduces:

* delayed effects
* non-stationarity
* dependence between rewards over time

---

## Methods

All methods operate on the **same simulated environment** for fair comparison.

### A/B Testing (Baseline)

* Two fixed notification strategies are selected upfront
* Users are randomly split into groups with fixed allocation
* Performance evaluated using:

  * click-through rates
  * two-proportion z-test
  * cumulative reward and regret

A/B testing prioritizes **statistical validity** over reward optimization.

---

### Multi-Armed Bandit Algorithms

The following bandit algorithms are implemented:

#### ε-Greedy

* Exploits the best-known arm with probability `1 − ε`
* Explores randomly with probability `ε`

#### UCB1

* Selects arms using an upper confidence bound:
  [
  \hat{\mu}_i + \sqrt{\frac{2 \log t}{n_i}}
  ]
* Encodes optimism under uncertainty

#### Thompson Sampling

* Maintains Bayesian posteriors over arm rewards
* Selects arms by sampling from the posterior

All bandits update **online after each interaction**.

---

## Evaluation Metrics

Performance is evaluated using:

* **Cumulative reward** (total clicks)
* **Cumulative regret** relative to an oracle with perfect knowledge
* **Number of opt-outs**
* **Active user count over time**

These metrics capture both **short-term performance** and **long-term user health**.

---

## Results (Summary)

Key qualitative observations:

* Bandit algorithms achieve higher early engagement due to adaptive allocation
* Thompson Sampling generally shows lower regret than ε-greedy and UCB
* Aggressive exploitation can increase opt-outs under fatigue
* A/B testing is conservative but more stable in preserving users

(Plots and detailed analysis are available in the notebook.)

---

## Limitations

* The simulator is a simplified abstraction of real human behavior
* User preferences and fatigue dynamics are assumed, not learned
* Contextual information (e.g., user history) is not explicitly used

---

## Future Work

Possible extensions include:

* contextual bandits using user state features
* explicit fatigue-aware constraints
* change-point detection for non-stationarity
* hybrid systems combining A/B testing and bandits

---

## Purpose of This Project

This project is **educational in nature** and was built to:

* deeply understand bandits, A/B testing, and simulation
* study their behavior in realistic, non-ideal settings
* practice principled experimental evaluation

It is **not intended as a production system**, but as a learning and exploration framework.

---

## Tech Stack

* Python
* NumPy
* Matplotlib
* Google Colab

---

## How to Run

1. Open the notebook in Google Colab
2. Run all cells sequentially
3. Inspect plots and printed metrics for comparisons

---

## Key Takeaway

Adaptive algorithms are powerful, but **real-world feedback loops matter**.
Simulation provides a safe way to explore these trade-offs before deployment.

I did this project just to learn the concepts of Bandits algorithm, Simulations, A/B testing. The best way to learn is to implement. (Also solving a real problem gives you a dopamine boost too).

---






