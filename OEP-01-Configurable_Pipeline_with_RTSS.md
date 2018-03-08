## Configurable Rendering Pipeline and PBS with the RTSS

* Author: [Pavel Rojtberg](https://github.com/paroj)
* Link: [The issue](https://github.com/OGRECave/evolution/issues/1)
* Status: **Draft**
* Platforms: **All**
* Complexity: 3 man-months

## Introduction and Rationale

The RTSS is currently modelled very tightly after the Fixed Function shading model: e.g. computing lighting before texturing.
But for Physically Based Shading we store data relevant for lighting in textures. So the RTSS needs to be made more flexible in this regard to allow scriptable render pipelines.
Also the current RTSS code is overly verbose and poorly documented. The project therefore would also include documentation and cleanup tasks.

## Proposed solution

The goal here is to weaken the separation - either by allowing a RTSS "module" (SRS) to implement all 3 stages at once, or better by introducing requirements and capabilities to e.g. say "normal mapping requires per pixel lighting". (the current implementation just copy pastes per pixel lighting)

Ultimately you should be able to just state "physically_based" as [the pass shading option](https://ogrecave.github.io/ogre/api/1.10/Material-Scripts.html#shading).

## Impact on existing code, compatibility

The internal structure of the RTSS will change, therefore Users currently implementing a custom SRS will likely to deal with API breakage.
However this would only affect a small fraction of the user base as most users do not even use the RTSS.

The external API (and the ogrescript snippets) for enabling/ disabling existing SRS should be kept stable so RTSS consumers would not need any changes.

## Possible alternatives

There is a HLMS backport in 1.10 that already implements PBS. However the RTSS has some advantages with managing the code complexity when compared to the HLMS.

At its core the HLMS shader templates are just another variation of an uber-shader with all of its drawbacks like the `#ifdef` explosion accompanied by the hard to maintain paths through the shader. Bad encapsulation basically.

The RTSS on the other hand shines in this regard as it automatically handles the connection of the library functions and one only looks at one function at a time.

## References

1. [RTSS Overview](https://ogrecave.github.io/ogre/api/1.10/rtss.html#rtss_overview)
2. [HLMS Overview](https://ogrecave.github.io/ogre/api/1.10/hlms.html)
3. [HLMS shortcomings discussion](https://forums.ogre3d.org/viewtopic.php?f=25&t=93854)