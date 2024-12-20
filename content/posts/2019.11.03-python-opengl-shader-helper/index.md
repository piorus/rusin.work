---
date: '2019-11-03T18:41:32+01:00'
draft: false
title: 'OpenGL Shader helper (Python)'
slug: 'python-opengl-shader-helper'
---

based on https://learnopengl.com/Getting-started/Shaders

```python
from OpenGL.GL import *
import glm, ctypes
 
class ShaderFromSource:
    def __init__(self, vertex_source, fragment_source):
        # Step 1 - creating vertex shader
        self.vertex = self.create_shader(GL_VERTEX_SHADER, vertex_source)
        # Step 2 - creating fragment shader
        self.fragment = self.create_shader(GL_FRAGMENT_SHADER, fragment_source)
        self.program = self.create_shader_program(self.vertex, self.fragment)
 
    def create_shader_program(self, vertex_shader, fragment_shader):
        # Step 3 - creating shader program
        shader_program = glCreateProgram()
        # Step 4 - attaching vertex & fragment shaders
        glAttachShader(shader_program, vertex_shader)
        glAttachShader(shader_program, fragment_shader)
        glBindFragDataLocation(shader_program, 0, "outColor")
        # Step 5 - linking shader program
        glLinkProgram(shader_program)
 
        return shader_program
 
    def create_shader(self, shader_type, source):
        """compile a shader"""
        shader = glCreateShader(shader_type)
        glShaderSource(shader, source)
        glCompileShader(shader)
        if glGetShaderiv(shader, GL_COMPILE_STATUS) != GL_TRUE:
            raise RuntimeError(glGetShaderInfoLog(shader))
 
        return shader
 
    def use(self):
        glUseProgram(self.program)
    def get_location(self, name):
        return glGetUniformLocation(self.program, name)
    def set_bool(self, name, value):
        glUniform1i(self.get_location(name), value)
    def set_int(self, name, value):
        glUniform1i(self.get_location(name), value)
    def set_float(self, name, value):
        glUniform1f(self.get_location(name), value)
    def _set_vector(self, name, vector_callback, points_callback, args):
        if(len(args) == 1):
            vector_callback(self.get_location(name), 1, glm.value_ptr(args[0]))
        else:
            points_callback(self.get_location(name), *(GLfloat * len(args))(*args))
    def set_vec2(self, *args):
        self._set_vector(args[0], glUniform2fv, glUniform2f, args[1:])
    def set_vec3(self, *args):
        self._set_vector(args[0], glUniform3fv, glUniform3f, args[1:])
    def set_vec4(self, *args):
        self._set_vector(args[0], glUniform4fv, glUniform4f, args[1:])
    def set_mat2(self, name, mat):
        glUniformMatrix2fv(self.get_location(name), 1, GL_FALSE, glm.value_ptr(mat))
    def set_mat3(self, name, mat):
        glUniformMatrix3fv(self.get_location(name), 1, GL_FALSE, glm.value_ptr(mat))
    def set_mat4(self, name, mat):
        glUniformMatrix4fv(self.get_location(name), 1, GL_FALSE, glm.value_ptr(mat))
 
class Shader(ShaderFromSource):
    def __init__(self, vertex_path, fragment_path):
        super().__init__(self.read_from_file(vertex_path), self.read_from_file(fragment_path))
 
    def read_from_file(self, path):
        # Loading shader source
        with open(path, 'r') as file:
            return file.read()
```