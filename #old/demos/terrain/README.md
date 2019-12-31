Author: Juan Becerra (juanbecerra@u.boisestate.edu)
Course: CS 464: Introduction to Computer Graphics
Assignment: A2 - Modeling
Instructor: Steve Cutchin

# Overview

This assignment tasks us with creating a 100 x 100 grid with a height map 
based off of an overlayed texture. Y values of the grid are pulled up 
based on the RGB values of the heightmap. A direction light and ambient 
light is also used to simulate a "sun".

# Grid Geometry

The algorithm approach showed in class was used in the initGeometry() 
method. It essentially breaks down to the following steps:

- Use a doube for-loop to create groups of three position vertices and
    normals (0 - 98 to avoid index out of bound exceptions).
- Generate s and t coordinates that span over the entire grid
- Generate faces based on the groups of three position vertices
- Load all of these values into their respective buffers

Additionally, a scaling matrix is passed into the vertex shader to scale 
the terrain to a more visible scale. The vertex shader also pulls Y 
vertices from the grid based off of the texture/height map (more details 
later).

Interacting with the buttons on the bottom of the page will also change 
values of the identity matrix, which will scale the terrain (more details
in the user interaction section).

# Lighting

Using the grid geometry normals stated in the previous section, lighting 
accomplished using a single direction and ambient light. The initShaders 
method passes different uniform variables into our shaders that will 
affect how our geometry is lit.

Withiin our fragment shader, frag colors is modified based on the RGB 
values in the drawScene() method. Within our vertex shader, ambient 
color, directional color, and directional direction (haha) are brought 
in as uniform variables. From there, light weighting of each face is 
calculated based off of the dot product of the normal and lighting 
direction. This is then added onto whatever ambient lighting is in the 
scene (since ambient lighting would affect all geometry regardless of 
normals).

Interacting with the buttons on the bottom of the page will also change 
lighting colors (more details in user interactions section).

# Texture as Height Map

The heightmap (Heightmap.png) was used to take Y values from the geometry 
and extract them upwards based on that mapped pizel's RGB value. Using 
the generated s and t coordinates back in the grid geometry section and 
texture setup methods, we can load it into our vertex shader. Our main 
method essentially ensures that, while the [0] and [2] maintain their 
originally specified positions, the [1] values will be increased based 
on the heightmap texture.

# User Interaction

The webpage allows users to modify the lighting or transform the terrain 
using the buttons found under the canvas. These options include the option
to change the lighting to white, red, green, blue, or cycle through a 
spectrum. The transformation buttons allow you to switch begin the default
terrain scaling and collapsing the geometry.

Lighting was accomplished by simply setting the directional and ambient 
light colors to global variables and modifying them using event listeners 
attached to each button. White, red, and green are fairly self-
explanatory. Choosing cycle spectrum offsets every color value, and 
increments/decrements every value by 0.01 every frame of the animation. 
The result is an incomplete spectrum cycle that resets every 200 frames.

The tranformation options where accomplished using a similar method to 
the lighting methods. A matrix that modifies values of an identity matrix 
are altered to "collapse" the geometry in on itself, or return it to its 
default viewpoint. Every frame, this modified identity matrix is 
multiplied with the gl_position attribute in the vertex shader. 

I would have liked to add more user interaction, such as interpolated 
movement, panning, and zooming, but I was unfortunately cut short due 
to time constraints. In the future, I'd love to implement these types 
of features into my project.

# Extra Credit

I strived to use a heightmap texture that could generate vertices that 
could simulate a semi-realistic mountain range. Unfortunately, like in 
my user interactions, I was somewhat short on time and could not 
accomplish as much as I would have liked to.