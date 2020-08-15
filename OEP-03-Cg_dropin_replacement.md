## Cg dropin replacement

* Author: Pavel Rojtberg
* Link: N/A
* Status: **Draft**
* Platforms: **All**
* Complexity: 3 man-months

## Introduction and Rationale

Many projects using the Ogre1 API rely on Cg for shader cross-compilation. However, Cg was deprecated in 2012 and since then not further developed. This means that neither new platforms (notably ARM) nor new backend languages (notably MSL) are supported.

As many existing Ogre projects are low on manpower, rewriting those shaders in a modern language is mostly not an option.

## Proposed solution

Provide a drop-in replacement for Cg in Ogre using a HLSL to GLSL translator library.
This is possible as Cg is almost exactly the same as SM2.0 HLSL (i.e. D3D9 syntax).

Here, the HLSLParser library seems the most promising candidate as it not only supports translation to GLSL, but also to Metal (MSL) and HLSL4. At this, it provides extensions to the HLSL dialect for new features texture arrays and shadow samplers.

## Impact on existing code, compatibility

Compatibility with the existing Cg plugin is the driving force behind this OEP.
Code currently not using the Cg plugin is not affected. However, it still can benefit from this development as e.g. the RTSS can be enabled for Metal through HLSL to MSL translation provided by this plugin.

## Possible alternatives

There is already support for the hlsl2glsl translator in the GLES2 RenderSystem.
While it lifts the main restriction of Cg - that is, being a closed source binary - the upstream development ceased and the project is deprecated as well.
Furthermore, we will miss our on new hardware features and MSL support.

Then there is also the `OgreUnifiedShader.h` preprocessor approach, which solves the problem of cross-platform shaders just like Cg.
However, it uses a GLSL subset and thus requires re-writing existing Cg shaders.

## References

1. [HLSLParser](https://github.com/Thekla/hlslparser)
1. [hlsl2glslfork](https://github.com/aras-p/hlsl2glslfork)