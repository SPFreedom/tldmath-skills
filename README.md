# TLDMath skills

Agent Skills from [TLDMath](https://www.tldmath.com), for Claude Code, Codex and other agents that load skills.

## tldmath-launch

Put an app on a custom domain, or move it off an AI app builder (Lovable, Bolt, Replit, Base44) to Vercel, Netlify, Cloudflare Pages or GitHub Pages, with working email.

TLDMath plans and checks: the exact DNS records for your host, inbox and app-email service, checked against live DNS; ordered steps with commands; each builder's export steps from its own docs. Your agent makes the changes with your own CLIs and tokens, one approved step at a time.

```bash
npx skills add SPFreedom/tldmath-skills --skill tldmath-launch
```

Global install for Claude Code: add `-g -a claude-code`. For Codex: `-g -a codex`.

What the skill tells your agent:

- Show every change first and wait for your yes.
- Never ask you to paste a key or token into the chat. Commands read `CLOUDFLARE_API_TOKEN` or `RESEND_API_KEY` from your own terminal.
- Only add or update DNS records; delete one only when you name it.
- Keep one SPF record per name, and leave mail records alone when moving a website.
- Check again at the end until every record is in place.

It uses TLDMath's read-only [connector](https://www.tldmath.com/connector) (`https://www.tldmath.com/mcp`) when it's installed, and TLDMath's public API otherwise. No account or key needed. TLDMath never changes anything and never sees your keys.

The skill is also served at https://www.tldmath.com/skills/tldmath-launch/SKILL.md. This repository mirrors that file.

## License

MIT
