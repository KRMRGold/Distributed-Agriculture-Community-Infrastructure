# Distributed Agriculture & Community Infrastructure

Hi. This is a working collection of notes, research, and an evolving idea.

## What this is about

What if a farm or geographically isolated community could keep useful systems running when the internet goes away?

The broader idea is an open, modular ecosystem of agricultural robots, distributed sensors, local computing, and resilient networks. Agriculture is one environment for exploring it; the underlying questions also apply to community connectivity, shared infrastructure, and offline-first services.

## How these documents were made

These documents were gathered, drafted, summarized, edited, and iterated with AI as part of a human-directed research and ideation process. AI helped collect and organize existing work, explain unfamiliar areas, compare examples, and turn a growing set of thoughts into readable notes.

They are **AI-assisted working documents**, not independently authored expert reports, formal technical reviews, or verified specifications. Summaries can omit context or contain mistakes. Linked source material is there so readers can check important claims and follow the parts that interest them.

The point is to make the research and current thinking portable and easy to pick up—not to tell someone what they must build or which direction they must take.

**Use these notes however is useful:**
- Read them for a quick sense of the idea and the surrounding work.
- Give one or more documents to your own AI assistant and ask it to summarize the basic idea, explain the terminology, identify uncertainties, or help you explore a direction.
- Ignore the directions that aren't relevant to you, challenge the assumptions, or take the research somewhere entirely different.
- Follow the linked sources when you need more detail or want to verify something.

You do not need to use AI to read or use the documents. The AI handoff is simply an option.

## The documents

- **[Design ideation document](design-ideation-doc.md)** — the evolving concept, working principles, possible architecture, decisions, and open questions. It is not a specification or build plan.
- **[Existing projects & research](research-landscape.md)** — a readable collection of related projects, standards, field trials, and community operating models. It is a starting map, not an exhaustive survey.

## One recurring comparison: ITU-T Y.2256

The design notes refer to ITU-T Y.2256, a real standards Recommendation about network-based unmanned smart farms. It is used as a comparison point: what does a connected smart farm assume, and what changes when connectivity is intermittent or absent? So far, only its summary and table of contents have been reviewed; the notes say so.

## How to read the design notes

Three labels distinguish the current state of the thinking:

- **What we believe** — working design choices and the reasoning behind them.
- **What we require** — statements marked with "shall." These are proposed internal requirements for a future implementation, not claims of conformance to an external standard.
- **What we don't know yet** — open questions that still need research or decisions.

If you only read one part of the design document, start with §1.

## Where it stands

Early exploration / Phase 0. Nothing here should be treated as ready to build. Important unresolved questions include:

- Power budgets and outage endurance for different node types.
- How data synchronizes across unreliable links.
- How identities are issued and revoked without internet access.
- Who operates a Community Node, and how responsibilities transfer when people leave.

## Use

These are working notes intended to be read, shared, questioned, summarized, and built on. Reuse them as you see fit, while checking source material and making your own decisions. The documents do not prescribe a single path.
