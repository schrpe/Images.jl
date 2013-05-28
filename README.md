# Images.jl

An image processing library for [Julia](http://julialang.org/).

## Aims

Images are very diverse.
You might be working with a single photograph, or you
might be processing MRI scans from databases of hundreds of subjects.
In the
former case, you might not need much information about the image; perhaps just
the pixel data itself is sufficient.
In the later case, you probably need to
know a lot of extra details, like the patient's ID number and characteristics of
the image like the physical size of a voxel in all three dimensions.

Even the raw pixel data can come in several different flavors:
- For example, you might represent each pixel as a `Uint32` because you are encoding red, green, and blue in separate 8-bit words within each integer---visualization libraries like Cairo use these kinds of representations, and you might want to interact with those libraries efficiently.
Alternatively, perhaps you're an astronomer and your camera has such high precision that 16 bits aren't enough to encode grayscale intensities.
- If you're working with videos (images collected over time), you might have arrays that are too big to load into memory at once.
You still need to be able to "talk about" the array as a whole, but it may not be trivial to adjust the byte-level representation to match some pre-conceived storage order.

To handle this diversity, we've endeavored to take a "big tent" philosophy.
We avoid imposing a strict programming model, because we don't want to make life
difficult for people who have relatively simple needs.
If you do all your image
processing with plain arrays (as is typical in Matlab, for example), that should
work just fine---you just have to respect certain conventions, like a
`m`-by-`n`-by-`3` array always means an RGB image with the third dimension
encoding color.
You can call the routines that are in this package, but write
your own custom algorithms to handle `Array`s.

But if your images don't fit neatly into these assumptions, you can choose to
represent your images using other schemes; you can then tag them with enough
metadata that there's no ambiguity about the meaning of anything.
The algorithms
in this package are already set to look for certain types of metadata, and
adjust their behavior accordingly.

One of the potential downsides of flexibility is complexity---it makes it harder
to write generic algorithms that work with all these different representations.
We've tried to mitigate this downside by providing many short utility functions
that abstract away much of the complexity.
Many algorithms require just a
handfull of extra lines to work generically.
Or if you just want to get
something running, it usually only takes a couple of lines of code to assert
that the input is in the format you expect.

## Installation

Install via the package manager,

```
Pkg.add("Images")
```

It's helpful to have ImageMagick installed on your system, as this relies on it for reading and writing many common image types.

## Image viewing

All code for the display of images has been moved to [ImageView](https://github.com/timholy/ImageView.jl).

## Documentation ##

Detailed documentation about the design of the library
and the available functions
can be found in the `doc/` directory. Here are some of the topics available:

- [Getting started](doc/usage.md)
- The [core](doc/core.md), i.e., the representation of images
- [I/O](doc/extendingIO.md) and custom image file formats

## Status/TODOs

### Data representation:

The central core seems to be in fairly good shape, with new utility functions still being added.

### I/O

- A framework for generic I/O,
  using the magic bytes of the file,
  is already in place. Several custom image formats already employ it.
- Image writing does not yet support transparency, but works otherwise. Reading supports transparency for RGB images.
- TIFF reading can be found at https://github.com/rephorm/TIFF.jl
  and should be incorporated.

### Algorithms

Most algorithms have been ported to work with `Image` types.
The glaring
exception are the colorspace conversion tools, which still assume `Array`s.
Looking for volunteers here,
since the maintainer doesn't use them at all.
The best plan may be to eliminate them in preference for the tools available in [Color.jl](https://github.com/JuliaLang/Color.jl).

# Credits

Many elements of this package descend from "image.jl"
that once lived in Julia's `extras/` directory.
That file had several authors, of which the primary were
Jeff Bezanson, Stefan Kroboth, Tim Holy, Mike Nolta, and Stefan Karpinski.
This repository has been quite heavily reworked;
the current package maintainer is Tim Holy.
