========================================
gl3w: Simple OpenGL core profile loading
========================================

Introduction
------------

gl3w_ is the easiest way to get your hands on the functionality offered by the
OpenGL core profile specification.

Its main part is a simple gl3w_gen.py_ Python script that downloads the
Khronos_ supported glcorearb.h_ header and generates gl3w.h and gl3w.c from it.
Those files can then be added and linked (statically or dynamically) into your
project.

Requirements
------------

gl3w_gen.py_ requires Python version 2.7 or newer.
It is also compatible with Python 3.x.

Example
-------

Here is a simple example of using gl3w_ with glut. Note that GL/gl3w.h must be
included before any other OpenGL related headers::

    #include <stdio.h>
    #include <GL/gl3w.h>
    #include <GL/glut.h>

    // ...

    int main(int argc, char **argv)
    {
            glutInit(&argc, argv);
            glutInitDisplayMode(GLUT_RGBA | GLUT_DEPTH | GLUT_DOUBLE);
            glutInitWindowSize(width, height);
            glutCreateWindow("cookie");

            glutReshapeFunc(reshape);
            glutDisplayFunc(display);
            glutKeyboardFunc(keyboard);
            glutSpecialFunc(special);
            glutMouseFunc(mouse);
            glutMotionFunc(motion);

            if (gl3wInit()) {
                    fprintf(stderr, "failed to initialize OpenGL\n");
                    return -1;
            }
            if (!gl3wIsSupported(3, 2)) {
                    fprintf(stderr, "OpenGL 3.2 not supported\n");
                    return -1;
            }
            printf("OpenGL %s, GLSL %s\n", glGetString(GL_VERSION),
                   glGetString(GL_SHADING_LANGUAGE_VERSION));

            // ...

            glutMainLoop();
            return 0;
    }

If your Project is using CMake you can import gl3w_ using ``FetchContent``::

    include(FetchContent)
    FetchContent_Declare(
        gl3w
        GIT_REPOSITORY https://github.com/Sebastian-Dawid/gl3w.git
        GIT_TAG        master
        GIT_SHALLOW    TRUE
        GIT_PROGRESS   TRUE
        EXCLUDE_FROM_ALL
    )
    FetchContent_MakeAvailable(gl3w)
    # Declare your target that needs gl3w here
    target_link_libraries(<target> PRIVATE gl3w)

API Reference
-------------

The gl3w_ API consists of just three functions:

``int gl3wInit(void)``

    Initializes the library. Should be called once after an OpenGL context has
    been created. Returns ``0`` when gl3w_ was initialized successfully,
    ``non-zero`` if there was an error.

``int gl3wIsSupported(int major, int minor)``

    Returns ``1`` when OpenGL core profile version *major.minor* is available
    and ``0`` otherwise.

``GL3WglProc gl3wGetProcAddress(const char *proc)``

    Returns the address of an OpenGL extension function. Generally, you won't
    need to use it since gl3w_ loads all functions defined in the OpenGL core
    profile on initialization. It allows you to load OpenGL extensions outside
    of the core profile.

Options
-------

The generator script optionally takes the arguments:

``--ext`` to include the GL Extensions in output header.

``--root=outputdir`` to set the location for the output to something else than current working directory.

License
-------

.. image:: public-domain-mark.png

gl3w_ is in the public domain. See the file UNLICENSE for more information.

Credits
-------

Slavomir Kaslev <slavomir.kaslev@gmail.com>
    Initial implementation

Kelvin McDowell
    Mac OS X support

Sjors Gielen
    Mac OS X support

Travis Gesslein
    Patches regarding glcorearb.h

Arthur Tombs
    Port to Python 3

Daniel Cousens [https://github.com/dcousens]
    Code contributions

Copyright
---------

OpenGL_ is a registered trademark of SGI_.

.. _gl3w: https://github.com/skaslev/gl3w
.. _gl3w_gen.py: https://github.com/skaslev/gl3w/blob/master/gl3w_gen.py
.. _glcorearb.h: https://www.opengl.org/registry/api/GL/glcorearb.h
.. _OpenGL: http://www.opengl.org/
.. _Khronos: http://www.khronos.org/
.. _SGI: http://www.sgi.com/
