# Developh skills

Agent skills and plugins from [Developh](https://developh.co), a design and development studio in Lisbon.

[![skills.sh](https://skills.sh/b/developh/skills)](https://skills.sh/developh/skills)
[![License: MIT](https://img.shields.io/badge/License-MIT-0A4DFF.svg)](./LICENSE)

## website-audit

Ask your agent "why is my website slow?" and it runs a Google PageSpeed Insights audit (Lighthouse, mobile) of any
public website, then answers with a ranked list of fixes for your stack instead of a raw report: the four scores,
Core Web Vitals from real visitors when Google has them, the failing audits that weigh most, missing security headers
and page weight.

The audit runs on Developh's free MCP server at `https://www.developh.co/mcp`. No sign-up, no API key.

### Install

Any agent that reads skills (Claude Code, Codex, Cursor, Gemini CLI and others):

```bash
npx skills add developh/skills
```

The skill works without the MCP server: it calls the same tool over HTTP with `curl`. To give the agent the tools
directly, add the server too.

Claude Code, as a plugin (skill and MCP server together):

```bash
claude plugin marketplace add Developh/skills
claude plugin install developh@developh
```

Claude Code, only the MCP server:

```bash
claude mcp add --transport http developh https://www.developh.co/mcp
```

Cursor: the repository is a Cursor plugin too (`.cursor-plugin/plugin.json`).

Hermes Agent: the repository root is an Agent Plugins v1 package (`plugin.json`, `mcp.json`, `skills/`). Pinned
install: `hermes plugins install Developh/skills --ref <commit>`.

### Tools on the MCP server

| Tool | What it does | Writes anything |
|---|---|---|
| `run_website_audit` | PageSpeed Insights mobile audit of a public website, 10 to 90 s | No |
| `studio_info` | Facts about Developh: services, sectors, timelines, contact | No |
| `list_case_studies` | Developh's published client work, by sector or service | No |
| `request_contact` | Sends a project inquiry to Developh on the person's behalf | Yes, see below |

### What it sends where

- Every tool call goes to `www.developh.co`. `run_website_audit` passes the address you give it to Google PageSpeed
  Insights. Developh does not store the audits.
- `request_contact` is the only tool that writes: it creates an inquiry with the name, email and message the person
  gave, which Developh stores with its other inquiries and records in its analytics, and they get a confirmation
  email. The skill tells the agent to use it only when the person asked, with
  their own details, after showing them the message. The Hermes package marks the server `untrusted`, so Hermes asks
  before it runs and refuses it in unattended runs.
- About 20 audits per 10 minutes per client and 5 inquiries per hour.
- Privacy policy: https://www.developh.co/en/privacy-policy

## Also from Developh

- [reap](https://github.com/Developh/reap): kills the zombie processes AI coding agents leave behind on macOS.
  `npx skills add developh/reap`
- [resolve-cut](https://github.com/Developh/resolve-cut): Claude Code edits, renders and checks videos in DaVinci
  Resolve. `npx skills add developh/resolve-cut`

## License

MIT. See [LICENSE](./LICENSE).

Built by [Tiago Rodrigues](https://github.com/tiagonrodrigues) and the team at [Developh](https://developh.co).
