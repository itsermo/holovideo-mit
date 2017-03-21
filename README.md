![holovideo by MIT Media Lab](http://excedrin.media.mit.edu/wp-content/uploads/sites/10/2013/07/holovideo-300x222.jpg)

# holovideo-mit [![Build Status](https://travis-ci.org/itsermo/holovideo-mit.svg?branch=master)](https://travis-ci.org/itsermo/holovideo-mit)

Holographic Video by MIT Media Lab and BYU Electro-Holography

## Synopsis
The content here spans the work done by many students and researchers over the course of three decades, starting with the [Spatial Imaging](https://www.media.mit.edu/spi/) group in the 1990's, headed by the late Stephen Benton and carried on to the present day by the [Object-based Media](http://obm.media.mit.edu) group, run by V. Michael Bove, Jr and [BYU Electro-Holography](https://holography.byu.edu/) run by Daniel Smalley, a former student of MIT Media Lab.

Here you can find the implementation of some canonical holovideo display algorithms published in papers and conference proceedings, and the source code for an interactive end-to-end 3D telepresence software suite for advanced 3D and holographic displays.

## Motivation

This code is being released to document some methods of generating real-time holographic images.  We hope that hobbyists and researchers around the world can be inspired by and use this code to help achieve the goal of creating high-quality consumer holovideo displays.

## Real-time Computer-generated Hologram (CGH) Algorithms

This is a collection of algorithms that generate interference patterns in real time for holographic video displays.  These algorithms generally work by taking some 3D input data (e.g. depth buffer from OpenGL), generating a chirp function for each depth value and then modulating the chirp function by some color/luminance/texture data.

Algorithms using physically-based interference modeling are more computationally involved, as they performs an actual physical simulation of wavefront interference on a simulated hologram plane.  This is also known as "Fresnel" computation, and an example of this is given in the last entry.

- [dscp4](holovideo-src/render_algorithms/dscp4) - Diffraction-specific Coherent Panoramagram for Full-Color Holographic Displays [[PAPER]](http://obm.media.mit.edu/wp-content/uploads/sites/10/2012/09/PW2015.pdf) 2015
- [holodepth](holovideo-src/render_algorithms/holodepth) - Diffraction-specific Coherent Panoramagram for Monochromatic Holographic Displays w/Accomodation Cues [[PAPER]](http://excedrin.media.mit.edu/wp-content/uploads/sites/10/2013/07/7619-02.pdf) 2012
- [ripgen-fbo](holovideo-src/render_algorithms/ripgen-fbo) - Reconfigurable Image Projection Holograms [[PAPER]](http://excedrin.media.mit.edu/wp-content/uploads/sites/10/2013/07/JOE115801.pdf) 2006
- [fresnel-mkii](holovideo-src/render_algorithms/fresnel-mkii) - Physically-based Interference Modeling Algorithm (Fresnel-Huygens wavelet interference computation) for monochrome MIT Mark II display in Cuda - 2012

For a good overview of these methods and the theory behind computer generated holograms for holovideo displays, refer to Benton, S.A. and Bove, V.M. Jr. (2008) *[Holographic Imaging](http://www.wiley.com/WileyCDA/WileyTitle/productCd-047006806X.html)* (pp. 207-231). Hoboken, NJ: John Wiley and Sons, Inc. Chapter 19 - Computational Display Holography.

## Holographic Telepresence

- [holosuite](holosuite-src) - An interactive end-to-end 3D telepresence software that takes input from OpenNI depth-sensing cameras, such as PrimeSense, and outputs to Z-Space 3D displays, MIT Mark II and MIT/BYU Mark IV holographic displays.  Two users can simultaneously talk to each other and collaborate in a shared 3D environment [[PAPER]](http://obm.media.mit.edu/wp-content/uploads/sites/10/2012/09/dreshaj_ermal_holosuite_thesis_v6_final.pdf) [[VIDEO]](https://vimeo.com/136748726)

## Compiling

Some care has been taken to make the code cross-platform using CMAKE.  However, the two latest building environments tested during development were Windows 10 + VS2015 and Ubuntu Linux 14.04 + Eclipse IDE, so YMMV on other platforms.  Code is written in C++, Cuda and makes use of various open-source libraries such as Boost, Qt, OpenCL and OpenGL.

Each project can be compiled individually using CMAKE in its respective folder.

### Linux

  cd eclipse-projects
  ./generate-holo-eclipse-projects.sh`

This will generate eclipse projects for libdscp4 (library for computing full-color holographic video), dscp4-qt (GUI frontend for libdscp4 that can load 3D models) and holosuite (3D telepresence).

### Windows

Use CMAKE to generate Visual Studio 2015 projects for rendering algorithms, such as libdscp4, dscp4-qt and/or holosuite.

## Holovideo Displays FAQ
**Q: What is a hologram?**

A: A hologram can most succinctly be described as a 2D encoding of a 3D light wavefront.  Holography is the art and science of capturing and re-playing the full information of light from a scene incident on a film.  Think of a hologram as being like a window that can store the light from the outside scenery.  Imagine if we could save that scenery in the glass pane of a window, and take it with us anywhere we want, and replay that view with depth, motional parallax, focus, specular highlights and everything about light that makes it highly realistic.  This is possible via the holographic method, which is to indirectly encode the phase information of light on a 2D generalized diffraction grating structure.

**Q: How do holographic video displays work? And what makes them "holographic"?**

A: Generally speaking, a true "holographic" video display is some display technology that uses the principles of light diffraction to reconstruct wavefronts of light.  If we think of light as a wave signal, the principles of holography dictate that we can use diffraction to alter not only the amplitude, but also the phase of light, which is where the realism and depth of scenery is encoded.  In lay terms, a holographic display is one that can create a 3D image with full motion parallax and focus cues that provide a viewing experience with high realism.

Holographic displays use some type of spatial light modulator (SLM), a device that can dynamically generate diffraction patterns that can alter the phase and/or amplitude of light.  Think of an SLM as a programmable dynamic diffraction grating, that is updated or changed for every frame in an animation.  The programming of an SLM requires very compute-intensive algorithms that can generate the appropriate signal to focus and steer beams of light at different positions.

There are a few types of SLMs that are typically used in holovideo displays.

- LCoS (Liquid Crystal on Silicon) is a technology that can change the index of refraction dynamically on an per-pixel addressable surface (this effectively allows us to alter the phase of light signal).  If the pixel size in an LCoS is very small (less than a few micro-meters), we can focus lights at different depths and generate an image with motion parallax from a range of angles.  Due to low space-bandwidth product, we need lots and lots of pixels at very very small sizes to make a wide-FOV large display.  Current LCoS devices are tiny and pixel sizes are too big to deflect light more than a few angles (small FOV).

- DMD (Digital Micromirror Device, branded as DLP) can be used to modulate amplitude of light.  Since these devices can be driven at incredibly high frequencies, they can be used in scanning solutions to create big images.  Amplitude modulation is inefficient, however and DMDs still suffer from low space-bandwidth product problems of LCoS (need very tiny pixels and lots and lots of them).

- AOM (Acousto-optic modulator) is a class of devices that can diffract light by generating an acoustic wave.  Bulk-wave AOMs were traditionally used for telecom operators to switch fiber optic lines.  This technology was first used by the Spatial Imaging Group to make the world's first holovideo display.  Bulk-wave acousto-optic modulators suffered from the pesky space-bandwidth problem, as well. Today, surface acoustic wave modulators carry on this tradition but get around the space-bandwidth product problem, allowing us to generate large images with large FOV (60 degrees).  With these modulators, we have a huge pixel budget that allows us to dynamically trade between high resolution, decent FOV, or high FOV and decent resolution, for example.

The algorithms in this repo are meant to work with a class of holovideo displays based on acousto-optic modulators.  However, the underlying math and principles are the same and therefore with a little massaging the algorithms can easily be applied to DMD and LCoS holovideo displays.

**Q: What is a Computer-generated hologram (CGH)?**

A: If we think of a traditional hologram like a traditional photograph made from film, then a CGH is the digital equivalent of a hologram, much like a JPEG is a digital representation of a photograph.  Traditional (analog) holograms are typically recorded on a glass plate coated with a photo-sensitive emulsion layer, such as silver halide.  Like film photographs, they are put through a photochemical development process before the viewer can see the image.  A computer-generated hologram contains a digital representation of the structure in a recorded and developed hologram (known as the fringe pattern, or interference pattern), just like a JPEG contains digital information of the structure of a photograph.

The JPEG analogy holds pretty well in many cases, however unlike a digital photograph, where every point (pixel) in the picture is independent of every other pixel, a CGH contains much richer information about the light of the scene, where every point in the scene depends on every other point in the scene.

**Q: How do CGH algorithms work?**

A: One of the most basic methods is known as physically-based interference modeling (or Fresnel computation).  The premise is similar to ray-tracing for 3D graphics generation and has been known since the 1970's.  In physically-based intereference modeling, a 3D scene is treated as a collection of spherical light source emitters for every 3D point in the scene.  This type of algorithm models the interference of each point emitting a wave of light in the scene, as it interferes with every other point's wave, as seen from some viewing plane in 3D space (known as a the hologram plane).  This class of algorithms are very compute intensive but produce the most accurate model of a traditional hologram, and so are considered the gold standard of producing images with high realism.

Computer-generated holography is still an active area of research, however.  More modern approaches cleverly use the high space-bandwidth of an SLM to produce light fields by generating independent abutting chirps, or zone plate structures.  This approach is more concerned with creating an approximation of the final resulting light field of a hologram, rather then simulating the physics of light interference.  The end visual result is comparable to Fresnel computation (providing focus, depth and motion parallax of high realism), but with computation effort reduced by orders of magnitude.

**Q: Where can you get a holographic video display?**

A: You have to build one!

Eventually, we hope to have a better open-source documentation for making your own holovideo displays.  For now, a great start would be to read Daniel Smalley's thesis *[Holovideo on a Stick](http://obm.media.mit.edu/wp-content/uploads/sites/10/2012/09/SmalleyDissertation.pdf)* and the documentation on building a Mark IV display found [here](https://holography.byu.edu/HoloVideoMonitor).

Making the most important component, the surface acoustic wave light modulator is still an area of active research.  You can build a simpler display using old, off-the-shelf bulk-wave acousto-optic modulators found on 2nd-hand optics bags and such from places like eBay.

## License

The code is licensed under the [3-clause BSD open source license](LICENSE)
