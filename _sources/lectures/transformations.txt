Transformations
===============

Open a window and type in the following code: ::

    #include <GL/glew.h>
    #include <GLFW/glfw3.h>
    #include <iostream>
    #include <SOIL.h>
    
    #define GLM_FORCE_RADIANS
    #include <glm/glm.hpp>
    #include <glm/gtc/matrix_transform.hpp>
    #include <glm/gtc/type_ptr.hpp>
    
    #include "Shader.h"
    
    GLfloat mix_value=0.2f;
    
    void key_callback(GLFWwindow* window,int key,int scancode,int action,int mode)
    {
        if(key==GLFW_KEY_ESCAPE && action==GLFW_PRESS)
            glfwSetWindowShouldClose(window,GL_TRUE);
    
        if(key==GLFW_KEY_UP && action==GLFW_PRESS)
        {
            mix_value+=0.1f;
            if(mix_value>1.0f) mix_value=1.0f;
        }
    
        if(key==GLFW_KEY_DOWN && action==GLFW_PRESS)
        {
            mix_value-=0.1f;
            if(mix_value<0.0f) mix_value=0.0f;
        }
    }
    
    int main()
    {
        glfwInit();
    #if __APPLE__
        glfwWindowHint(GLFW_OPENGL_FORWARD_COMPAT,GL_TRUE);
    #endif
        glfwWindowHint(GLFW_CONTEXT_VERSION_MAJOR,3);
        glfwWindowHint(GLFW_CONTEXT_VERSION_MINOR,3);
        glfwWindowHint(GLFW_OPENGL_PROFILE,GLFW_OPENGL_CORE_PROFILE);
        glfwWindowHint(GLFW_RESIZABLE,GL_FALSE);
    
        GLFWwindow *window=glfwCreateWindow(800,600,"Learn OpenGL",nullptr,nullptr);
        if(window==nullptr)
        {
            std::cout<<"Failed to create GLFW window!"<<std::endl;
            glfwTerminate();
            return -1;
        }
        glfwMakeContextCurrent(window);
    
        glewExperimental=GL_TRUE;
        if(glewInit()!=GLEW_OK)
        {
            std::cout<<"Failed to initialize GLEW!"<<std::endl;
            return -1;
        }
    
        int width,height;
        glfwGetFramebufferSize(window,&width,&height);
        glViewport(0,0,width,height);
    
        glfwSetKeyCallback(window,key_callback);
    
        Shader our_shader("shader.vs","shader.frag");
    
        GLuint texture1,texture2;
    
        // generate texture 1
        glGenTextures(1,&texture1);
        glBindTexture(GL_TEXTURE_2D,texture1);
        // set texture parameters
        glTexParameteri(GL_TEXTURE_2D,GL_TEXTURE_WRAP_S,GL_CLAMP_TO_EDGE);
        glTexParameteri(GL_TEXTURE_2D,GL_TEXTURE_WRAP_T,GL_CLAMP_TO_EDGE);
        // set texture filtering
        glTexParameteri(GL_TEXTURE_2D,GL_TEXTURE_MIN_FILTER,GL_NEAREST);
        glTexParameteri(GL_TEXTURE_2D,GL_TEXTURE_MAG_FILTER,GL_NEAREST);
        // load, create and generate mipmaps
        unsigned char* image=SOIL_load_image("container.jpg",&width,&height,0,SOIL_LOAD_RGB);
        glTexImage2D(GL_TEXTURE_2D,0,GL_RGB,width,height,0,GL_RGB,GL_UNSIGNED_BYTE,image);
        glGenerateMipmap(GL_TEXTURE_2D);
        // free image data
        SOIL_free_image_data(image);
        glBindTexture(GL_TEXTURE_2D,0);
    
        // generate texture 2
        glGenTextures(1,&texture2);
        glBindTexture(GL_TEXTURE_2D,texture2);
        // set texture parameters
        glTexParameteri(GL_TEXTURE_2D,GL_TEXTURE_WRAP_S,GL_REPEAT);
        glTexParameteri(GL_TEXTURE_2D,GL_TEXTURE_WRAP_T,GL_REPEAT);
        // set texture filtering
        glTexParameteri(GL_TEXTURE_2D,GL_TEXTURE_MIN_FILTER,GL_NEAREST);
        glTexParameteri(GL_TEXTURE_2D,GL_TEXTURE_MAG_FILTER,GL_NEAREST);
        // load, create and generate mipmaps
        image=SOIL_load_image("awesomeface.png",&width,&height,0,SOIL_LOAD_RGB);
        glTexImage2D(GL_TEXTURE_2D,0,GL_RGB,width,height,0,GL_RGB,GL_UNSIGNED_BYTE,image);
        glGenerateMipmap(GL_TEXTURE_2D);
        // free image data
        SOIL_free_image_data(image);
        glBindTexture(GL_TEXTURE_2D,0);
    
        GLfloat vertices[]={
            // positions        // colors           // textures
             0.5f, 0.5f, 0.0f,  1.0f, 0.0f, 0.0f,   1.0f, 1.0f,
             0.5f,-0.5f, 0.0f,  0.0f, 1.0f, 0.0f,   1.0f, 0.0f,
            -0.5f,-0.5f, 0.0f,  0.0f, 0.0f, 1.0f,   0.0f, 0.0f,
            -0.5f, 0.5f, 0.0f,  1.0f, 1.0f, 0.0f,   0.0f, 1.0f
        };
    
        GLuint indices[]={
            0, 1, 3,
            1, 2, 3
        };
    
        GLuint VAO,VBO,EBO;
        glGenBuffers(1,&VBO);
        glGenBuffers(1,&EBO);
        glGenVertexArrays(1,&VAO);
    
        // bind vertex array object
        glBindVertexArray(VAO);
    
        // copy the vertices in a buffer
        glBindBuffer(GL_ARRAY_BUFFER,VBO);
        glBufferData(GL_ARRAY_BUFFER,sizeof(vertices),vertices,GL_STATIC_DRAW);
        // copy the index array in an element buffer
        glBindBuffer(GL_ELEMENT_ARRAY_BUFFER,EBO);
        glBufferData(GL_ELEMENT_ARRAY_BUFFER,sizeof(indices),indices,GL_STATIC_DRAW);
    
        // set position attribute pointers
        glVertexAttribPointer(0,3,GL_FLOAT,GL_FALSE,8*sizeof(GL_FLOAT),(GLvoid*)0);
        glEnableVertexAttribArray(0);
        // set color attribute pointers
        glVertexAttribPointer(1,3,GL_FLOAT,GL_FALSE,8*sizeof(GL_FLOAT),(GLvoid*)(3*sizeof(GLfloat)));
        glEnableVertexAttribArray(1);
        // set texture attribute pointers
        glVertexAttribPointer(2,2,GL_FLOAT,GL_FALSE,8*sizeof(GL_FLOAT),(GLvoid*)(6*sizeof(GLfloat)));
        glEnableVertexAttribArray(2);
    
        // unbind the vertex array object
        glBindVertexArray(0);
    
        while(!glfwWindowShouldClose(window))
        {
            glfwPollEvents();
            glClearColor(.2f,.3f,.3f,1.f);
            glClear(GL_COLOR_BUFFER_BIT);
    
            // use shader program
            our_shader.Use();
    
            // bind textures
            glActiveTexture(GL_TEXTURE0);
            glBindTexture(GL_TEXTURE_2D,texture1);
            glUniform1i(glGetUniformLocation(our_shader.program,"our_texture1"),0);
            glActiveTexture(GL_TEXTURE1);
            glBindTexture(GL_TEXTURE_2D,texture2);
            glUniform1i(glGetUniformLocation(our_shader.program,"our_texture2"),1);
    
            glUniform1f(glGetUniformLocation(our_shader.program,"mix_value"),mix_value);
    
            // generate transformation matrix
            glm::mat4 trans(1.0f);
            trans=glm::translate(trans,glm::vec3(0.5f,-0.5f,0.0f));
            trans=glm::rotate(trans,glm::radians((GLfloat)glfwGetTime()*50.0f),glm::vec3(0.0f,0.0f,1.0f));
    
            GLuint transform_location=glGetUniformLocation(our_shader.program,"transform");
            glUniformMatrix4fv(transform_location,1,GL_FALSE,glm::value_ptr(trans));
    
            // draw
            glBindVertexArray(VAO);
            glDrawElements(GL_TRIANGLES,6,GL_UNSIGNED_INT,0);
            glBindVertexArray(0);
    
            glfwSwapBuffers(window);
        }
    
        // deallocate all resources
        glDeleteVertexArrays(1,&VAO);
        glDeleteBuffers(1,&VBO);
        glDeleteBuffers(1,&EBO);
        // terminate GLFW
        glfwTerminate();
    
        return 0;
    }

Save this file as ``main.cpp``. Open another window and type in the following
code: ::

    #version 330 core
    layout (location=0) in vec3 position;
    layout (location=1) in vec3 color;
    layout (location=2) in vec2 tex_coord;
    
    out vec3 our_color;
    out vec2 TexCoord;
    
    uniform mat4 transform;
    
    void main()
    {
        gl_Position=transform*vec4(position,1.0f);
        our_color=color;
        TexCoord=vec2(tex_coord.x,1.0f-tex_coord.y);
    }

Save this file as ``shader.vs``. Finally, open another window and type the
following code: ::

    #version 330 core
    in vec3 our_color;
    in vec2 TexCoord;
    
    out vec4 color;
    
    uniform float mix_value;
    
    uniform sampler2D our_texture1;
    uniform sampler2D our_texture2;
    
    void main()
    {
        color=mix(texture(our_texture1,TexCoord),texture(our_texture2,TexCoord),mix_value);
    }

Save this file as ``shader.frag``.
