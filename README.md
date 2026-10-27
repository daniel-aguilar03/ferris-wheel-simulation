# Ferris Wheel Simulation

An interactive 3D fairground built with **Three.js**. Explore a Ferris wheel from the ground or ride inside a gondola while experimenting with transforms, surface detail and lighting.

[Open the live demo](https://daniel-aguilar03.github.io/ferris-wheel-simulation/) · [Watch the walkthrough](https://youtu.be/aYnfPb46d2c)

[![Ferris wheel walkthrough](https://img.youtube.com/vi/aYnfPb46d2c/hqdefault.jpg)](https://youtu.be/aYnfPb46d2c)

## Highlights

- Level gondolas driven by a nested transform hierarchy, with a live cabin-tilt readout.
- Ground and rider cameras, a rotating platform, spinning wheel and gently rocking seats.
- Custom tapered cabin geometry with authored UVs, colour, normal and emission maps.
- Day/night lighting with directional, point and spot lights, specular highlights and optional shadows.
- Interactive diagnostics for leveling, texture maps and shadow cost; live frame-rate and draw-call counters.

## Try it

Use a modern desktop browser. This project is **desktop WebGL**, not an immersive VR application.

| Control | Action |
| --- | --- |
| V | Switch ground / rider view |
| N | Switch day / night |
| P | Pause motion |
| B | Toggle the intentional leveling fault |
| M | Toggle normal / emission maps |
| H | Toggle shadows |
| Drag background | Look around |
| Mouse wheel | Zoom in ground view |

A useful first tour: watch the upright cabins, press B to remove compensation, restore it, then press V to ride. Compare maps with M and lighting with N.

## Run locally

Requires Node.js 20 or newer.

```sh
npm ci
npm start
```

Open [localhost:8080](http://localhost:8080/). Three.js is pinned and vendored, so the running demo does not depend on a CDN.

```sh
npm test
npm run check
npm run build
```

The build exports only the static application to `dist/`. GitHub Actions tests and publishes that directory to GitHub Pages.

## How it works

`Turntable → Wheel → Leveling pivot → Cabin → Rocking seat → Rider camera`

The wheel rotates by an angle and its child pivot applies the opposite angle. The cabin inherits its orbit but remains upright. The rider camera inherits the seat's movement naturally.

Each of the cabin's four walls uses one-quarter of the texture's horizontal U range and the full vertical V range. Normal maps change shading rather than geometry; emission makes strips appear bright without illuminating nearby objects. One shadow-casting light is active at a time to limit extra rendering work.

## Project layout

- `src/main.js`: scene, motion, lighting, camera and controls.
- `src/cabin.js`: custom wall geometry, normals and UVs.
- `src/shared/`: self-contained Three.js runtime, helpers and styles.
- `tests/`: outward normals, triangle winding, UV bounds and transform compensation.

## Scope and references

This is a graphics simulation, not a mechanical engineering model. Scene dimensions and speeds are design values; performance depends on browser and hardware. Shared geometry/materials reduce allocations but do not automatically combine draw calls.

References: [Three.js scene graph](https://threejs.org/manual/en/scenegraph.html), [materials](https://threejs.org/docs/#api/en/materials/MeshStandardMaterial), [shadow maps](https://threejs.org/manual/en/shadows.html).

Created by **Daniel Aguilar**. See [credits and development provenance](CREDITS.md).
