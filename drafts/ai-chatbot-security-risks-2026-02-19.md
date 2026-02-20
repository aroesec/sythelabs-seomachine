---
Meta Title: ClawdBot Security Risks: 42,900 Exposed Instances
Meta Description: Understand the ClawdBot security risks threatening businesses in 2026, including 42,900+ exposed instances and three critical CVEs. Learn how to protect your organization.
Primary Keyword: ClawdBot security risks
Secondary Keywords: OpenClaw security, Moltbot security, ClawdBot vulnerabilities, agentic AI security risks
URL Slug: /blog/clawdbot-security-risks
Internal Links:
 - https://sythelabs.com/blog/understanding-llm-interactions-a-technical-guide
 - https://sythelabs.com/blog/software-3-0-in-the-lens-of-security
 - https://sythelabs.com/contact
External Links:
 - https://www.bitdefender.com/en-us/blog/hotforsecurity/moltbot-security-alert-exposed-clawdbot-control-panels-risk-credential-leaks-and-account-takeovers
 - https://www.straiker.ai/blog/how-the-clawdbot-moltbot-ai-assistant-becomes-a-backdoor-for-system-takeover
 - https://www.paloaltonetworks.com/blog/network-security/why-moltbot-may-signal-ai-crisis/
 - https://securityscorecard.com/blog/beyond-the-hype-moltbots-real-risk-is-exposed-infrastructure-not-ai-superintelligence/
 - https://www.wiz.io/blog/exposed-moltbook-database-reveals-millions-of-api-keys
 - https://owasp.org/www-project-top-10-for-large-language-model-applications/
Word Count: ~3,200
---

# ClawdBot security risks: what every business needs to know in 2026

In January 2026, security researchers at Straiker discovered over 4,500 exposed ClawdBot deployments across 49 countries. Most had no authentication enabled. Within days, Bitdefender confirmed that attackers could access API keys, session credentials, and full private conversation histories from these exposed instances. By February, SecurityScorecard's STRIKE team had identified [42,900+ exposed instances across 82 countries](https://securityscorecard.com/blog/beyond-the-hype-moltbots-real-risk-is-exposed-infrastructure-not-ai-superintelligence/), with 15,200 directly exploitable through remote code execution. The scale and severity of these ClawdBot security risks caught the industry off guard.

ClawdBot (now renamed OpenClaw) gained 145,000 GitHub stars and 720,000 weekly downloads in a matter of weeks. It is the fastest-growing open-source repository in GitHub history. It is also one of the most significant ClawdBot security risks to emerge in the agentic AI era.

This article breaks down the specific vulnerabilities, documented incidents, and practical steps your organization should take to protect itself. Whether your team adopted ClawdBot intentionally or employees installed it on their own, these risks demand immediate attention.

## What is ClawdBot (OpenClaw)?

ClawdBot is an open-source AI personal assistant created by Austrian developer Peter Steinberger. First published in November 2025, it connects large language models to messaging platforms like WhatsApp, Telegram, Slack, Discord, Signal, Microsoft Teams, and iMessage. Unlike browser-based AI tools, ClawdBot runs locally on your machine and can proactively send messages, execute shell commands, manage files, and take autonomous actions across connected services.

The project has gone through three names in under three months:

- **November 2025**: Launched as "ClawdBot" (agent named "Clawd")
- **January 27, 2026**: Renamed to "Moltbot" (agent named "Molty") after a trademark request from Anthropic over similarity to "Claude"
- **January 30, 2026**: Renamed again to "OpenClaw" because, in Steinberger's words, "Moltbot never quite rolled off the tongue"

On February 14, 2026, Steinberger announced he was joining OpenAI, with the project moving to an open-source foundation. The rapid name changes have created confusion around patch tracking, vulnerability disclosures, and organizational awareness. Security teams searching for "ClawdBot" may miss advisories published under "Moltbot" or "OpenClaw," and vice versa. Whether you search for ClawdBot vulnerabilities, OpenClaw security advisories, or Moltbot security alerts, you are looking at the same tool and the same risks.

For a deeper understanding of how LLM-powered tools like ClawdBot interact with your systems, see our [technical guide to LLM interactions](https://sythelabs.com/blog/understanding-llm-interactions-a-technical-guide).

## What are the ClawdBot security risks?

The ClawdBot security risks span multiple categories: critical software vulnerabilities, insecure default configurations, plaintext credential storage, supply chain attacks, and indirect prompt injection. Here is what security researchers have documented.

### Critical CVEs

Three critical ClawdBot vulnerabilities were patched in version v2026.1.29:

| CVE ID | CVSS | Impact | Patched In |
|---|---|---|---|
| CVE-2026-25253 | 8.8 | Remote code execution | v2026.1.29 |
| CVE-2026-24763 | 8.8 | Docker sandbox escape | v2026.1.29 |
| CVE-2026-25157 | 7.8 | SSH command injection (macOS) | v2026.1.29 |

**CVE-2026-25253 (CVSS 8.8): One-click remote code execution.** Discovered by researcher Mav Levin, this vulnerability allowed attackers to steal authentication tokens through a malicious link. The Control UI accepted a `gatewayUrl` query parameter without validation and automatically bundled the stored auth token into a WebSocket handshake. An attacker could disable safety features, escape the Docker container sandbox, and execute arbitrary commands on the host machine. All versions prior to v2026.1.24-1 were affected.

**CVE-2026-24763 (CVSS 8.8): Docker sandbox escape.** This vulnerability allowed attackers to escape the container sandbox through PATH environment variable manipulation, gaining access to the host system.

**CVE-2026-25157 (CVSS 7.8): SSH command injection on macOS.** A command injection vulnerability targeting the macOS SSH subsystem allowed arbitrary code execution.

### Insecure defaults and exposed control panels

ClawdBot's default configuration binds to ``0.0.0.0:18789``, which exposes the control panel to all network interfaces rather than restricting it to localhost. [Bitdefender's investigation](https://www.bitdefender.com/en-us/blog/hotforsecurity/moltbot-security-alert-exposed-clawdbot-control-panels-risk-credential-leaks-and-account-takeovers) confirmed that authentication is disabled by default, WebSocket connections lack origin verification, and localhost connections are implicitly trusted.

Researcher Jamieson O'Reilly of Dvuln examined eight exposed instances via Shodan and found zero authentication on all of them, providing full command execution access. SecurityScorecard later reported that 93% of verified instances exhibit critical authentication bypass flaws.

### Plaintext credential storage

ClawdBot stores API keys, passwords, and OAuth tokens in plaintext Markdown and JSON files. The `.env` file contains LLM API keys for services like Claude and OpenAI, while `creds.json` stores messaging platform session credentials. Sensitive directories include `~/.openclaw/`, `~/clawd/`, and `~/.clawdbot/`.

[Straiker's research](https://www.straiker.ai/blog/how-the-clawdbot-moltbot-ai-assistant-becomes-a-backdoor-for-system-takeover) demonstrated a full attack chain: an attacker sends a message through a connected platform, triggers command execution on the host, accesses credential files, and exfiltrates them to an external server. The entire process takes under 30 seconds.

Infostealer malware families, including RedLine, Lumma, and Vidar, now specifically target these ClawdBot directories when scanning compromised machines.

### Supply chain attacks through ClawHub

ClawdBot's skill distribution platform, ClawHub, became a vector for widespread supply chain attacks. Security researchers at Koi Security identified 341 malicious skills out of 2,857 audited on the platform, an 11.9% compromise rate. A later analysis by GBHackers found 1,184 total malicious skills historically published in what researchers named the "ClawHavoc" campaign.

Of the confirmed malicious skills, 335 deployed Atomic Stealer (AMOS), a macOS-targeting malware-as-a-service that captures browser credentials, keychain passwords, cryptocurrency wallets, and SSH keys. Other skills embedded reverse shell backdoors or exfiltrated `.clawdbot/.env` files to external servers.

ClawHub had no moderation process at the time. Its documentation stated: "All code downloaded will be treated as trusted code." The current remediation is a reporting system where skills with more than three unique reports are auto-hidden.

### Indirect prompt injection and memory poisoning

Because ClawdBot reads emails, messages, and web pages, attackers can inject malicious instructions through any of these channels. [Palo Alto Networks identified](https://www.paloaltonetworks.com/blog/network-security/why-moltbot-may-signal-ai-crisis/) a critical additional dimension: persistent memory. Malicious instructions hidden in forwarded messages can persist in ClawdBot's memory for weeks, creating what researchers describe as "delayed multi-turn attack chains."

This transforms prompt injection from a point-in-time exploit into a stateful, delayed-execution attack. A malicious payload delivered today could trigger actions weeks later when the right conditions are met.

## Real-world ClawdBot security incidents

These ClawdBot security risks are not theoretical. Every incident below has been documented by independent security researchers.

### Moltbook database breach: 1.5 million API tokens exposed

Wiz Research discovered that Moltbook, ClawdBot's social platform for AI agents, had its entire production database exposed to the public internet. The Supabase database had [Row Level Security (RLS) disabled](https://www.wiz.io/blog/exposed-moltbook-database-reveals-millions-of-api-keys), and the Supabase API key was hardcoded in client-side JavaScript.

The exposed data included:

- **1.5 million API keys and tokens** (including OpenAI API keys shared between agents)
- **17,000 user emails and X (Twitter) handles**
- **29,631 additional emails** from early access signups
- **4,060 private conversations** stored in plaintext
- **4.75 million total records** across all database tables

Attackers also had write access, meaning they could inject malicious content or prompt injection payloads into the database. The remediation required just two SQL statements to enable RLS. Wiz contacted Steinberger at 21:48 UTC on January 31, and all tables were secured by 01:00 UTC on February 1.

### ClawHavoc: 1,184 malicious skills on ClawHub

The ClawHavoc campaign represents one of the largest supply chain attacks against an AI platform. Malicious skills masqueraded as cryptocurrency tools (Solana wallets, Ethereum trackers, ByBit integrations), YouTube utilities, and Google Workspace plugins.

O'Reilly demonstrated the ease of exploitation by uploading a benign skill with artificially inflated download counts. It reached 4,000+ downloads, with developers from seven countries installing it, before he disclosed the proof of concept.

### Exposed instances linked to known threat actors

SecurityScorecard found that some exposed ClawdBot instance IPs correlate with infrastructure attributed to known advanced persistent threat (APT) groups, including Kimsuky, APT28, Salt Typhoon, Sandworm, and APT41. While correlation does not prove these groups are actively exploiting ClawdBot, the overlap with known threat actor infrastructure is concerning.

### Fake extensions distributing malware

During the chaotic rebranding from ClawdBot to Moltbot, fake "Clawdbot Agent" extensions appeared on the VS Code marketplace. These extensions installed trojans and remote access malware on users' machines. Impersonation scams also targeted over 60,000 social media followers during the name change confusion.

## Why ClawdBot is a shadow AI problem for businesses

According to Noma Security, [53% of their enterprise customers unknowingly granted ClawdBot privileged access](https://noma.security/blog/customers-gave-clawdbot-privileged-access-and-noone-asked-permission/) over a single weekend, and nobody asked for permission. Employees installed the tool on work machines, connected it to corporate messaging accounts, and gave it access to sensitive systems without involving IT or security teams.

This is the shadow AI problem at its most extreme. Traditional security tools cannot detect ClawdBot installations because they were not designed to recognize AI agent architectural patterns. Personal devices with work VPN configurations and corporate tokens become attack vectors that bypass every control your organization has in place.

Gartner issued a warning that OpenClaw "comes with unacceptable cybersecurity risk." Palo Alto Networks stated plainly: "OpenClaw is not designed to be used in an enterprise ecosystem."

As we explored in our analysis of [AI-driven software security](https://sythelabs.com/blog/software-3-0-in-the-lens-of-security), agentic AI security risks are fundamentally different from traditional software risks. Tools like ClawdBot combine access to private data, exposure to untrusted content, the ability to take external actions, and persistent memory into a single tool that operates with minimal human oversight.

Palo Alto Networks mapped ClawdBot against all 10 categories in the [OWASP Top 10 for AI Agents](https://owasp.org/www-project-top-10-for-large-language-model-applications/), including prompt injection, insecure tool invocation, excessive autonomy, memory poisoning, and supply chain risks. ClawdBot exhibits vulnerabilities in every single category.

## How to protect your organization from ClawdBot security risks

Whether employees installed ClawdBot with or without approval, here are the steps your security team should take.

### 1. Discover and inventory all ClawdBot installations

Search for the following indicators across your environment:

- **Directories**: `~/.openclaw/`, `~/clawd/`, `~/.clawdbot/`
- **Network traffic**: WebSocket connections on ports 3000 and 18789
- **mDNS broadcasts**: Port 5353 activity
- **Process names**: Look for OpenClaw, Moltbot, or ClawdBot processes

Your endpoint detection tools may not flag these by default. You may need to create custom detection rules or use [our security assessment methodology](https://sythelabs.com/process) to identify all instances.

### 2. Revoke and rotate all credentials immediately

If any ClawdBot installation is found, treat it as a potential compromise. Revoke all API keys stored in the tool, disconnect all connected messaging platforms, and rotate credentials for any service that ClawdBot could access. This includes LLM API keys, OAuth tokens, and messaging platform session credentials.

### 3. Patch or remove

If your organization has a legitimate use case for ClawdBot, update to v2026.2.1 or later immediately. The OpenClaw security hardening steps below are essential for any deployment:

- Bind the gateway to localhost only (`gateway.bind: "127.0.0.1"`)
- Enable strong authentication on the gateway
- Use a VPN or zero-trust tunnel (Tailscale, Cloudflare Tunnel) for remote access
- Block port 18789 at the network perimeter
- Enable Docker sandbox mode
- Whitelist only necessary tools and explicitly block shell execution unless required
- Never run as root; use unprivileged service accounts

If there is no approved use case, remove all installations and block ClawdBot downloads and traffic at the network level.

### 4. Update your AI acceptable use policy

Your acceptable use policy should explicitly address agentic AI security risks. Unlike browser-based AI assistants, tools like ClawdBot execute commands on host machines and maintain persistent access to messaging platforms. The risk profile is fundamentally different from pasting text into ChatGPT.

Define which AI tools are approved, what data they can access, and whether autonomous execution capabilities are permitted. Review and update this policy quarterly as new agentic AI tools emerge.

### 5. Monitor for ongoing threats

Add ClawdBot-specific indicators to your security monitoring:

- Unusual OAuth events and non-standard User-Agents
- Automated data harvesting patterns during off-hours
- Outbound connections to known ClawHavoc C2 infrastructure (documented indicator: `91.92.242.30`)
- New processes or services matching ClawdBot, Moltbot, or OpenClaw signatures

### 6. Assess your broader agentic AI posture

ClawdBot is the first agentic AI tool to reach this scale, but it will not be the last. Use this as an opportunity to evaluate how your security program handles AI tools that can take autonomous actions on your systems.

## Frequently asked questions about ClawdBot security risks

### Is ClawdBot safe to use?

Not in its default configuration. The tool ships with authentication disabled, binds to all network interfaces, and stores credentials in plaintext. These ClawdBot security risks persist even after patching, since 78% of detected instances still run unpatched versions. Gartner and Palo Alto Networks have both warned against enterprise use. If you choose to use it, follow the hardening steps in this article.

### What is the difference between ClawdBot, Moltbot, and OpenClaw?

They are the same tool under different names. ClawdBot was renamed to Moltbot on January 27, 2026, after a trademark request from Anthropic. It was renamed again to OpenClaw on January 30. Any Moltbot security advisory or OpenClaw security bulletin applies equally to all three names. The rapid name changes have created confusion around vulnerability tracking and patch management.

### What data does ClawdBot have access to?

ClawdBot can access messages across all connected platforms (WhatsApp, Telegram, Slack, Discord, Signal, Teams), execute shell commands on the host machine, read and write files, manage calendars and email, and store conversation history with persistent memory. The scope of access is significantly broader than a typical AI chatbot.

### How do I know if employees are using ClawdBot?

Search for the directories `~/.openclaw/`, `~/clawd/`, and `~/.clawdbot/` on employee machines. Monitor for WebSocket traffic on ports 3000 and 18789. Check for mDNS broadcasts on port 5353. Review network logs for connections to ClawHub or Moltbook domains.

### Were the ClawdBot vulnerabilities patched?

The three critical CVEs (CVE-2026-25253, CVE-2026-24763, CVE-2026-25157) were patched in v2026.1.29 on January 29, 2026. However, SecurityScorecard found that 78% of detected instances are still running pre-patch versions. The Moltbook database breach was resolved within three hours of disclosure. Supply chain risks on ClawHub remain an ongoing concern.

### Should my company block ClawdBot entirely?

For most organizations, yes. Unless you have a specific, approved use case with proper hardening in place, the risk profile of ClawdBot in its current state outweighs the productivity benefits. Gartner recommended blocking downloads and traffic. If you do allow use, implement the hardening steps described above and monitor continuously.

## Conclusion

ClawdBot security risks represent a new category of threat for organizations. This is not a browser-based chatbot where the worst case is an employee pasting sensitive data into a text box. ClawdBot runs on host machines, executes shell commands, stores credentials in plaintext, and maintains persistent access to messaging platforms. When exposed to the internet or compromised through supply chain attacks, it gives attackers full access to everything the tool can reach.

As security researcher Jamieson O'Reilly put it: "AI agents tear down 20 years of security boundaries by design, including sandboxing, process isolation, permission models, and firewalls. When exposed to the internet or compromised through supply chains, attackers inherit all access."

Here are the key takeaways:

- **Discover first.** Search your environment for ClawdBot, Moltbot, and OpenClaw installations immediately.
- **Assume compromise.** If you find unpatched or exposed instances, revoke and rotate all associated credentials.
- **Patch or remove.** Update to v2026.2.1+ with hardened configuration, or remove the tool entirely.
- **Update your policies.** Your AI acceptable use policy must address agentic AI tools that execute code and maintain system access.
- **Prepare for what comes next.** ClawdBot is the first agentic AI tool at this scale. It will not be the last.

If your organization needs help assessing its exposure to agentic AI security risks, [contact Sythe Labs](https://sythelabs.com/contact) to schedule a consultation.
