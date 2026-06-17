# webmix-gsap

Custom Skill for generating refined, lightweight GSAP animations for websites, including WordPress projects.

This repository follows the same public Skill layout style as `diffusionstudio/lottie`: the Skill lives under `skills/`.

## Install

```bash
npx skills add <your-github-user>/webmix-gsap
```

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
