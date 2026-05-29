# NSA MCP Security Design Considerations — 7/10

## What it is
The NSA's Artificial Intelligence Security Center just published a 17-page security guide specifically for Model Context Protocol (MCP) deployments. Published May 2026, publicly available, free. This is the US government's official threat model for how people are going to get hacked through MCP, and what to do about it. Identifier: U/OO/6030316-26, May 2026, Ver. 1.0.

## Why you'd want it (specific to your stack)
Jarvis currently runs with MCP connectors for GitHub, Gmail, and Slack. You're about to expand it further post-finals. The NSA's key finding: security problems in agentic systems don't stay isolated — a weakness in one MCP server propagates and compounds across the entire agent environment. You can't just patch individual endpoints; you have to treat the whole system as a continuum.

Their specific recommendations map directly to Jarvis: filtering proxies (don't let agents make arbitrary outbound requests), message integrity checks (verify MCP responses haven't been tampered with), local MCP scans before install (exactly what Step 4.5 of the Jarvis runbook says, now with government validation), and explicit access control on any MCP server that touches sensitive data.

This is the reference document that gives the Jarvis security gate (Step 4.5) authoritative backing. After last week's ToxicSkills wave (36% of public Claude skills malicious), having the NSA's threat model in hand before you expand the MCP stack is just good practice.

## Why I think it's worth your attention
The NSA publishing a document specifically about MCP security — one week after the biggest skill-security wave in the ecosystem's history — is not a coincidence. The government is taking this seriously. That's a signal about where the real risk is, and about how quickly this space is maturing from "cool hacks" to "actual threat landscape."

## What to do
Read pages 1-5 (threat model + key findings). Then audit the current Jarvis MCP setup against their checklist: does each MCP connector have explicit, scoped access? Is there any way an MCP response could hijack the agent's next action? Use the NSA framework to update the Step 4.5 safety gate in AGENT_RUNBOOK.md.

🔗 https://www.nsa.gov/Press-Room/Press-Releases-Statements/Press-Release-View/Article/4496698/nsa-releases-security-design-considerations-for-ai-driven-automation-leveraging/  
🔗 Full PDF: https://www.nsa.gov/Portals/75/documents/Cybersecurity/CSI_MCP_SECURITY.pdf
