Tarp
======

Overview
--------

*Tarp* is an *almost* single header C library to raster vector graphics. It provides a lightweight and portable API purely focussed on decently fast rendering without any gimmicks.

![tarp demo](https://user-images.githubusercontent.com/10217168/40217212-6e979956-5a20-11e8-9012-0c30483df8a7.gif)

What can Tarp do?
--------
- Rasterize fills and strokes.
- Stroke joins (round, bevel, miter) and caps (round, square, butt).
- Dashed strokes.
- Gradients as fill and strokes (Note: only linear at the moment).
- Transformations for path, fills and strokes.
- EvenOdd and NonZero fill rules.
- Nested path clipping.
- Non-scaling stroke.

What does Tarp not want to provide?
--------
- A matrix stack.
- Any form of document representation.
- Any form of document loading.
- Bezier math that goes beyond rendering.
- Raster image rendering
- Text rendering. (Dealing with fonts is a huge task by itself, you can easily build a font rasterizer on top of Tarp using *stb_truetype* or *freeimage*, though)

Basic usage
--------

Install *Tarp* or add the Tarp folder to your source directory. Include it into your project with `#include <Tarp/Tarp.h>`. Specify the implementation you want to compile in **one** c/c++ file to create the implementation, i.e.:

```
/* include OpenGL here! */

/* tell Tarp to compile the opengl implementations */
#define TARP_IMPLEMENTATION_OPENGL
#include <Tarp/Tarp.h>
```

Here is a basic *Tarp* example:

```
tpPath path;
tpStyle style;
tpMat4 proj;

/* set an orthographic projection based on the window size */
proj = tpMat4MakeOrtho(0, wwidth, wheight, 0, -1, 1);
tpSetProjection(&ctx, &proj);

/* create a path and add one circle contour */
path = tpPathCreate();
tpPathAddCircle(path, 400, 300, 100);

/* add another custom contour to the path */
tpPathMoveTo(path, 400, 320);
tpPathLineTo(path, 420, 280);
tpPathQuadraticCurveTo(path, 400, 260, 380, 280);
tpPathClose(path); /* close the contour */

/* create a style that we can draw the path with */
style = tpStyleCreate();
tpStyleSetFillColor(style, 1.0, 1.0, 0.0, 1.0);
tpStyleSetStrokeColor(style, 1.0, 0.6, 0.1, 1.0);
tpStyleSetStrokeWidth(style, 10.0);
tpStyleSetStrokeJoin(style, kTpStrokeJoinRound);

/* ... */

/* call this at the beginning of your frame */
tpPrepareDrawing(&ctx);

/* draw the path with our style */
tpDrawPath(&ctx, path, style);

/* call this when you are done with Tarp for the frame */
tpFinishDrawing(&ctx);

/* ... */

/* clean up tarp */
tpStyleDestroy(style);
tpPathDestroy(path);
tpContextDeallocate(&ctx);
```
Checkout *HelloWorld.c* in the examples folder for the full source of this example using *OpenGL* and *GLFW*.

How does Tarp rasterize
--------
The only available backend right now is a written in *OpenGL*. It is based on the *stencil and cover* method.


Supported Platforms
-------------

Any platform that supports OpenGL 3.0+. Tested on *OSX* and *Linux* so far.

License
-------------

MIT License:

Copyright 2018 Matthias Dörfelt

Permission is hereby granted, free of charge, to any person obtaining a copy of this software and associated documentation files (the "Software"), to deal in the Software without restriction, including without limitation the rights to use, copy, modify, merge, publish, distribute, sublicense, and/or sell copies of the Software, and to permit persons to whom the Software is furnished to do so, subject to the following conditions:

The above copyright notice and this permission notice shall be included in all copies or substantial portions of the Software.

THE SOFTWARE IS PROVIDED "AS IS", WITHOUT WARRANTY OF ANY KIND, EXPRESS OR IMPLIED, INCLUDING BUT NOT LIMITED TO THE WARRANTIES OF MERCHANTABILITY, FITNESS FOR A PARTICULAR PURPOSE AND NONINFRINGEMENT. IN NO EVENT SHALL THE AUTHORS OR COPYRIGHT HOLDERS BE LIABLE FOR ANY CLAIM, DAMAGES OR OTHER LIABILITY, WHETHER IN AN ACTION OF CONTRACT, TORT OR OTHERWISE, ARISING FROM, OUT OF OR IN CONNECTION WITH THE SOFTWARE OR THE USE OR OTHER DEALINGS IN THE SOFTWARE.