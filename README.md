# joedevflow

`joedevflow` is a development workflow skill for coding agents.

It is built around a simple premise: day-to-day software engineering work gets better when concerns are separated clearly. Planning, testing, implementation, and maintenance are related, but they are not the same activity. This skill makes an agent operate in one mode at a time so each part of the work has a clear purpose, input, output, and stopping point.

TDD sits at the center of the workflow. The agent designs first, writes failing tests before implementation, then writes only the code needed to make those tests pass. After that, it observes, debugs, verifies, and maintains the system with regression coverage.

## Workflow

```mermaid
flowchart TD
    A["Mode 1<br/>Design"] --> B["Mode 2<br/>Test (Red)"]
    B --> C["Mode 3<br/>Implement (Green)"]
    C --> D["Mode 4<br/>Observe, Debug & Maintain"]
    D --> A

    A1["Clarify why, what, and how<br/>No implementation code"] --> A
    B1["Write tests that fail for the right reason<br/>No production code"] --> B
    C1["Make the red tests pass<br/>Do not edit the tests"] --> C
    D1["Verify, cover, debug, and preserve behavior<br/>Regression tests before fixes"] --> D
```

## The Four Modes

### 1. Design

Design establishes the contract for the work before code is written.

The agent focuses on the problem, requirements, architecture, tradeoffs, and implementation plan. The output should be specific enough that tests can be written from it.

### 2. Test (Red)

Test mode turns the design into executable expectations.

The agent writes tests before implementation. Those tests should fail at assertions, not because of missing imports, broken setup, or vague scaffolding. A test that cannot fail for the right reason is worse than no test.

### 3. Implement (Green)

Implementation mode is constrained by the red tests.

The agent writes production code to make the tests pass. It does not edit the tests in this mode. If a test appears wrong, the agent stops and asks instead of quietly changing the specification.

### 4. Observe, Debug & Maintain

The final mode verifies that the system behaves correctly outside the narrow implementation loop.

The agent runs coverage, looks for untested lines, adds end-to-end checks where appropriate, investigates failures, and treats bug fixes as maintenance work. Bugs should be reproduced with a regression test before the fix is written.

## Handoffs

Each mode ends by writing a `HANDOFF.md` file at the repository root. The handoff is the canonical summary of the current state of work:

- What mode just ran
- What goal it pursued
- What inputs it used
- What files changed
- What outputs were produced
- What risks or open questions remain
- What mode should happen next

This matters because effective agent engineering depends on using context windows deliberately. `HANDOFF.md` lets independent agents work in separate context windows without sharing the whole conversation history. Each agent receives the contract it needs, produces the next contract, and leaves a compact handoff for parallel or follow-on work.

## Usage

Use `joedevflow` when an agent is building a feature, creating an MVP, refactoring code, fixing a bug, or doing any task that involves writing or changing implementation code.

The skill accepts an optional mode argument:

| Argument | Starts in |
| --- | --- |
| `design` | Mode 1 - Design |
| `test` | Mode 2 - Test (Red) |
| `implement` | Mode 3 - Implement (Green) |
| `debug` | Mode 4 - Observe, Debug & Maintain |

If no argument is provided, the agent reads `HANDOFF.md` when present. Otherwise, it asks which mode to start in.

## Philosophy

`joedevflow` is not a general instruction to "write better code." It is a workflow boundary.

It keeps an agent from blending discovery, specification, implementation, and maintenance into one vague coding pass. That separation is the point: design produces a testable contract, tests lock in the contract, implementation satisfies the tests, and observation keeps the system healthy after the initial change is done.
