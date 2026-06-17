---
name: webmix-gsap
description: Generate refined, lightweight GSAP animations for websites and WordPress projects. Use when Codex or Claude Code is asked to create, edit, or review GSAP motion, ScrollTrigger scroll effects, SVG animation, page entrance effects, micro-interactions, or performance-conscious web animation with prefers-reduced-motion and mobile fallbacks.
---

# Webmix GSAP

## Overview

Create tasteful GSAP motion for production websites. Prefer subtle, fast, accessible animation that supports the content instead of drawing attention to itself.

## Motion Rules

- Keep motion restrained: short distances, low rotation, no bouncing unless the brand explicitly calls for it.
- Animate `transform` and `opacity` first. Avoid layout properties such as `top`, `left`, `width`, `height`, and expensive filters.
- Use durations around `0.35` to `0.9` seconds for UI/content reveals. Use longer timelines only when scroll-scrubbed.
- Use calm eases such as `power2.out`, `power3.out`, `sine.out`, or `expo.out` sparingly. Avoid elastic/back/bounce by default.
- Preserve readability: do not animate body copy in a way that delays access to content.
- Do not create one-off animation systems when a small local GSAP module is enough.

## Implementation Pattern

Use `gsap.context()` or a local cleanup function when the framework supports mounting/unmounting. In vanilla or WordPress, scope selectors to a root element and return cleanup.

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

## WordPress

Keep WordPress integration copy-pasteable and theme-friendly:

- Put animation code in a small file such as `assets/js/animations.js`.
- Enqueue GSAP and ScrollTrigger explicitly, or bundle them through the project's build step.
- Use `defer` when possible.
- Scope selectors with classes or data attributes owned by the theme/block.
- Avoid animations that depend on admin-only markup or editor wrappers.

```php
add_action('wp_enqueue_scripts', function () {
    wp_enqueue_script('gsap', 'https://cdn.jsdelivr.net/npm/gsap@3/dist/gsap.min.js', [], null, true);
    wp_enqueue_script('gsap-scrolltrigger', 'https://cdn.jsdelivr.net/npm/gsap@3/dist/ScrollTrigger.min.js', ['gsap'], null, true);
    wp_enqueue_script('theme-animations', get_template_directory_uri() . '/assets/js/animations.js', ['gsap', 'gsap-scrolltrigger'], '1.0.0', true);
});
```

## Checklist

Before finishing generated GSAP work:

1. Respect `prefers-reduced-motion`.
2. Use `gsap.matchMedia()` or equivalent mobile branching.
3. Keep mobile simpler than desktop.
4. Animate mostly `transform` and `opacity`.
5. Use ScrollTrigger for scroll effects.
6. Clean up contexts, matchMedia instances, and ScrollTriggers in component-based projects.
7. Verify that text remains readable and content is not hidden when JavaScript fails.
8. Do not add heavy dependencies or decorative motion that the request did not need.
