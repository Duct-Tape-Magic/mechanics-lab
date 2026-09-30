# CLAUDE.md — Mechanics Lab

## Project overview

A single-file static web app (`index.html`) that simulates 2D classical mechanics for students: projectiles, collisions, pendulums, springs. Hosted on **GitHub Pages** from the `main` branch of `github.com/Duct-Tape-Magic/mechanics-lab`. **No backend** (no Supabase, no server); the only external dependency is Google Fonts.

Live URL: https://duct-tape-magic.github.io/mechanics-lab/

The owner is a teacher, not a professional developer. Keep the single-file, no-build architecture unless they explicitly ask for a build step.

## Design

The UI deliberately follows the look of the claude.ai web interface: warm off-white backgrounds (`--bg-100` #FAF9F5, sidebar `--bg-200` #F5F4EF), near-black text, terracotta accent (`--accent` #D97757) for the primary button, 8 px button radius, 12 px card radius, serif display title (Source Serif 4) with a sans body (Inter). A dark theme mirrors claude.ai's dark palette (#262624 / #1F1E1D). All colours are CSS custom properties on `:root`, redefined under `:root[data-theme="dark"]`. The theme choice is stored in `localStorage` (`mech-theme`) because it is a per-viewer convenience.

Layout: top bar, left sidebar (add objects, experiments, scene list), centre stage (toolbar, canvas, readouts, graph), right inspector (Object / World tabs). Below 980 px the sidebar and inspector become slide-in drawers.

## Code structure (all inside `index.html`)

- **State:** `state.bodies` (balls and axis-aligned blocks; `static: true` makes a wall or fixed peg), `state.links` (`rod` constraints and `spring` forces; each has endpoint `a` = `{kind:'anchor',x,y}` or `{kind:'body',id}` and `b` = `{kind:'body',id}`), `state.world` (gravity, drag, floor, walls, width, friction, timeScale, display flags), `state.snapshot` (the starting state that Reset restores; taken automatically while `t === 0` and by "Set as start").
- **Physics (`step`)**: velocity Verlet with `H = 1/240` s substeps; springs are forces, rods are position-based constraints solved 4 times per step, collisions are impulse-based (circle/circle, circle/AABB, AABB/AABB) with restitution `min(e_a, e_b)`, iterated 3 times, then floor/wall bounds. Blocks do not rotate.
- **Rendering (`draw`)**: canvas with world-to-screen transform in `state.view` (`scale` px/m, `ox`/`oy` origin). y is up; the floor is y = 0. Velocity arrows are v × 0.25 s; force arrows are F × 0.05 m/N.
- **Inspector**: fields are generated with `num`/`chk`/`sel` helpers and carry `data-target` (`b:<id>`, `l:<id>` or `w`) and `data-prop`; `getProp`/`applyProp` handle real and virtual props (speed, direction, pendulum angle and angular velocity, attach-to). `syncInspector` refreshes non-focused inputs live.
- **Presets** live in the `PRESETS` array; each `build()` returns `{world, bodies, links}`.
- **Share link** encodes `sceneJSON()` as base64 in `#s=`; `loadFromHash` restores it on load.

## Conventions

- SI units everywhere; angles are degrees in the UI and radians internally.
- New world settings go in `DEFAULT_WORLD` (so old share links still load) and in `renderWorldPanel`.
- New object properties must survive `sceneJSON()` (strip transient fields like `trail`, `fx`, `fy`, `ax`, `ay`).
- Keep default visuals unchanged unless asked; the owner uses the site with students.

## Deployment

Edit `index.html`, commit to `main`, GitHub Pages redeploys within about a minute.
