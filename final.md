#Introduction
Setting up a working environment of modern OpenGL is not a trivial task on the mac. I have adapted my tutorial from Tom Dalling's XCode Project: [getting-started-in-xcode-and-visual-cpp](http://www.tomdalling.com/blog/modern-opengl/01-getting-started-in-xcode-and-visual-cpp). The project includes many dependencies such as:
- glfw
    + Handles Windowing
    + Setting Opengl contexts
- glew
    + Compiling & Linking Shaders
    + Extends OpenGL library
- glm
    + Math operations
    + opengl operations were deprecated in opengl 4
    + useful matrix functions such as gluLookat, gluOrtho, etc

As I do not prefer XCode I have created my own project that is built using a makefile using the g++ compiler. In the video below I walk through how to setup the project and a brief introduction to the tutorial program's features and how to use them. The project is located [here](https://github.com/timthez/shader_tutorial).
----VIDEO----
    
#Requirements
- Mac that supports OpenGL 4.1 Core
    + To determine if your Mac supports v4.1 or to see what features it does support download [opengl-extensions-viewer](https://itunes.apple.com/us/app/opengl-extensions-viewer/id444052073?mt=12) 
- g++ compiler
- make command

You may see around the web and in the opengl-extensions-viewer application the terms opengl compatibility and core version. The compatibility version is simply a version that supports both the old and the new OpenGL graphics pipeline. The differences between the different core versions are as follows:
- After OpenGL 2 shaders became available but were optional
    + Vertex Shaders
    + Fragment Shaders
- Opengl 3 
    + Vertex Shaders required
    + Fragment Shaders
    + Geometry shaders
- OpenGL 4 
    + Vertex Shaders required
    + Fragment Shaders
    + Geometry Shaders
    + Tesselation Shaders
    + Compute Shaders (4.3)


#Overview of Shaders

##OpenGL Fixed Function Graphics Pipeline
Early OpenGL used what was called the fixed function pipeline. It did a couple of things very well but whenever a developer wanted to tweak the way it did something it became a nightmare. It provided some standard ways of doing things but since it was fixed, it was very oppinionated in how things were done. This led to performance hits and lots of hackery to get it to do what was wanted. Eventually some graphics card manufacturers figured that allowing programmable features at critical points in the system greatly helped devlopers. They came up with what was called ARB. This simple language paved the way for the current pipeline.

![Fixed Function Pipeline](http://www.3dgep.com/wp-content/uploads/2014/02/OpenGL-Fixed-Function-Pipeline1.png)

##OpenGL Programmable Graphics Pipeline
Eventually the Programmable Pipeline came into existence and gave developers control over virtually every aspect of the graphics pipeline. This level of control is orchestrated in what are called shader programs or shaders. 

###What are shaders?
Shaders are simply programs that are run on a GPU. Although the name implies that it has to do with shading, shading is one of many things that it can do. In OpenGL shaders are written in the OpenGL Shading Language(GLSL). GLSL is very much like C. 

####Why run programs on a GPU?
GPUs are designed with many processors or cores with specially designed hardware to do certain mathimatical operations very quickly such as matrix transformations. GPU are also designed for throughput of data, meaning lots of data can stream through it very quickly. Shader programs are assigned cores or processors that allow them to run in parallel to each each increasing overall performance. This website provides a much more in depth view if you are curious: [fgiesen.wordpress.com](https://fgiesen.wordpress.com/2011/07/09/a-trip-through-the-graphics-pipeline-2011-index/)

####Shader Types
There are currently 5 different types of shader programs. Each type is responsible for certain tasks in the pipeline. They are:
- Vertex
    + Handle Vertex Transformations (Explained more later) 
- Fragment
    + Handles a group of pixels called a fragment, primarily used for setting color and depth
- Geometry
    + Work on a set of vertices that form a figure
- Tesselation
    + Forming tesselations on objects that can be useful for controlling detail based on distance
- Compute
    + Computes on raw data. Not necessarily graphics specific.

In this tutorial we will be using only the vertex and fragment shaders as they are simpler and most widely supported. Compute Shaders are only available in OpenGl 4.3. 

####What makes up a shader?
A shader is made up of special variables called attributes and uniforms. Attributes are variables that change extremely often and many types are quite large. Attributes are typically used for vertex coordinates or other frequently changing data. Uniforms are variables that are used for much slower data. Typically this may include matrix transformations such as those defined by ```gluLookat```, ```gluOrth```, and ```gluPerspective```. It may also include lighting information or anything else that could be used. A list of all built in variables can be found [here](https://www.opengl.org/wiki/Built-in_Variable_%28GLSL%29).

####How do they fit in the new pipeline? (Ordered)
![Programmable Pipeline](http://www.3dgep.com/wp-content/uploads/2014/02/OpenGL-4.0-Pipeline.png)
- Vertex Shaders
    + Takes Model Coordinates as a vertex array object (VAO) and vertex buffer objects (VBO)
        * Intimately handles what used to happen with glVertex         
        * VBOs
            - Buffers of video memory 
            - Can be anything, simply copies a chunk of memory
        * VAO
            - Link VBO to actual shader variables.
            - Describes type and what shader variable it refers to.
        * Will be discussed more in video demo on full project setup    
    + Output will be in Clipping Coordinates:
        * gl_Position
        * gl_PointSize
        * gl_ClipDistance[]
    + This means the programmer must perform these transformations in the vertex shader to transform from modeling Coordinates to Clipping Coordinates 
        *  Commonly Call "MVP"                
        * Modeling Transformation 
            - (Model Coordinates -> World Coordinates) 
            - Position Model in World
        * Viewing Transformation 
            - (World Coordinates -> View/Eye Coordinates) 
            - Position Camera
        * Projection Transformation 
            - (View/Eye Coordinates -> Clip Coordinates) 
            - Orthographic or Perspective?
        * Clipping Coordinates are then set to the built in gl_Position attribute
            - In the range of -1f to 1f in each direction
        *  Reference: https://en.wikibooks.org/wiki/GLSL_Programming/Vertex_Transformations
- Geometry and Tesselation Shaders
- Rasterization
    + Clip Coordinates -> Normalized device coordinates -> screen/window coordinates
    + performs perspective division using gl_Position.w
- Fragment Shaders
    + After Rasterization, a sample of pixels is generated into whats called a fragment.
    + The Fragment's color and depth can be set in this shader

[Tutorial 1](./tutorial_1.html)
