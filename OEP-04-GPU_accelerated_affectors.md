## GPU accelerated particle affectors

* Author: Pavel Rojtberg
* Link: [The issue](https://github.com/OGRECave/evolution/issues/4)
* Status: **Draft**
* Platforms: **All**
* Complexity: 3 man-months

## Introduction and Rationale

Ogre provides a powerful and flexible particle system. However, it has a major drawback of all particle updates happening on the CPU.
This limits the number of usable particles a lot - even when compared to WebGL implementations.

It is therefore desirable to parallelise the update or Particle Affectors to use the Ogre terminology.

## Proposed solution

Particle Affectors should be able to apply a shader to modify the active particles instead of being forced to loop over a `std::list`.
For this, transform feedback (aka RenderToVertexBuffer) should be used for efficient buffer to buffer mapping.
This has been already prototyped in the ParticleGS sample.

Affectors should be offered the option to set a custom RenderToVertexBuffer material, which will reference the respective shaders.
It should be signalled whether the material is supported by the current RenderSystem, s.t. the Affector can fall-back to CPU processing.

## Impact on existing code, compatibility

The core Particle API will need to be refactored as all particle data must be stored in hardware buffers for fast GPU transfer.
It should be evaluated how this affects existing projects and, a compatibility layer should be provided to support legacy code as needed.

## Possible alternatives

It possible render targets (render to texture) instead of transform feedback, which gives compatibility with WebGL1 class APIs/ hardware. However, on those floating point texture-formats are not supported, which in turn requires packing/ unpacking of RGBA textures.
This penalizes performance and generally is not worth the trouble as it is uncommon to use many particles on such low-end systems anyway.

As the particle updates are data-parallel one can trivially introduce multi-threading using OpenMP pragmas.
Given the core-count of contemporary CPUs, this will already yield a speed-up of one order of magnitude.

## References

1. [WebGL2 Particles](https://gpfault.net/posts/webgl2-particles.txt.html)
1. [WebGL1 Particles](https://nullprogram.com/blog/2014/06/29/)