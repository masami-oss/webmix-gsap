# webmix-gsap

Custom Skill for guiding refined, lightweight GSAP motion across websites, landing pages, product sites, SaaS interfaces, ecommerce, media sites, dashboards, and app UIs.

This is not a replacement for the official GSAP skills. Use it alongside `greensock/gsap-skills`:

- `greensock/gsap-skills`: official GSAP API, plugins, framework patterns, and performance details
- `webmix-gsap`: motion taste, restraint, accessibility, mobile lightness, and production polish

This repository follows the same public Skill layout style as `diffusionstudio/lottie`: the Skill lives under `skills/`.

## Install

Install the official GSAP skills first:

```bash
npx skills add https://github.com/greensock/gsap-skills
```

Then install this motion-direction skill:

```bash
npx skills add masami-oss/webmix-gsap
```

## Claude Code global setup

To make this skill available from every Claude Code project on the same machine, install or copy it into Claude Code's global skills directory:

```bash
mkdir -p ~/.claude/skills
npx skills add https://github.com/greensock/gsap-skills
npx skills add masami-oss/webmix-gsap
```

If the `npx skills add` command does not place it under `~/.claude/skills`, copy the skill folder manually:

```bash
git clone https://github.com/masami-oss/webmix-gsap.git
mkdir -p ~/.claude/skills
cp -R webmix-gsap/skills/webmix-gsap ~/.claude/skills/webmix-gsap
```

Restart Claude Code after installing so the global skill list is reloaded.

## Use

Ask Claude Code or Codex to use `webmix-gsap` when creating or reviewing GSAP motion.

Example:

> Use the official GSAP skills for implementation accuracy, then apply webmix-gsap as the motion direction layer. Create subtle fade-up reveals, SVG path drawing, ScrollTrigger, prefers-reduced-motion support, and mobile fallbacks. Keep the motion tasteful, lightweight, and restrained.

## Structure

```text
webmix-gsap/
├── README.md
├── LICENSE
└── skills/
    └── webmix-gsap/
        ├── SKILL.md
        └── agents/
            └── openai.yaml
```
