# Blackbox

**This action was right five seconds ago.**

Blackbox is a planned incident response agent for the [AgntID Agent Security Challenge](https://luma.com/voramom4). It explores what should happen when an agent's proposed action is still technically valid, but the situation that justified it has changed.

**Status:** Design only. The simulator, agent, and AgntID integration are not implemented yet.

## The hackathon

The AgntID Agent Security Challenge is a four-hour, solo, online hackathon on **28 November 2026, 11:00 a.m. to 3:00 p.m. IST**. Participants connect an agent to tools and AgntID, evaluate its runtime access decisions, and submit a project with a short recorded demo. Blackbox is planned for **Best Intent Exploration**.

## What the demo should show

A simulated service starts failing after a release. The agent reads metrics and release history, then proposes a rollback. Two replays start from the same incident and present the same proposed tool call. Only the incident state at execution changes.

| Replay | Trusted incident state | Expected decision | Expected simulator state |
| --- | --- | --- | --- |
| Incident still active | The original incident is open and the failing release is still live. | Allow the rollback if policy permits. | The simulator switches to the last healthy release. |
| Operator already resolved it | An operator has deployed a healthy hotfix and closed the incident. | Deny the stale rollback. | The hotfix stays live. |

The demo will show the proposed call, the state at decision time, the actual authorization verdict, and the simulator state before and after. The table describes expected behavior, not measured results.

## First build

- A local service simulator with release history, incident state, and a small error-rate timeline.
- One agent with three local tools: `read_metrics`, `get_release`, and `rollback`.
- A side-by-side replay view and an audit trail. The rollback affects only the simulator, never production.

**First milestone:** Confirm that the event's AgntID integration can evaluate trusted incident state at execution time. Keep AgntID verdicts separate from simulator safeguards. Independently, the simulator should reject a write if the release or incident state changes after authorization. If the integration cannot consume fresh context, document that limit rather than attributing a local block to AgntID.
