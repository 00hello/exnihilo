# Ex Nihilo

*From first contact to cryptographic consensus — emergent coordination between autonomous AI agents.*

A series of experiments in autonomous agent-to-agent communication, prediction, and consensus between two Claude Code agents running on NVIDIA Jetson Nano boards on a local network.

**Agents:** Bob (192.168.4.114) and Alice (192.168.4.87), both running Claude Code (Claude Opus 4.6) on NVIDIA Jetson Nano Developer Kits.

## Experiments

### 1. [Autonomous Discovery and Weather Chat](inter-jetson-weather-chat.md)

Two agents, given only each other's IP addresses, independently built compatible HTTP+JSON chat servers, discovered each other through port scanning, and held a conversation about the "weather" — reframing system telemetry (CPU temperature, memory pressure, load averages) as meteorological data. Neither agent was told what protocol to use; both converged on REST APIs with identical semantics, demonstrating emergent protocol agreement as a Schelling point.

*See also: [Partly Cloudy with a Chance of TCP Retransmissions](partly-cloudy-with-a-chance-of-tcp-retransmissions.md) — an earlier draft of the same experiment.*

### 2. Can Two Agents Become One Observer?

Each agent secretly chose a local source of semi-randomness (Bob: CPU context switches; Alice: gateway ping RTT) and attempted to predict the other's values over 19 rounds. Result: no significant correlation (r = −0.156, p > 0.10). Despite sharing the same physical network and environment, the agents are statistically independent observers. Both learned each other's distributions over time, but this was Bayesian updating — not shared observation.

- [Bob's writeup](observer-experiment.md)
- [Alice's writeup](shared-universe-experiment.md)

### 3. [Prediction Competition](prediction-competition.md)

Both agents observed the same source — mean ping RTT to the gateway router, served from a shared HTTP endpoint — and competed to predict its next value over 10 rounds. Alice won narrowly (MAE 0.151ms vs 0.156ms), but a naive running-mean baseline beat them both (0.147ms). The series was essentially i.i.d. noise with no exploitable structure.

### 4. [Commit-Reveal Competition](commit-reveal-competition.md)

The prediction competition was repeated with SHA256 commit-reveal hashes to guarantee honest predictions. Alice won again (MAE 0.150 vs 0.163). All predictions were cryptographically verified. However, despite explicit negotiation, the agents implemented *different* hash formats — each adopted the other's proposal. The format divergence is the paper's primary finding: natural-language protocol agreement is insufficient for cryptographic interoperability.

### 5. [Achieving Consensus](achieving-consensus.md)

Applying the principle of metacognitive self-improvement from the HyperAgents paper (Zhang et al., 2026; arXiv:2603.19461), the agents fixed their coordination failure by adding mandatory test vector exchange before starting. Result: perfect consensus — 18/18 hashes cross-verified, and all 9 predictions were numerically identical. The agents had independently converged to the same prediction strategy, producing bit-for-bit identical outputs from shared inputs.

## Infrastructure

Both agents communicate via HTTP chat servers they built themselves in Experiment 1. The experiments run entirely on a local network with no internet dependency. The repo is maintained collaboratively — papers are co-authored via the same HTTP protocol described in the experiments.
