# Research Brief: ClawdBot (OpenClaw) Security Risks

**Date**: February 19, 2026 (updated February 20, 2026)
**Topic**: Security risks of ClawdBot/Moltbot/OpenClaw agentic AI assistant in enterprise environments
**Target Audience**: CISOs, IT leaders, security engineers, compliance officers, SMB owners
**Sythe Labs Alignment**: AI/LLM Security cluster + Application Security + Offensive Security

---

## 1. SEO Foundation

### Primary Keyword
- **Keyword**: ClawdBot security risks
- **Estimated Volume**: High (trending, viral tool with 145,000+ GitHub stars)
- **Competition**: Medium (emerging topic, few established guides)
- **Search Intent**: Informational

### Secondary Keywords
1. **OpenClaw security** (Medium volume, Low competition, current project name)
2. **Moltbot security** (Medium volume, Low competition, interim name still searched)
3. **ClawdBot vulnerabilities** (Medium volume, Low competition, CVE-focused searches)
4. **agentic AI security risks** (Medium volume, Medium competition, broader category)

### Long-Tail Opportunities
- "is ClawdBot safe to use"
- "ClawdBot CVE-2026-25253"
- "OpenClaw security hardening"
- "how to block ClawdBot in enterprise"
- "ClawdBot vs Moltbot vs OpenClaw"
- "ClawdBot credential theft"
- "ClawHub malicious skills"
- "ClawHavoc campaign"

### Featured Snippet Opportunities
- **"What are ClawdBot security risks?"** -- Numbered list of risk categories would win
- **"Is ClawdBot safe?"** -- Definition/answer snippet, wide open
- **"ClawdBot CVE"** -- Table snippet with CVE IDs, CVSS scores, patch versions
- **"How to protect against ClawdBot"** -- Numbered protection steps list

### Target Word Count
- **Minimum**: 2,500 words
- **Recommended**: 2,500-3,000 words (comprehensive guide, competitive at 70th percentile)

### Topic Cluster Fit
- Extends Sythe Labs' AI/LLM Security content cluster
- Links to existing blog posts: "Understanding LLM Interactions" and "Software 3.0 in the Lens of Security"
- Ties into Application Security and Offensive Security clusters
- First-mover advantage on ClawdBot-specific security content

---

## 2. Subject Overview

### What is ClawdBot/OpenClaw?

ClawdBot is a free, open-source, self-hosted AI personal assistant created by Austrian developer **Peter Steinberger**. First published in **November 2025**, it connects large language models to messaging platforms (WhatsApp, Telegram, Discord, Slack, Signal, Microsoft Teams, iMessage, Google Chat) and performs autonomous tasks: email management, calendar coordination, call screening, reservation booking, and shell command execution on the host machine.

Unlike browser-based AI tools (ChatGPT, Claude, Gemini), ClawdBot runs locally, can proactively message users, remembers conversations for weeks, and executes tasks directly on the host computer.

### Name Changes Timeline
- **November 2025**: Launched as "ClawdBot" (agent named "Clawd")
- **January 27, 2026**: Renamed to "Moltbot" (agent "Molty") after trademark request from Anthropic
- **January 30, 2026**: Renamed to "OpenClaw" ("Moltbot never quite rolled off the tongue")
- **February 14, 2026**: Steinberger announced joining OpenAI; project moving to open-source foundation

### Adoption Stats
- **145,000+ GitHub stars** (fastest-growing repository in GitHub history)
- **2+ million GitHub visitors** in a single week
- **720,000 weekly downloads**
- **1.7 million agents** registered on Moltbook social platform (~17,000 human owners, 88:1 agent-to-human ratio)
- **22% of surveyed enterprises** report active employee usage
- **53% of enterprise customers** unknowingly granted privileged access (Noma Security)

---

## 3. Security Incidents Timeline

| Date | Event |
|---|---|
| **November 2025** | ClawdBot launched publicly |
| **January 24-27, 2026** | ClawdBot goes viral, 54,000 GitHub stars in four days |
| **January 27, 2026** | Renamed to Moltbot; Bitdefender publishes exposed control panel investigation; first malicious ClawHub skill appears |
| **January 29, 2026** | Straiker finds 4,500+ exposed deployments across 49 countries; Guardz publishes defense playbook; renamed to OpenClaw; patch v2026.1.29 released (CVE-2026-25253, CVE-2026-24763, CVE-2026-25157) |
| **January 30, 2026** | Noma Security reports 53% of enterprise customers granted privileged access without permission |
| **January 31, 2026** | Major surge of malicious skills on ClawHub; Wiz Research discovers Moltbook database exposure (1.5M API tokens); ClawHavoc campaign begins |
| **February 1, 2026** | Koi Security discloses ClawHavoc campaign (341 malicious skills); Moltbook database patched (~3 hours) |
| **February 3, 2026** | SecurityWeek publishes first public disclosure of CVE-2026-25253 (1-click RCE) |
| **February 5, 2026** | Adversa AI publishes comprehensive hardening guide |
| **February 2026** | SecurityScorecard STRIKE team identifies 42,900+ exposed instances across 82 countries; Hudson Rock confirms infostealers targeting OpenClaw |
| **February 14, 2026** | Steinberger announces joining OpenAI; project moves to foundation |

---

## 4. Specific Vulnerabilities

### Critical CVEs

| CVE ID | CVSS | Impact | Patched In |
|---|---|---|---|
| CVE-2026-25253 | 8.8 | 1-click RCE via auth token exfiltration | v2026.1.29 |
| CVE-2026-24763 | 8.8 | Docker sandbox escape via PATH manipulation | v2026.1.29 |
| CVE-2026-25157 | 7.8 | SSH command injection (macOS) | v2026.1.29 |

**CVE-2026-25253 Details (discovered by Mav Levin, depthfirst):**
- Control UI accepts `gatewayUrl` query parameter without validation
- Automatically establishes WebSocket connection bundling stored `authToken`
- Exploit chain: victim clicks malicious link -> attacker receives auth token via Cross-Site WebSocket Hijacking (CSWSH) -> attacker disables safety features via `exec.approvals.set` with `ask: "off"` -> escapes container via `config.patch` setting `tools.exec.host` to "gateway" -> arbitrary command execution via `node.invoke` -> `system.run`
- Affects all versions prior to v2026.1.24-1

### February 2026 Vulnerability Cluster (NEW)

| CVE ID | Impact |
|---|---|
| CVE-2026-26323 | Maintainer compromise |
| CVE-2026-26327 | Discovery spoofing |
| CVE-2026-26329 | Token replay |
| CVE-2026-26317 | Metadata manipulation |
| CVE-2026-26328 | Rogue gateway registration |

Additionally, Endor Labs disclosed six more vulnerabilities on February 18, 2026, covering SSRF, missing authentication, and path traversal.

### Insecure Defaults
- Default configuration binds to `0.0.0.0:18789` (all network interfaces, not localhost)
- Authentication disabled by default
- WebSocket connections lack origin verification
- Localhost connections implicitly trusted
- Configuration parameters leak via mDNS broadcasts on port 5353
- Researcher Jamieson O'Reilly (Dvuln) found hundreds of exposed instances via Shodan; examined 8 with zero authentication

### Plaintext Credential Storage
- API keys (Claude, OpenAI), passwords, OAuth tokens stored in plaintext Markdown and JSON files
- `.env` file contains LLM API keys
- `creds.json` stores messaging platform session credentials
- Sensitive paths: `~/.openclaw/`, `~/clawd/`, `~/.clawdbot/`
- RedLine Stealer, Lumma Stealer, and Vidar infostealers now specifically target these directories
- Straiker demonstrated full attack chain in under 30 seconds

### Unauthenticated Command Execution
- The "exec tool" passes user input directly to `os.system()` and `subprocess.run()` without sanitization
- Shell metacharacters (`;`, `|`, `&&`, `$`, backticks) execute unfiltered on the host

### Supply Chain: ClawHavoc Campaign
- **1,184 total malicious skills** historically published on ClawHub
- **341 confirmed malicious** out of 2,857 audited (11.9% compromise rate)
- **335 deploying Atomic Stealer (AMOS)** targeting macOS ($500-1,000/month MaaS)
- AMOS captures: browser credentials, keychain passwords, cryptocurrency wallets, SSH keys
- Skills masqueraded as cryptocurrency tools, YouTube utilities, Google Workspace integrations
- Some embedded reverse shell backdoors; others exfiltrated `.clawdbot/.env` to `webhook.site`
- C2 server: `91.92.242.30`
- ClawHub had no moderation: "All code downloaded will be treated as trusted code"
- Researchers: Oren Yomtov (Koi Security), Paul McCarty/6mile (OpenSourceMalware)

### Moltbook Database Breach
- Wiz Research (researcher: Gal Nagli) discovered Supabase database with Row Level Security (RLS) disabled
- Supabase API key hardcoded in client-side JavaScript
- Exposed: 1.5M API keys/tokens, 17,000+ user emails and X handles, 29,631 additional emails, 4,060 private conversations in plaintext
- ~4.75 million total records exposed
- Write access confirmed (attackers could inject malicious content)
- Fix required just 2 SQL statements
- Remediation: Jan 31 21:48 UTC (initial contact) to Feb 1 01:00 UTC (all tables secured)

### Indirect Prompt Injection and Memory Poisoning
- Reads emails, messages, web pages, enabling injection through any channel
- Palo Alto Networks identified persistent memory as "fourth critical dimension"
- Malicious instructions persist in memory for weeks ("delayed multi-turn attack chains")
- Web search results can contain indirect prompt injection in HTML payloads
- Third-party skills execute without context filtering or human-in-the-loop

### Fake Extensions
- Fake VS Code "Clawdbot Agent" extensions distributing trojans and remote access malware
- 60,000+ social media followers targeted by impersonation scams during rebrand confusion

---

## 5. Enterprise Risk Data

### Exposure at Scale
- **42,900+ unique IPs** hosting exposed control panels across **82 countries** (SecurityScorecard STRIKE team)
- **15,200 instances** vulnerable to RCE enabling full host machine takeover
- **24,000+ instances** associated with known CVEs
- **93% of verified instances** exhibit critical authentication bypass flaws
- **78% run pre-patch versions** (only 22% use "OpenClaw Control" branding)
- **45% hosted on Alibaba Cloud**, 37% of instances in China
- Top cloud: Tencent Cloud (48.9%), Alibaba Cloud (37.5%), DigitalOcean (24.5%), Hetzner (13.0%), Cloudflare (10.1%)
- Industries: information services, technology, manufacturing, telecom, financial services, healthcare, government, education

### Shadow AI
- **53% of enterprise customers** unknowingly granted ClawdBot privileged access over a weekend (Noma Security)
- **22% of surveyed enterprises** have active employee usage
- Traditional security tools fail to recognize AI-specific architectural patterns
- Gartner: OpenClaw "comes with unacceptable cybersecurity risk"
- Palo Alto Networks: "OpenClaw is not designed to be used in an enterprise ecosystem"

### The "Lethal Quadruple" (Palo Alto Networks)
1. Access to private data (credentials, personal information, business data)
2. Exposure to untrusted content (web, messages, third-party integrations)
3. Ability to externally communicate (send messages, execute commands)
4. **Persistent memory** (transforms attacks into stateful, delayed-execution chains)

### OWASP Top 10 for Agents
Palo Alto Networks mapped OpenClaw to all 10 OWASP vulnerabilities for AI agents: prompt injection (A01), insecure tool invocation (A02), excessive autonomy (A03), missing human-in-the-loop (A04), memory poisoning (A05), insecure third-party integrations (A06), insufficient privilege separation (A07), supply chain model risks (A08), unbounded agent-to-agent actions (A09), lack of runtime monitoring (A10).

### Threat Actor Correlation
Some exposed instance IPs correlate with infrastructure attributed to known APT groups: Kimsuky, APT28, Salt Typhoon, Sandworm, APT41.

---

## 6. Key Statistics for Content

| Statistic | Source |
|---|---|
| 42,900+ exposed instances across 82 countries | SecurityScorecard |
| 15,200 instances directly exploitable via RCE | SecurityScorecard |
| 93% of verified instances have auth bypass flaws | SecurityScorecard |
| 78% running unpatched versions | SecurityScorecard |
| 53% of enterprise customers granted privileged access unknowingly | Noma Security |
| 145,000+ GitHub stars | GitHub |
| 720,000 weekly downloads | GitHub |
| 1.5M API tokens exposed in Moltbook breach | Wiz Research |
| 4.75M total records exposed | Wiz Research |
| 1,184 total malicious skills on ClawHub | GBHackers/CyberPress |
| 341 confirmed malicious out of 2,857 audited (11.9%) | Koi Security |
| 335 deploying Atomic Stealer (AMOS) | Koi Security |
| 3 critical CVEs (CVSS 7.8-8.8) | NVD |
| Credential exfiltration in under 30 seconds | Straiker |
| Database fix required 2 SQL statements | Wiz Research |
| 22% of surveyed enterprises have active usage | CSO Online |

---

## 7. Key Quotes

- **Heather Adkins (Google Cloud VP, Security Engineering)**: Called it "an infostealer malware disguised as an AI personal assistant"
- **Yassine Aboukir (Principal Security Consultant)**: "How could someone trust that thing with full system access?"
- **Eric Schwake (Salt Security)**: "A significant gap exists between consumer enthusiasm for one-click appeal and technical expertise needed for secure operation"
- **Jamieson O'Reilly (Dvuln)**: "AI agents tear down 20 years of security boundaries by design, including sandboxing, process isolation, permission models, firewalls. When exposed to the internet or compromised through supply chains, attackers inherit all access"
- **Palo Alto Networks**: AI agents represent "the new era of insider threats"; "OpenClaw is not designed to be used in an enterprise ecosystem"
- **Gartner**: OpenClaw "comes with unacceptable cybersecurity risk"
- **Wiz Research**: "AI tools don't yet reason about security posture or access controls on a developer's behalf"

---

## 8. Security Researcher Recommendations

### Immediate Actions
1. Revoke all API keys immediately
2. Disconnect from all messaging platforms
3. Forensically review host machine for compromise
4. Audit message history across all connected platforms
5. Rotate all credentials (especially if running pre-v2026.1.29)
6. Patch to v2026.2.1 or later

### Configuration Hardening
1. Bind to localhost: `gateway.bind: "127.0.0.1"` (never `0.0.0.0`)
2. Enable strong authentication on the gateway
3. Use Tailscale, VPN, or Cloudflare Tunnel for remote access
4. Block port 18789 at network perimeter
5. Enable Docker sandbox mode
6. Whitelist necessary tools only; block shell execution and browser control
7. Scope API tokens with minimal permissions
8. Never run as root; use unprivileged service accounts

### Enterprise Guidance
- **Gartner**: Block OpenClaw downloads and traffic; rotate corporate credentials; isolate to non-production VMs only; prohibit unvetted skills
- **Straiker**: Block deployment entirely until security guardrails implemented
- **Noma Security**: Discovery/inventory of all deployments; posture management; guided remediation; behavioral anomaly monitoring
- **Kaspersky**: Host-level application allowlisting; isolated subnets for experimental deployments; SIEM flagging abnormal agent activity

### Detection Indicators
- Directories: `~/.openclaw/`, `~/clawd/`, `~/.clawdbot/`
- WebSocket traffic on ports 3000, 18789
- mDNS broadcasts on port 5353
- Unusual OAuth events and non-standard User-Agents
- Automated data harvesting during off-hours
- C2 indicator: `91.92.242.30`

---

## 9. Competitive Landscape

### Competitor Content (Top SERP Results)

**1. Bitdefender** -- "Moltbot Security Alert: Exposed ClawdBot Control Panels"
- Focus: Exposed control panels, credential leaks, rebranding timeline
- Strength: First-mover security vendor report
- Gap: Limited remediation guidance

**2. Straiker** -- "How ClawdBot/Moltbot Becomes a Backdoor for System Takeover"
- Focus: Full attack chain demonstration, 4,500+ deployments
- Strength: Technical depth, PoC demonstration
- Gap: Enterprise-focused framing

**3. Palo Alto Networks** -- "Why Moltbot May Signal the Next AI Security Crisis"
- Focus: Lethal trifecta + persistent memory, OWASP mapping
- Strength: Framework-level analysis, strategic perspective
- Gap: Less tactical remediation

**4. SecurityScorecard** -- "Beyond the Hype: Moltbot's Real Risk"
- Focus: 42,900+ exposed instances, geographic/cloud distribution, APT correlation
- Strength: Scale data, quantitative analysis
- Gap: Limited actionable steps for SMBs

**5. Wiz Research** -- "Exposed Moltbook Database Reveals Millions of API Keys"
- Focus: Database breach, 1.5M tokens, remediation timeline
- Strength: Incident-specific technical depth
- Gap: Single incident focus

**6. Noma Security** -- "53% of Customers Gave ClawdBot Privileged Access"
- Focus: Enterprise shadow AI angle
- Strength: Enterprise risk framing, original data
- Gap: Product-focused, less educational

**7. Cisco** -- "Personal AI Agents like OpenClaw Are a Security Nightmare"
- Focus: Skill Scanner tool, malicious skill analysis
- Strength: Technical depth, specific PoC
- Gap: Product-focused

**8. CrowdStrike** -- "What Security Teams Need to Know About OpenClaw"
- Focus: Threat intelligence perspective, detection guidance
- Strength: Enterprise credibility
- Gap: Limited remediation playbook

**9. Snyk** -- "Your Clawdbot AI Assistant Has Shell Access"
- Focus: Developer security perspective, supply chain
- Strength: Developer audience alignment
- Gap: Narrow scope

**10. Sophos** -- "The OpenClaw Experiment is a Warning Shot"
- Focus: Enterprise AI security governance
- Strength: Strategic governance framing
- Gap: Limited technical detail

### Content Gaps Across ALL Competitors
1. **No single comprehensive guide** covering the FULL picture in one piece
2. **No SMB-focused content** addressing how small businesses should respond
3. **No complete incident timeline** from launch through February 2026 vulnerability cluster
4. **No "what to tell your boss" framing** for non-technical executives
5. **No regulatory/compliance angle** covering how OpenClaw affects SOC 2, HIPAA, PCI DSS posture
6. **No comprehensive detection checklist** combining file paths, network indicators, and behavioral indicators
7. **No "lessons for AI adoption" angle** framing this as a case study for evaluating any AI agent tool

### Differentiation Strategy
- **Sythe Labs angle**: Comprehensive single-source guide combining all incidents, vulnerabilities, and actionable protection steps. No competitor article covers the full picture (CVEs + exposed panels + ClawHavoc + Moltbook breach + shadow AI + remediation) in one piece.
- **Unique value**: Step-by-step protection playbook with detection indicators, hardening checklist, and policy guidance. Written for the business leader and security team, not just the researcher.

---

## 10. Recommended Outline

```
H1: ClawdBot security risks: What every organization needs to know

Introduction (150-200 words)
- Hook: 42,900+ exposed instances, 15,200 exploitable via RCE
- Problem: Fastest-adopted AI tool in history, built without enterprise security
- Promise: Complete breakdown of risks + actionable protection steps

H2: What is ClawdBot (and why does it keep changing names)?
- What it does: local AI assistant with full system access
- Name changes: ClawdBot -> Moltbot -> OpenClaw
- Adoption stats: 145K GitHub stars, 720K weekly downloads
- Why it matters: unlike ChatGPT, it runs ON your machine

H2: What are the security risks of using ClawdBot?
H3: Authentication bypass and network exposure
H3: Plaintext credential storage
H3: Remote code execution vulnerabilities
H3: Prompt injection and memory poisoning

H2: How the ClawHavoc supply chain attack compromised thousands of users
- 1,184 malicious skills on ClawHub
- 11.9% compromise rate
- Atomic Stealer deployment
- Zero moderation policy

H2: What the Moltbook database breach exposed
- 1.5M API tokens
- 4.75M total records
- Write access to the database
- Fixed with 2 SQL statements

H2: Why enterprises are banning ClawdBot
- Meta's ban and termination policy
- Gartner and Palo Alto Networks advisories
- 53% of enterprises unknowingly granted privileged access
- The "lethal quadruple" framework

H2: How to protect your organization
H3: If you're currently running OpenClaw
H3: Enterprise detection and blocking
H3: Hardening checklist for authorized use
H3: Compliance implications (SOC 2, HIPAA)

H2: Frequently asked questions
- Is ClawdBot safe to use?
- What is the difference between ClawdBot, Moltbot, and OpenClaw?
- Can ClawdBot be hacked?
- How do I know if my employees are using it?

Conclusion (150-200 words)
- Key takeaways
- Broader lesson: evaluate AI agents like you evaluate any third-party software
- CTA: Contact for AI security assessment
```

---

## 11. Meta Elements Preview

### Meta Title (50-60 characters)
- **Option 1**: "ClawdBot Security Risks: What Users Need to Know" (49 chars)
- **Option 2**: "ClawdBot Security Risks Explained | Sythe Labs" (47 chars)
- **Option 3**: "OpenClaw Security Risks: A Complete Guide" (41 chars)

### Meta Description (150-160 characters)
"ClawdBot has critical security risks including RCE vulnerabilities, credential theft, and supply chain attacks. Learn how to protect your organization." (152 chars)

### URL Slug
`/blog/clawdbot-security-risks`

---

## 12. Internal Linking Strategy

### Links FROM This Article
- `/blog/understanding-llm-interactions-a-technical-guide` (LLM architecture context)
- `/blog/software-3-0-in-the-lens-of-security` (agentic AI risk model)
- `/process` (security assessment methodology)
- `/contact` (consultation CTA)

### Cross-Links TO This Article (from existing pages)
- From "Understanding LLM Interactions" (add: "ClawdBot security risks" link)
- From "Software 3.0 in the Lens of Security" (add: "agentic AI security risks" link)
- From penetration testing service page (if has related content section)
- From vCISO service page (AI governance context)

### Content Constraint
User specified this article is purely informational. No service page links in the article body.

---

## 13. Sources

1. [Bitdefender](https://www.bitdefender.com/en-us/blog/hotforsecurity/moltbot-security-alert-exposed-clawdbot-control-panels-risk-credential-leaks-and-account-takeovers)
2. [Straiker](https://www.straiker.ai/blog/how-the-clawdbot-moltbot-ai-assistant-becomes-a-backdoor-for-system-takeover)
3. [The Register](https://www.theregister.com/2026/01/27/clawdbot_moltbot_security_concerns/)
4. [Palo Alto Networks](https://www.paloaltonetworks.com/blog/network-security/why-moltbot-may-signal-ai-crisis/)
5. [Guardz](https://guardz.com/blog/when-ai-agents-go-wrong-clawdbots-security-failures-active-campaigns-and-defense-playbook/)
6. [Noma Security](https://noma.security/blog/customers-gave-clawdbot-privileged-access-and-noone-asked-permission/)
7. [Tenable](https://www.tenable.com/blog/agentic-ai-security-how-to-mitigate-clawdbot-moltbot-openclaw-vulnerabilities)
8. [SecurityScorecard](https://securityscorecard.com/blog/beyond-the-hype-moltbots-real-risk-is-exposed-infrastructure-not-ai-superintelligence/)
9. [Wiz Research](https://www.wiz.io/blog/exposed-moltbook-database-reveals-millions-of-api-keys)
10. [depthfirst (CVE-2026-25253 technical writeup)](https://depthfirst.com/post/1-click-rce-to-steal-your-moltbot-data-and-keys)
11. [The Hacker News (ClawHavoc)](https://thehackernews.com/2026/02/researchers-find-341-malicious-clawhub.html)
12. [University of Toronto Advisory](https://security.utoronto.ca/advisories/openclaw-vulnerability-notification/)
13. [Kaspersky](https://www.kaspersky.com/blog/moltbot-enterprise-risk-management/55317/)
14. [CSO Online](https://www.csoonline.com/article/4129867/what-cisos-need-to-know-about-clawdbot-i-mean-moltbot-i-mean-openclaw.html)
15. [OWASP Top 10 for LLM Applications](https://owasp.org/www-project-top-10-for-large-language-model-applications/)
16. [Cisco - Personal AI Agents like OpenClaw Are a Security Nightmare](https://blogs.cisco.com/ai/personal-ai-agents-like-openclaw-are-a-security-nightmare)
17. [CrowdStrike - What Security Teams Need to Know About OpenClaw](https://www.crowdstrike.com/en-us/blog/what-security-teams-need-to-know-about-openclaw-ai-super-agent/)
18. [Snyk - Your Clawdbot AI Assistant Has Shell Access](https://snyk.io/articles/clawdbot-ai-assistant/)
19. [Sophos - The OpenClaw Experiment is a Warning Shot](https://www.sophos.com/en-us/blog/the-openclaw-experiment-is-a-warning-shot-for-enterprise-ai-security)
20. [Akamai - Practical Lessons in Building Secure Agents](https://www.akamai.com/blog/security/clawdbot-openclaw-practical-lessons-building-secure-agents)
21. [SocRadar - Is ClawdBot Actually Safe to Run?](https://socradar.io/blog/clawdbot-is-it-safe/)
22. [BleepingComputer - Infostealer Malware Stealing OpenClaw Secrets](https://www.bleepingcomputer.com/news/security/infostealer-malware-found-stealing-openclaw-secrets-for-first-time/)
23. [SecurityWeek - OpenClaw Security Issues Continue as SecureClaw Debuts](https://www.securityweek.com/openclaw-security-issues-continue-as-secureclaw-open-source-tool-debuts/)
24. [Infosecurity Magazine - Researchers Reveal Six New OpenClaw Vulnerabilities](https://www.infosecurity-magazine.com/news/researchers-six-new-openclaw/)
25. [VentureBeat - OpenClaw Proves Agentic AI Works, Also Proves Your Security Model Doesn't](https://venturebeat.com/security/openclaw-agentic-ai-security-risk-ciso-guide)
26. [OpenClaw Official Security Guide](https://clawbot.ai/clawbot-security-guide.html)
27. [Adversa AI - SecureClaw Open Source Security Plugin](https://www.helpnetsecurity.com/2026/02/18/secureclaw-open-source-security-plugin-skill-openclaw/)
