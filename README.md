# DO178C_agents

Companion to the JudeLabs article on using the DO-178C lifecycle as a multi-agent AI development framework. This repository is a working sketch rather than a finished artifact set: the agent definitions, skills, and LAR structure are starting points, not complete or certified prompts and templates.

Agents A, B, C, and D are collectively the developer. DO-178C independence comes from the human reviewer at the commit gate, not from the division of labor between agents. Agent C is a drafting aid for verification artifacts; its record is reviewed by the human at commit time. Conventional static analyzers run as pre-merge gates on Agent B's output. Recommended: run Agents A and C on different model families, since same-model agents share correlated blind spots.
