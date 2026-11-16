# Distributed-Agriculture-Community-Infrastructure

Distributed Agriculture & Community Infrastructure
Hi. This is a folder of notes.


The idea
What if a farm could keep farming when the internet goes away?




The one comparison you'll see a lot
The doc keeps referencing ITU-T Y.2256, which is a real standards document about network-based smart farms. I'm using it as a mirror: it says what a smart farm looks like when the network is there, and this idea is about what happens when the network is not there. Comparing the two makes the differences clearer. I've only read its summary and table of contents so far, and the doc says so.

How to read the notes
Three things are marked differently:

"What we believe" — choices I've made, with reasons.

"What we require" — marked with "shall." These are things an implementation would need to do if it claims to follow the idea. They're internal to the doc, not claims about any external standard.

"What we don't know yet" — the open questions section. Each one is tagged with the phase that needs an answer.

If you only read one thing, read §1. It explains all of this better than I just did.


Research landscape
For readable summaries of existing community networks, mesh and radio projects, offline networking, and related field trials, see [Existing Projects & Research](research-landscape.md).

Where it stands
Phase 0, basically. The doc lays out five development phases, but the project is still in the "research and decide things" phase. The four things that need deciding before anything gets built:

Power budgets (how long does a node need to survive an outage?)

How data syncs across bad links (this is the hard one)

How identities are issued and revoked with no internet

Who actually runs a Community Node, and what happens when they leave

Until those are answered, it's all notes.

License / use
It's notes. Read them, quote them, build on them. If it ever becomes something more formal, this section will say so. For now, use your judgment.
