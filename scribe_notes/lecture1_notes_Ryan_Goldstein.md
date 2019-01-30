# Arduino
## The Arduino IDE
- Download the Arduino IDE
- It uses a C-like programming language
- You can use the Arduino IDE to find bugs in your code locally before loading it onto the board (the checkmark)
- The arrow will upload your sketch to the board
- NOTE: Once a sketch is uploaded, it will run until the board loses power!
## Be careful: you can burn your board!
- You can plug things into the pins, be careful that you add a resistor to damp the current before the electronic device
- When in doubt, pass less current rather than more
- Don't touch bare wires without a good idea of what you're touching
## On the board
- A lot of the board components can be considered black box components
- LEDs PX and RX will glow when you upload a sketch (Arduino programs are called sketches)
- The L LED is within your control (it defaults to the on state)
## Configuring your board
- Before you can upload sketches, you need to configure your board
- Under Tools / Board, you should select which one you're using
- Under Tools / Port, you should select where it's plugged in
- Make sure you read the Arduino install instructions correctly
## Compiling and uploading
- You have to save your sketches before compiling and uploading
- Make sure to compile before uploads

# Blender
- It's recommended you download and start playing with Blender ASAP
- Blender features the Cycles rendering engine, which can produce cool results
- The video on [this page](https://www.cs.rutgers.edu/~ma635/papers/SCFSHCP/paper.html) was made in Cycles
- Blender does have some simulation support, but it doesn't have deep support for simulation engines

# An Aside
- "In graphics, you should always be ready to get your hands dirty."
- Graphics is a bleeding-edge field that constantly innovates
- Innovations like the GPU come from graphics bleeding-edge nature, even though it can be used for many other applications

# Autodesk Maya
- If your computer cannot handle rendering locally, Maya will be installed on the iLabs
- It is free for student use but it is commercial software

# Pixar's RenderMan
- You can get educational licenses, but it's only one per student
- Goal is to have it on the iLabs

# Obj file format
- Comprised of the vertices of the triangles in your mesh

# OpenGL
- The graphics library we'll be using to build our code
- We want to use >= 3.0, because <= 2.0 is deprecated
