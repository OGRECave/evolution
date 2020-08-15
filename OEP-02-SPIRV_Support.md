## SPIRV Support via GLSLang

* Author: [Pavel Rojtberg](https://github.com/paroj)
* Link: [The issue](https://github.com/OGRECave/evolution/issues/2)
* Status: **Open**
* Platforms: **GL3Plus**
* Complexity: 3 man-months

## Introduction and Rationale

Support for loading/ compiling to binary SPIRV shaders is a prerequisite for supporting Vulkan at some point.

Furthermore glslang would allow cross-compiling existing HLSL shaders for OpenGL.

Additionally using SPIRV is very handy on mobile where shader compiler quality is mediocre at best.

## Proposed solution

Similar to Cg, add a Plugin for generating SPIRV shaders as well as support for loading SPIRV shaders in GL3Plus and/ or GLES2. The plugin would leverage glslang. 

Also add SPIRV compatible sources to the RTShaderLib to demo on-the fly translation from Fixed-Function.

## Impact on existing code, compatibility

This is purely a new feature, so exiting code should not be affected. Some changes to the GL3Plus RenderSystem have to be made so they can load precompiled low-level shaders.
However we already have a boilerplate for this in the legacy GL RenderSystem as this is how the first ARB shader programs worked. **DONE**

The Plugin will need to build glslang which is a beast. However this will only affect the Plugin build and it is fine when it only works on Linux at first, where the build is more tractable.

Finally, the RTSS must be extended to support the SPIRV GLSL flavour, mainly introducing explicit uniform and attribute locations.

## Possible alternatives

Just implement the loading part in the RenderSystems. However this will result in a much smaller coverage and testing which on-the-fly compilation of existing shaders would give us.

## References

1. [glslang](https://github.com/KhronosGroup/glslang)
2. [SPIRV](https://www.khronos.org/registry/spir-v/)
3. [shaderc](https://github.com/google/shaderc)
4. [Layout Qualifier (GLSL)](https://www.khronos.org/opengl/wiki/Layout_Qualifier_(GLSL)#Explicit_uniform_location)