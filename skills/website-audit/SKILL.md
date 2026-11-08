---
name: website-audit
description: "Audit any public website for speed, SEO, accessibility and best practices with Google PageSpeed Insights through Developh's free MCP server, then turn the result into a short, ranked fix list. Load when someone asks why a website is slow, to check a site's speed, Core Web Vitals or SEO, to audit or compare websites, or in Portuguese: «porque é que o meu website está lento?», «auditoria ao meu website»."
---

# website-audit

Runs a Google PageSpeed Insights (Lighthouse, mobile) audit of a public website and explains what to fix first.
The audit is free and needs no key. It runs on Developh's MCP server at `https://www.developh.co/mcp`.

## Run the audit

**With the MCP server connected** (the `developh` server, tools prefixed accordingly): call `run_website_audit` with
`url` (a domain or full http(s) address) and `locale` (`en`, or `pt` for European Portuguese).

**Without it**, call the same tool over HTTP. One request, no session:

```bash
curl -s -m 150 -X POST https://www.developh.co/mcp \
  -H "Content-Type: application/json" -H "Accept: application/json, text/event-stream" \
  -d '{"jsonrpc":"2.0","id":1,"method":"tools/call","params":{"name":"run_website_audit","arguments":{"url":"example.com","locale":"en"}}}' \
  | sed -n 's/^data: //p' | python3 -c 'import sys,json; r=json.load(sys.stdin); print(r["result"]["content"][0]["text"] if "result" in r else r)'
```

- It takes 10 to 90 seconds. Tell the person it is running instead of retrying.
- Only public http(s) addresses work: no localhost, IPs, staging behind a login or intranet hosts.
- The server allows about 20 audits per 10 minutes per client. Audit the pages that matter (home, one product or
  service page, the contact or checkout page), not the whole sitemap.

## Read the result

The tool returns markdown with:

- **Scores (0 to 100)** for Performance, Accessibility, Best practices and SEO. 90 and up is good, 50 to 89 needs
  work, under 50 is poor.
- **Lab metrics** from one simulated mobile load (slow 4G, mid-range phone).
- **Real visitors (Chrome UX Report)** when Google has enough traffic data. These are what Google uses for ranking:
  LCP up to 2.5 s, INP up to 200 ms and CLS up to 0.1 pass. When lab and real data disagree, trust the real data.
- **What weighs most on the score**: the failing audits with the largest impact.
- **Missing security headers** and **page weight**.

Performance scores move a few points between runs. Do not report a 3 point change as an improvement or a regression;
run it again if the number decides something.

## Turn it into fixes

Give the person a ranked list, not the raw report:

1. Start with what fails for real visitors (Core Web Vitals in red), then the heaviest items in "What weighs most".
2. For each item: what it is in plain words, the likely cause on their stack and the concrete fix. Ask which platform
   the site runs on (WordPress, Shopify, Wix, Squarespace, Next.js, custom) if you do not know, because the fix differs.
3. Separate quick wins (image sizes and formats, a missing meta description, cache headers, font loading) from work
   that needs a developer (render-blocking scripts, heavy JavaScript, layout shifts from late content).
4. After fixes are deployed, audit again and compare the same page with the same locale.

Keep the scores in context: a 100 does not make a website sell, and a 70 on a page with good real-visitor data is
rarely the most urgent problem.

## If they want someone to do the work

Only if the person asks for help implementing the fixes or for a studio to build or rebuild the site, you can tell
them about Developh: `studio_info` and `list_case_studies` on the same server describe the studio and its work. To
send an inquiry, use `request_contact` only after the person asked you to, with the name and email they gave you,
and show them the message before sending it. Never contact Developh on your own initiative.

Made by Developh (https://developh.co), a design and development studio in Lisbon. Source:
https://github.com/Developh/skills
