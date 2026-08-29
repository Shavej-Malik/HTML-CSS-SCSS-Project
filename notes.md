# Learn to Make Animations Using CSS by Swaraj Singh

### Video Explanation - [ https://youtu.be/cDDqAGifhrw?si=yRbYc5NsmcM88ome ]

# 1. Transition vs Animation - the big picture

CSS gives you two motion tools. Knowing which one to reach for is 80% of the battle.

|  | `transition` | `animation` (+ `@keyframes`) |
| --- | --- | --- |
| Knows | Only **2 points**: start & end | **Any number** of points (0%, 25%, 50%...) |
| Runs when | A property value changes (hover, class toggle, focus) | On its own, whenever you tell it to |
| Needs a trigger? | Yes — `:hover`, `:focus`, JS class change | No — can `infinite` loop, or run once on load |
| Typical use | Buttons, links, small state changes | Loaders, hero intros, looping motion, complex sequences |

**Rule of thumb:** if you can describe it as "when X happens, this becomes that" → `transition`. If you can describe it as "this keeps doing a sequence of things" → `@keyframes` + `animation`.

# 2. Transitions

A transition animates a property from its old value to its new value whenever that value changes.

```css
.box {
  transition-property: transform, background-color, border-radius;
  transition-duration: 0.4s;
  transition-timing-function: ease-out;
  transition-delay: 0s;

  /* shorthand — same order as above */
  transition: all 0.4s ease-out;
}

.box:hover {
  transform: scale(1.15) rotate(6deg);
  background: linear-gradient(135deg, #4dd8ff, #ff4d8d);
  border-radius: 24px;
}
```

**Key points**

- A transition needs a *start state* and an *end state* already defined in CSS (base style + `:hover` / a toggled class).
- You can transition multiple properties at once, each with its own timing, by comma-separating them:

```css
transition: transform 0.3s ease, opacity 0.5s linear 0.1s;
```

- Not every property can be transitioned. Things like `display` can't be smoothly animated (it's either on or off) — this is a common beginner trap.

> **Gotcha:** if nothing seems to be animating, check that the property actually has two different values to move between, and that `transition` is declared on the *base* selector (not just on `:hover`), otherwise the transition-out won't be smooth.
> 

# 3. Transform - the building block

`transform` is the property that actually moves things: `translate`, `rotate`, `scale`, `skew`. It's the property you'll use inside almost every animation in this course.

```css

.box { transition: transform 0.5s ease; }

.translate:hover { transform: translateY(-16px); }
.rotate:hover     { transform: rotate(45deg); }
.scale:hover      { transform: scale(1.35); }
.skew:hover       { transform: skew(-12deg, 4deg); }

/* combine any number of functions in one line */
transform: translateX(20px) rotate(10deg) scale(1.1);

```

**The four core functions**

- `translate(x, y)` — move without affecting layout flow
- `rotate(deg)` — spin around `transform-origin` (default: center)
- `scale(n)` — grow/shrink
- `skew(x, y)` — shear/tilt

**transform-origin** controls the pivot point:

```css
.box { transform-origin: top left; } /* rotate/scale from a corner instead of center */
```

**Gotcha:** transforms apply left-to-right. `translate() rotate()` is **not** the same as `rotate() translate()` — rotating first tilts the axis the translate then moves along.

# 4. Timing functions / easing

The same distance, in the same time, feels completely different depending on the **timing function**. This is the single biggest lever for making motion feel "designed" instead of robotic.

```css
.linear      { transition-timing-function: linear; }        /* constant speed — feels mechanical */
.ease        { transition-timing-function: ease; }          /* default: quick start, slow end */
.ease-in-out { transition-timing-function: ease-in-out; }   /* slow, fast, slow — most "natural" */
.overshoot   { transition-timing-function: cubic-bezier(.68,-0.55,.27,1.55); } /* bouncy pop */

```

**Built-in keywords:** `linear`, `ease`, `ease-in`, `ease-out`, `ease-in-out`

**Custom curves:** `cubic-bezier(x1, y1, x2, y2)` — the two control points of a bezier curve. Sites like cubic-bezier.com let you drag handles and copy the value out.

> **Pro tip:** "premium" motion on award-winning sites is almost always a custom `cubic-bezier`, not the browser default `ease`. Try `cubic-bezier(0.16, 1, 0.3, 1)` for a smooth "expo-out" feel — a favorite among motion designers.
> 

# 5. @keyframes

A `transition` only knows "from" and "to." `@keyframes` lets you define a whole sequence of stops — as many as you like — and then run it with the `animation` property. It plays on its own; no hover required.

```css
@keyframes pulse {
  0%   { transform: scale(1);   box-shadow: 0 0 0 0 rgba(255,193,69,.5); }
  70%  { transform: scale(1.4); box-shadow: 0 0 0 14px rgba(255,193,69,0); }
  100% { transform: scale(1);   box-shadow: 0 0 0 0 rgba(255,193,69,0); }
}

.pulse-dot {
  animation: pulse 1.8s ease-out infinite;
}
```

- You can also write `from { }` and `to { }` instead of `0%` / `100%` — identical meaning, just shorthand for a two-stop keyframe.
- Any number of stops is allowed: `0%, 20%, 55%, 80%, 100% { ... }`.

# 6. Animation properties, deep dive

`animation-name` just points to the `@keyframes` block. Everything else controls **how** it plays.

```css
.drift-box {
  animation-name: drift;                 /* which @keyframes to run */
  animation-duration: 1.6s;              /* one full cycle */
  animation-timing-function: ease-in-out;
  animation-delay: 0s;                   /* wait before starting */
  animation-iteration-count: infinite;   /* a number, or "infinite" */
  animation-direction: alternate;        /* normal | reverse | alternate | alternate-reverse */
  animation-fill-mode: forwards;         /* keep the end-state after finishing */
  animation-play-state: running;         /* running | paused — great for a pause button */

  /* shorthand, same order as above */
  animation: drift 1.6s ease-in-out 0s infinite alternate forwards;
}
```

| Property | What it controls |
| --- | --- |
| `animation-name` | which `@keyframes` rule to play |
| `animation-duration` | length of one cycle |
| `animation-timing-function` | the easing curve |
| `animation-delay` | wait time before it starts |
| `animation-iteration-count` | how many times it repeats (`3`, `infinite`) |
| `animation-direction` | play forward, backward, or alternate each cycle |
| `animation-fill-mode` | what styles apply before/after the animation runs |
| `animation-play-state` | pause/resume it (toggle with a class + JS, or `:hover`) |

> **The one everyone forgets:** without `animation-fill-mode: forwards`, an element **snaps back** to its pre-animation state the instant the animation ends. If your "slide in and stay" animation keeps vanishing right after it finishes, this is almost always why.
> 

# 7. steps() and staggering

`steps(n)` jumps between *n* discrete frames instead of smoothly interpolating — perfect for sprite sheets, ticking clocks, or a deliberately "chunky" feel.

```css
@keyframes tick {
  from { transform: translateX(0); }
  to   { transform: translateX(-160px); }
}
.digits {
  display: flex;
  animation: tick 1.6s steps(4) infinite; /* jumps in 4 discrete increments */
}

```

**Staggering** is how you build the "elements arrive one after another" effect used on almost every good landing page. Same animation, incrementing `animation-delay` per child:

```css
@keyframes riseIn {
  from { opacity: 0; transform: translateY(16px); }
  to   { opacity: 1; transform: translateY(0); }
}
.chip {
  opacity: 0;
  animation: riseIn 0.6s ease forwards;
}
.chip:nth-child(1) { animation-delay: 0.05s; }
.chip:nth-child(2) { animation-delay: 0.15s; }
.chip:nth-child(3) { animation-delay: 0.25s; }
.chip:nth-child(4) { animation-delay: 0.35s; }
.chip:nth-child(5) { animation-delay: 0.45s; }
```

# 8. Performance & accessibility

Two rules that separate hobby CSS from production CSS.

### Animate cheap properties

```css
/* SLOW — triggers layout on every single frame */
.bad { transition: width 0.3s, top 0.3s, left 0.3s; }

/* FAST — same visual result, runs on the GPU compositor thread */
.good { transition: transform 0.3s, opacity 0.3s; }

/* will-change: a hint for animations about to start — use sparingly, remove after */
.card { will-change: transform; }
```

`transform` and `opacity` don't trigger layout or paint recalculation — the browser can hand them straight to the GPU. Properties like `width`, `top`, `left`, `margin` force the browser to recompute layout on every frame, which is expensive and can cause visible jank, especially on lower-end devices.

### Respect motion sensitivity

Some people get motion-sick or have vestibular disorders. This media query is close to mandatory on real projects:

```css
@media (prefers-reduced-motion: reduce) {
  *, *::before, *::after {
    animation-duration: 0.001ms !important;
    animation-iteration-count: 1 !important;
    transition-duration: 0.001ms !important;
  }
}
```