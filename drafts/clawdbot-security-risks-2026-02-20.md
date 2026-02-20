---
Meta Title: "ClawdBot Security Risks: What Every User Must Know"
Meta Description: "42,900+ exposed ClawdBot instances. Critical RCE vulnerabilities. Plaintext credential storage. Get the full risk breakdown and actionable protection steps."
Primary Keyword: ClawdBot security risks
Secondary Keywords: OpenClaw security, ClawdBot vulnerabilities, is ClawdBot safe, agentic AI security risks
URL Slug: /blog/clawdbot-security-risks
Internal Links:
 - https://sythelabs.com/blog/software-3-0-in-the-lens-of-security
 - https://sythelabs.com/blog/understanding-llm-interactions-a-technical-guide
 - https://sythelabs.com/process
 - https://sythelabs.com/contact
External Links:
 - https://www.paloaltonetworks.com/blog/network-security/why-moltbot-may-signal-ai-crisis/
 - https://genai.owasp.org/resource/owasp-top-10-for-agentic-applications-for-2026/
 - https://www.wiz.io/blog/exposed-moltbook-database-reveals-millions-of-api-keys
 - https://securityscorecard.com/blog/beyond-the-hype-moltbots-real-risk-is-exposed-infrastructure-not-ai-superintelligence/
---

# ClawdBot security risks: What every user must know

Security researchers have identified over 42,900 exposed ClawdBot instances across 82 countries. Of those, 15,200 are directly exploitable through remote code execution. ClawdBot (now called OpenClaw) became the fastest-growing open-source project in GitHub history, accumulating 145,000 stars and 900,000 weekly downloads in just a few months. The speed of adoption has far outpaced the security maturity of the project.

The ClawdBot security risks span critical CVEs, supply chain attacks, a massive database breach, and architectural design flaws that fundamentally undermine user safety. These risks reflect a broader shift in how [AI-driven software intersects with security](https://sythelabs.com/blog/software-3-0-in-the-lens-of-security). Whether you are running OpenClaw yourself or concerned about shadow AI deployments in your organization, this article breaks down every major risk and provides actionable protection steps.

## What is ClawdBot (and why does it keep changing names)?

ClawdBot is a free, open-source AI personal assistant created by Austrian developer Peter Steinberger. First published in November 2025, it gained explosive popularity by late January 2026. Unlike browser-based AI tools such as ChatGPT or Claude, ClawdBot runs locally on your machine with full system access. It connects to more than 15 messaging platforms, including WhatsApp, Telegram, Discord, Slack, Signal, and Microsoft Teams.

The tool can execute shell commands, write and run code, control web browsers, access cameras, read and write files, and maintain persistent memory across conversations. It can even proactively message users and act autonomously without being prompted. This makes it fundamentally different from the AI assistants most people are familiar with. It is not a chatbot. It is an autonomous agent with the keys to your entire system.

### The name changes

The project has gone through three names in under four months:

- **November 2025**: Launched as "ClawdBot" (with an AI agent named "Clawd")
- **January 27, 2026**: Renamed to "Moltbot" (agent "Molty") after a trademark request from Anthropic
- **January 30, 2026**: Renamed to "OpenClaw," the current official name

On February 14, 2026, Steinberger announced he was joining OpenAI, and the project is moving to an open-source foundation. Despite the rebrand, most searches still reference "ClawdBot" or "Moltbot," creating confusion that threat actors have exploited through impersonation scams and fake extensions.

### Adoption at scale

The numbers tell the story of unprecedented adoption:

- **145,000+ GitHub stars**, making it the fastest-growing repository in GitHub history
- **900,000+ weekly downloads**
- **2+ million GitHub visitors** in a single week
- **22% of surveyed enterprises** report active employee usage
- **53% of enterprise customers** unknowingly granted it privileged access, according to Noma Security

This adoption rate matters because every new installation creates a potential attack surface. As the sections below demonstrate, the default security posture makes exploitation trivial.

## What are the ClawdBot security risks?

The vulnerabilities span multiple categories, from critical CVEs to fundamental architectural design flaws. Here is what you need to know.

### Authentication bypass and network exposure

ClawdBot's default Docker configuration binds to `0.0.0.0:18789`, exposing the service to all network interfaces rather than restricting it to localhost. Additionally, authentication is disabled by default. WebSocket connections lack origin verification. Localhost connections receive implicit trust, which creates bypass opportunities when the tool runs behind reverse proxies.

Security researcher Jamieson O'Reilly of Dvuln found over 900 publicly exposed instances, eight of which had zero authentication. The [SecurityScorecard STRIKE team](https://securityscorecard.com/blog/beyond-the-hype-moltbots-real-risk-is-exposed-infrastructure-not-ai-superintelligence/) later identified 42,900+ exposed instances. Independent researcher Maor Dayan found that 93% of scanned instances exhibited critical authentication bypass flaws.

Configuration parameters also leak through mDNS broadcasts on port 5353. Anyone on the local network can discover running ClawdBot instances and their configuration details without any additional tools.

### Plaintext credential storage

ClawdBot stores API keys, passwords, and OAuth tokens in plaintext Markdown and JSON files. The `.env` file contains LLM API keys. The `creds.json` file stores messaging platform session credentials. Sensitive paths include `~/.openclaw/`, `~/clawd/`, and `~/.clawdbot/`.

As a result, infostealers have already adapted to target these directories specifically. RedLine Stealer uses its FileGrabber module to sweep `%UserProfile%\.clawdbot\*.json`. Lumma Stealer applies heuristics to find files named "secret" or "config." Noma Security demonstrated a complete data exfiltration kill chain in under 30 seconds, extracting tokens, API keys, and cryptocurrency seed phrases from a running instance.

Beyond the immediate credential theft, attackers who access these files gain months of conversation history, planning context, behavioral patterns, and social graphs from every connected messaging platform.

### Remote code execution: critical ClawdBot vulnerabilities

Three critical CVEs were patched in version v2026.1.29:

| CVE ID | CVSS Score | Impact |
|---|---|---|
| CVE-2026-25253 | 8.8 (High) | One-click RCE via auth token exfiltration |
| CVE-2026-24763 | 8.8 (High) | Docker sandbox escape via PATH manipulation |
| CVE-2026-25157 | 7.5 (High) | SSH command injection on macOS |

Of these, CVE-2026-25253 is particularly severe. The attack chain works as follows: a victim clicks a malicious link, which triggers a WebSocket connection that exfiltrates the stored authentication token. The attacker then disables safety features, escapes the container, and achieves arbitrary command execution on the host machine. All of this requires only a single click from the user.

In February 2026, researchers disclosed additional vulnerabilities including CVE-2026-26323 (maintainer compromise), CVE-2026-26329 (path traversal), and CVE-2026-26327 (discovery spoofing). Endor Labs disclosed six more vulnerabilities on February 18, 2026, covering SSRF, missing authentication, and path traversal.

Despite patches being available, 78% of identified instances still run unpatched versions.

### Prompt injection and memory poisoning

ClawdBot reads emails, messages, and web pages as part of its normal operation. This creates [prompt injection](https://sythelabs.com/blog/understanding-llm-interactions-a-technical-guide) vectors through any channel it monitors. A crafted email, a poisoned web search result, or a malicious message on any connected platform can all serve as entry points.

[Palo Alto Networks](https://www.paloaltonetworks.com/blog/network-security/why-moltbot-may-signal-ai-crisis/) identified persistent memory as the critical amplifier of these risks. Malicious instructions can persist in ClawdBot's memory for weeks, enabling what researchers call "delayed multi-turn attack chains." In one demonstrated attack, a single crafted email extracted a private cryptocurrency key from a running OpenClaw instance.

Palo Alto Networks expanded on Simon Willison's "lethal trifecta" framework for AI agents, identifying three compounding risk factors that apply to ClawdBot:

1. Access to private data (credentials, personal information, business data)
2. Exposure to untrusted content (web, messages, third-party integrations)
3. Ability to communicate externally (send messages, execute commands)

Palo Alto Networks argued that ClawdBot's persistent memory acts as an accelerant, amplifying each of these risks by enabling stateful, delayed-execution attacks.

Consequently, [Palo Alto Networks mapped](https://www.paloaltonetworks.com/blog/network-security/why-moltbot-may-signal-ai-crisis/) this architecture to all 10 categories in the [OWASP Top 10 for Agentic Applications](https://genai.owasp.org/resource/owasp-top-10-for-agentic-applications-for-2026/), describing it as a "full spectrum failure" across prompt injection, insecure tool invocation, excessive autonomy, and supply chain model risks.

## How the ClawHavoc supply chain attack compromised thousands of users

ClawHub, the community skill registry for ClawdBot, operated with a zero-moderation policy. The official documentation stated: "All code downloaded will be treated as trusted code." Naturally, threat actors exploited this trust model aggressively.

Security researchers Oren Yomtov (Koi Security) and Paul McCarty (6mile/OpenSourceMalware) documented the ClawHavoc campaign in detail. Out of 2,857 skills audited, 341 were confirmed malicious, an 11.9% compromise rate. The total number of malicious skills published historically on ClawHub reached 1,184.

Of the confirmed malicious skills, 335 deployed Atomic Stealer (AMOS), a macOS-targeting malware-as-a-service tool priced at $500 to $1,000 per month. AMOS captures browser credentials, keychain passwords, cryptocurrency wallets, and SSH keys from infected machines.

Specifically, the malicious skills masqueraded as cryptocurrency tools, YouTube utilities, and Google Workspace integrations. Some embedded reverse shell backdoors. Others exfiltrated the `.clawdbot/.env` file containing all API keys to `webhook.site`. The campaign's command-and-control server operated at `91.92.242.30`.

Separately, fake VS Code extensions labeled "Clawdbot Agent" distributed trojans and remote access malware. Tens of thousands of social media followers were targeted by impersonation scams during the chaotic rebrand period.

The ClawHavoc campaign illustrates how ClawdBot security risks extend beyond the core application to its entire ecosystem of skills and extensions.

## What the Moltbook database breach exposed

Moltbook, the social platform where ClawdBot agents interact with each other, suffered a significant data breach discovered by [Wiz Research](https://www.wiz.io/blog/exposed-moltbook-database-reveals-millions-of-api-keys). The platform's Supabase database had Row Level Security (RLS) disabled entirely. The Supabase API key was hardcoded in client-side JavaScript.

The exposed data included:

- **1.5 million API keys and tokens** for services like Claude, OpenAI, and others
- **17,000+ user accounts** and X (Twitter) handles
- **29,631 additional email addresses**
- **4,060 private conversations** stored in plaintext
- **~4.75 million total records**

Moreover, the database had write access enabled. Attackers could have injected malicious content into agent conversations or modified stored data. The remediation was remarkably simple: two SQL statements to enable Row Level Security. Resolution took approximately three hours from initial contact to full database security.

This breach highlights a recurring pattern in the ClawdBot ecosystem. The ClawdBot vulnerabilities discovered across its ecosystem are not sophisticated. They reflect missing fundamental security controls, not cleverly bypassed ones.

## Why major companies are banning ClawdBot

Multiple major companies and industry analysts have taken firm positions against ClawdBot.

**Meta** banned OpenClaw from its corporate networks. Employees who use it on work laptops risk termination. **Gartner** described the OpenClaw security posture as carrying "unacceptable cybersecurity risk." **Palo Alto Networks** stated explicitly that "OpenClaw is not designed to be used in an enterprise ecosystem."

Furthermore, the shadow AI problem compounds these risks. According to Noma Security, 53% of enterprise customers unknowingly granted ClawdBot privileged access over a single weekend. Traditional security tools fail to recognize AI-specific architectural patterns, so deployments often go undetected by existing monitoring.

Security researcher Yassine Aboukir called ClawdBot "an infostealer malware disguised as an AI personal assistant," while Google's Heather Adkins, VP of Security Engineering, warned bluntly: "Don't run Clawdbot." While those characterizations are pointed, the architectural parallels are worth considering. Like infostealers, ClawdBot harvests credentials, maintains persistence, communicates externally, and operates with broad system access. The difference is intent, not capability.

Additionally, some exposed instance IPs have been correlated with infrastructure attributed to known APT groups, including Kimsuky, APT28, Salt Typhoon, Sandworm, and APT41 (according to SecurityScorecard). While this correlation does not prove nation-state usage of ClawdBot, it demonstrates that sophisticated threat actors are aware of the attack surface it creates.

## How to protect yourself from ClawdBot security risks

Whether you are actively running OpenClaw or suspect it may be running on your systems, these steps will reduce your exposure.

### If you are currently running OpenClaw

Take these actions immediately:

1. **Revoke all API keys** stored in ClawdBot configuration files
2. **Disconnect from all messaging platforms** to prevent further data exposure
3. **Forensically review the host machine** for indicators of compromise
4. **Audit message history** across all connected platforms for unauthorized activity
5. **Rotate all credentials**, especially if running versions prior to v2026.1.29
6. **Update to v2026.2.14 or later** to patch known CVEs

### Detection and blocking

To identify ClawdBot on your systems:

- **File system indicators**: Search for directories `~/.openclaw/`, `~/clawd/`, and `~/.clawdbot/`
- **Network indicators**: Monitor for WebSocket traffic on ports 3000 and 18789
- **mDNS broadcasts**: Watch for activity on port 5353
- **Authentication anomalies**: Flag unusual OAuth events and non-standard User-Agents
- **Behavioral indicators**: Detect automated data harvesting during off-hours
- **C2 indicator**: Block traffic to `91.92.242.30` (known ClawHavoc C2 server)

### Hardening checklist for authorized use

If you decide to proceed after evaluating OpenClaw security risks, use it in a controlled capacity with a thorough [security assessment](https://sythelabs.com/process):

1. Bind to localhost only: set `gateway.bind: "loopback"` (the default; never use `"lan"` or `"auto"` on untrusted networks)
2. Enable strong authentication on the gateway
3. Use Tailscale, VPN, or Cloudflare Tunnel for any remote access
4. Block port 18789 at the network perimeter
5. Enable Docker sandbox mode
6. Whitelist only necessary tools; block shell execution and browser control
7. Scope API tokens with minimal permissions
8. Never run as root; use unprivileged service accounts
9. Audit all installed skills; remove any not explicitly vetted by your security team

### Compliance implications

In addition, if you work in a regulated industry, consider how ClawdBot usage affects compliance. The tool's plaintext credential storage, unrestricted system access, and lack of audit logging may conflict with requirements under SOC 2, HIPAA, PCI DSS, and similar frameworks.

If ClawdBot runs on systems that process regulated data, the unauthorized access and data handling could constitute a compliance violation. Review your acceptable use policies and ensure they explicitly address agentic AI tools with system-level access.

## Frequently asked questions

### Is ClawdBot safe to use?

In its default configuration, ClawdBot is not safe. It ships with authentication disabled, credentials stored in plaintext, and Docker deployments exposed to all network interfaces. With proper hardening (localhost binding, authentication, Docker sandboxing, and restricted tool access), the risk decreases significantly. However, even in hardened deployments, the supply chain risks from ClawHub skills and the architectural exposure to [prompt injection](https://sythelabs.com/blog/understanding-llm-interactions-a-technical-guide) remain significant concerns.

### What is the difference between ClawdBot, Moltbot, and OpenClaw?

In short, they are all the same project under different names. "ClawdBot" was the original name (November 2025). "Moltbot" was used briefly after an Anthropic trademark request (January 27, 2026). "OpenClaw" is the current official name (January 30, 2026 onward). Most users and search queries still reference "ClawdBot."

### Can ClawdBot be hacked?

Yes. Multiple critical ClawdBot vulnerabilities have been demonstrated, including one-click remote code execution (CVE-2026-25253), Docker sandbox escapes, and SSH command injection. Additionally, over 42,900 instances have been found publicly exposed with minimal or no authentication. The ClawHavoc supply chain campaign compromised 11.9% of audited skills on the ClawHub registry.

### How do I check if ClawdBot is on my system?

Search for the directory paths listed in the detection section above (`~/.openclaw/`, `~/clawd/`, `~/.clawdbot/`). Monitor network traffic for connections on ports 3000, 18789, and 5353. Check your running processes for anything associated with OpenClaw. Finally, consider implementing application allowlisting to prevent unauthorized installations.

## Key takeaways

The ClawdBot security risks represent a new category of cybersecurity challenge: an [agentic AI security landscape](https://sythelabs.com/blog/software-3-0-in-the-lens-of-security) where autonomous tools operate with broad system access and minimal security controls by default. The core risks include critical remote code execution vulnerabilities, plaintext credential storage, a compromised supply chain, and architectural exposure to prompt injection and memory poisoning.

The broader lesson extends beyond this single tool. As agentic AI security risks grow alongside adoption, users need to evaluate these tools with the same rigor applied to any third-party software that requests privileged access. That means thorough security assessments, strict access controls, continuous monitoring, and clear policies for acceptable use.

If you need help navigating the security implications of agentic AI tools, [reach out to our team](https://sythelabs.com/contact) for guidance on evaluating and securing these emerging technologies.
