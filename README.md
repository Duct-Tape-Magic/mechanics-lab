# Mechanics Lab

An interactive 2D mechanics sandbox for teaching classical mechanics: projectile motion, momentum and collisions, pendulums and springs.

**Live site:** https://duct-tape-magic.github.io/mechanics-lab/

## What it does

- Add balls, blocks, fixed walls, pendulums (mass on a rigid rod) and springs (mass on a Hooke's-law spring).
- Drag objects to place them, drag the blue arrow tip to set velocity, or type exact values (mass, position, velocity, speed and direction, restitution, gravity on/off, fixed in place).
- Rods and springs can attach to a fixed point or to another object, so you can build double pendulums, coupled masses and Newton's cradle.
- World settings: gravity, air drag, floor and side walls, floor friction, time scale, trails, velocity and force vectors.
- Live readouts of kinetic energy, potential energy, total energy and momentum, plus a graph of energy, position or velocity over time.
- Eleven built-in experiments, and a **Share link** button that encodes the whole scene in the URL.

Everything is in SI units. Physics runs at 240 steps per second using velocity Verlet integration, position-based rod constraints and impulse-based collisions with a coefficient of restitution.

## Hosting

The whole app is a single `index.html` with no build step and no backend. It is served by GitHub Pages from the `main` branch. To update the site, edit `index.html` and commit it.
