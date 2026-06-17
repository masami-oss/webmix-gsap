# webmix-gsap

Custom Skill for generating refined, lightweight GSAP animations for websites, including WordPress projects.

This repository follows the same public Skill layout style as `diffusionstudio/lottie`: the Skill lives under `skills/`.

## Install

```bash
npx skills add masami-oss/webmix-gsap
```

## Claude Code global setup

To make this skill available from every Claude Code project on the same machine, install or copy it into Claude Code's global skills directory:

```bash
mkdir -p ~/.claude/skills
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

> Use webmix-gsap to add subtle fade-up section reveals, SVG path drawing, ScrollTrigger, prefers-reduced-motion support, and mobile fallbacks to this WordPress theme.

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
