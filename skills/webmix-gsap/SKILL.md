---
name: webmix-gsap
description: Guide refined, lightweight GSAP motion direction for websites, landing pages, product sites, SaaS interfaces, ecommerce, media sites, dashboards, and app UIs. Use alongside the official greensock/gsap-skills for GSAP API accuracy when Codex or Claude Code is asked to create, edit, or review tasteful web animation, ScrollTrigger effects, SVG motion, page transitions, micro-interactions, or performance-conscious motion with prefers-reduced-motion and mobile fallbacks.
---

# Webmix GSAP

## Overview

Use this skill as a motion-direction layer on top of the official `greensock/gsap-skills`.

- Use `greensock/gsap-skills` for GSAP API correctness, plugin details, framework integration, and advanced patterns.
- Use `webmix-gsap` for taste, restraint, accessibility, performance, and production motion judgment.

Create tasteful GSAP motion for production web experiences. Prefer subtle, fast, accessible animation that supports the content and task instead of drawing attention to itself.

## Pairing With Official GSAP Skills

When both are available, apply them in this order:

1. Load the relevant official GSAP skill for the technical domain, such as `gsap-core`, `gsap-scrolltrigger`, `gsap-react`, `gsap-frameworks`, `gsap-plugins`, or `gsap-performance`.
2. Use the official skill's API guidance for syntax, cleanup, plugin registration, and framework lifecycle details.
3. Apply this skill's constraints to reduce excess motion, simplify mobile behavior, and keep the final result polished and lightweight.

Do not override official GSAP API guidance with this skill. If there is a conflict, keep the official implementation pattern and adjust the motion design parameters to match this skill.

## Motion Rules

- Keep motion restrained: short distances, low rotation, no bouncing unless the brand explicitly calls for it.
- Animate `transform` and `opacity` first. Avoid layout properties such as `top`, `left`, `width`, `height`, and expensive filters.
- Use durations around `0.35` to `0.9` seconds for UI/content reveals. Use longer timelines only when scroll-scrubbed.
- Use calm eases such as `power2.out`, `power3.out`, `sine.out`, or `expo.out` sparingly. Avoid elastic/back/bounce by default.
- Preserve readability: do not animate body copy in a way that delays access to content.
- Do not create one-off animation systems when a small local GSAP module is enough.
- Keep repeated UI interactions shorter and quieter than first-time content reveals.
- Match the domain: editorial can be softer, SaaS should be efficient, ecommerce should not slow product inspection, dashboards should prioritize scanability.

## Implementation Pattern

Use `gsap.context()` or a local cleanup function when the framework supports mounting/unmounting. In vanilla JavaScript, scope selectors to a root element and return cleanup.

```js
import gsap from "gsap";
import { ScrollTrigger } from "gsap/ScrollTrigger";

gsap.registerPlugin(ScrollTrigger);

export function initGsapMotion(root = document) {
  const reduceMotion = window.matchMedia("(prefers-reduced-motion: reduce)").matches;
  if (reduceMotion) {
    gsap.set(root.querySelectorAll("[data-animate]"), { clearProps: "all", opacity: 1 });
    return () => {};
  }

  const mm = gsap.matchMedia();

  mm.add("(min-width: 768px)", () => {
    const items = gsap.utils.toArray(root.querySelectorAll("[data-animate='fade-up']"));

    items.forEach((item) => {
      gsap.fromTo(
        item,
        { y: 24, opacity: 0 },
        {
          y: 0,
          opacity: 1,
          duration: 0.65,
          ease: "power3.out",
          scrollTrigger: {
            trigger: item,
            start: "top 85%",
            once: true
          }
        }
      );
    });
  });

  mm.add("(max-width: 767px)", () => {
    gsap.set(root.querySelectorAll("[data-animate]"), { opacity: 1, y: 0 });
  });

  return () => mm.revert();
}
```

## ScrollTrigger

- Use ScrollTrigger for scroll-based animation; do not hand-roll scroll listeners.
- Prefer `once: true` for reveal effects.
- Use `scrub: true` only for effects that genuinely need scroll-linked progress.
- Keep pinned sections rare and verify mobile behavior.
- Always register the plugin once in the module that owns the animations.

```js
gsap.to(".js-parallax-media", {
  yPercent: -8,
  ease: "none",
  scrollTrigger: {
    trigger: ".js-parallax-section",
    start: "top bottom",
    end: "bottom top",
    scrub: true
  }
});
```

## SVG Animation

- Animate SVG groups, paths, masks, gradients, and `drawSVG`-style stroke reveals when appropriate.
- Prefer transform-origin and SVG attributes that GSAP handles reliably.
- If DrawSVGPlugin is unavailable, implement path reveal with `strokeDasharray` and `strokeDashoffset`.

```js
document.querySelectorAll(".js-draw-path").forEach((path) => {
  const length = path.getTotalLength();
  gsap.set(path, { strokeDasharray: length, strokeDashoffset: length });
  gsap.to(path, {
    strokeDashoffset: 0,
    duration: 1.1,
    ease: "power2.out",
    scrollTrigger: { trigger: path, start: "top 85%", once: true }
  });
});
```

## Context Guidance

Adapt the same restrained motion principles to the product type:

- Marketing and landing pages: use reveals, mild parallax, and SVG accents to guide attention without delaying the offer.
- SaaS and dashboards: prefer micro-interactions, state transitions, and small spatial cues over large entrance choreography.
- Ecommerce: keep motion secondary to product browsing; avoid scroll effects that interfere with image inspection or purchase flow.
- Editorial and media: use scroll reveals sparingly and prioritize reading comfort.
- Apps and tools: animate state changes to clarify cause and effect; avoid decorative motion in repeated workflows.

## Checklist

Before finishing generated GSAP work:

1. Use official `greensock/gsap-skills` guidance for GSAP syntax and plugin behavior when available.
2. Respect `prefers-reduced-motion`.
3. Use `gsap.matchMedia()` or equivalent mobile branching.
4. Keep mobile simpler than desktop.
5. Animate mostly `transform` and `opacity`.
6. Use ScrollTrigger for scroll effects.
7. Clean up contexts, matchMedia instances, and ScrollTriggers in component-based projects.
8. Verify that text remains readable and content is not hidden when JavaScript fails.
9. Do not add heavy dependencies or decorative motion that the request did not need.
