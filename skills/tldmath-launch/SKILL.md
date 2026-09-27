---
name: tldmath-launch
description: Put an app on a custom domain, or move it off an AI app builder (Lovable, Bolt, Replit, Base44) to Vercel, Netlify, Cloudflare Pages or GitHub Pages, with working email. Uses TLDMath's read-only plans (exact DNS records checked against live DNS, ordered steps, builder export steps from each builder's docs) and runs the changes with the user's own CLIs and tokens, after the user approves each one. Use when the user says "connect my domain", "put my Lovable app on my domain", "move my app off Replit", "set up email on my domain", "my custom domain isn't working", or "who runs this domain".
---

# TLDMath launch

TLDMath plans and checks; it never changes anything and never sees a key. You make the changes with the user's own tools, one approved step at a time.

## Tools

If the TLDMath connector is available (MCP, `https://www.tldmath.com/mcp`), use its tools. Otherwise call the same plans over HTTPS; no key is needed.

| Job | MCP tool | HTTP |
| --- | --- | --- |
| Records for a host, inbox and app-email service, checked live | `plan_dns_setup` | `GET https://www.tldmath.com/api/launch?domain=D&host=H&email=E&sender=S` |
| The same as ordered steps with commands | `plan_launch_steps` | `GET https://www.tldmath.com/api/launch/steps?domain=D&host=H&email=E&sender=S` |
| Move an app off a builder | `plan_app_move` | `POST https://www.tldmath.com/api/launch/move` with JSON `{"domain":D,"from":B,"to":H,"deps":[...],"env":[...]}` |
| Who runs a domain (registrar, DNS, website, inbox, senders, renewal) | `ownership_card` | `GET https://www.tldmath.com/api/launch/card?domain=D` |

Host ids: lovable, bolt, v0, replit, base44, framer, webflow, bubble, vercel, netlify, github-pages, cloudflare-pages. Inbox ids: google-workspace, microsoft-365, zoho-mail, fastmail, proton-mail, icloud, purelymail, migadu, namecheap-email, hostinger-email, cloudflare-email, no-email. App-email ids: resend, postmark, amazon-ses, sendgrid, mailgun, mailersend. Builders you can move from: lovable, bolt, replit or base44.

## Workflow

1. **Detect.** Read `package.json` (dependency names) and `.env.example` (variable names) yourself. Never read or send values from `.env`. Ask which domain, and where the app is built or hosted, if it isn't obvious.
2. **Plan.** Call the right tool. For a move, pass `dependencies` and `env_names` (names only).
3. **Show the plan.** List every step and every record to add, change or remove, in plain words, and say which account each change happens in. Wait for a clear yes.
4. **Apply, one step at a time.** Run the commands the plan gives, with the user's own signed-in CLIs (`vercel`) or tokens they set in their terminal (`CLOUDFLARE_API_TOKEN`, `RESEND_API_KEY`). Stop at the first error and report it.
5. **Verify.** Call the plan again. Repeat until every step it can check is done. Tell the user which steps only they can confirm (inside a builder, an inbox sign-up).

## Rules

- Show each change first and wait for a yes. A yes to one step isn't a yes to the next.
- Never ask the user to paste a key, token or password into the chat. Tell them the variable name and let them set it in their own terminal.
- Only add or update DNS records. Delete a record only when the user approves that record by name. Never delete MX, SPF, DKIM or DMARC records while moving a website.
- One SPF record per name: merge includes into it, as the plan does. Never add a second `v=spf1` record.
- On Cloudflare, create website records as "DNS only" (not proxied) until the host has issued its certificate.
- Don't buy anything. Domains are bought at the registrar's own checkout, by the user.
- Moving off a builder: export and back up the data before changing DNS, switch DNS last, and keep the builder project until the new site has run for a few days.
