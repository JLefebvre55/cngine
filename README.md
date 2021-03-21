# Cngine

> Pronounced "SEE-(en)-jin"

A rudimentary attempt at a physics and graphics engine for simulations and video games written entirely in C.

# Reverse Engineering

## Sequence

### Main
1. [110] Create global window; pass init, destroy, tick, update, and render functions; initializes GLFW and creates window, callbacks, etc.
2. [111] Call window main loop (while not waiting to close window) \[`window.c`\]
   1. **Init**
   2. Frame/tick length and FPS/TPS calculations (calls **Tick**)
   3. **Update**
   4. **Render**
   5. Call GLFW buffer swap and event poll

### Init
1. [11] Main *State* struct declared
2. [15] Pointer to **global** window passed to state struct
4. [16] Initialize renderer
5. \> *Call other inits, pass relevant state field pointers*
6. [19] Grab mouse
7. \> *Rest of engine setup stuff*

### Tick
1. Increment state ticks count
2. \> *Perform other tick-based operations, pass relevant state field pointers*

### Update
1. [81] Update renderer
2. \> *Call other updates, pass relevant state field pointers*
3. [91] Check window for ESC pressed -> toggle mouse grab
    
### Render
1. [97] Prepare renderer 3D pass-by (?)
2. [98] Prepare renderer 2D pass-by (?)
3. [99-106] Push camera, set camera, pop camera (?) 

## Objects

State (struct) \[`state.h`\]
 - struct Window\* *window* (unallocated) \[`window.h`\]
 - size_t ticks - holds the tick count (?)
 - struct Renderer *renderer* \[`renderer.h`\]

Window (struct) \[`window.h`\]
 - WIP. A wrapper for GLFW window.

Renderer (struct) \[`renderer.h`\]
- enum *CameraType* - Enum of camera types (orthographic, perspective)
- struct *camera_stack*, contains:
  - enum CameraType[] *array* - contains a stack of different cameras
  - size_t *size* - number of cameras
- struct PerspectiveCamera *perspective_camera*
- struct OrthoCamera *ortho_camera*
- struct Shader[] *shaders* - Purpose NYI
- enum ShaderType *current_shader* - one of SHADER_NONE, SHADER_CHUNK, SHADER_SKY, SHADER_BASIC_TEXTURE, SHADER_BASIC_COLOR (\*to be revised\*)
- struct Shader shader
- vec4s clear_color - Purpose unclear
- struct VBO vbo, ibo - Purpose NYI
- struct VAO vao - Purpose NYI
- struct *flags* - contains bool wireframe

vec4s \[CGLM\] - 4D vector of floats x,y,z,w

### Cameras

> \[`camera.h`\]

PerspectiveCamera (struct)
- WIP

OrthoCamera (struct)
- WIP

# Development

## Building

## Unix-like
`$ git clone --recurse-submodules https://github.com/JLefebvre55/cngine.git`\
`$ make`

The following static libraries under `lib/` must be built before the main project can be built:

- GLAD `lib/glad/src/glad.o`
- CGLM `lib/cglm/.libs/libcglm.a`
- GLFW `lib/glfw/src/libglfw3.a`
- libnoise `lib/noise/libnoise.a`

All of the above have their own Makefile under their respective subdirectory and can be built with `$ make libs`.
If libraries are not found, ensure that submodules have been cloned.

The game binary, once built with `$ make`, can be found in `./bin/`.

*Be sure* to run with `$ ./bin/game` out of the root directory of the repository.
If you are getting "cannot open file" errors (such as "cannot find ./res/shaders/*.vs"), this is the issue. 

## Windows

WIP