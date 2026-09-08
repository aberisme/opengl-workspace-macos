# OpenGL Workspace for macOS

A ready-to-use C++ OpenGL and GLUT workspace for macOS. It supports Apple Silicon (M1, M2, M3, M4, and later) and Intel Macs by using Apple Clang and the OpenGL/GLUT frameworks included with the macOS development tools.

## Requirements

- macOS on Apple Silicon or Intel
- [Visual Studio Code](https://code.visualstudio.com/)
- The **C/C++** extension by Microsoft (`ms-vscode.cpptools`)
- Apple Command Line Tools
- Git, which is installed with the Command Line Tools

## One-time macOS setup

Open Terminal and install the Apple Command Line Tools:

```bash
xcode-select --install
```

If macOS reports that they are already installed, continue. Verify the compiler:

```bash
/usr/bin/clang++ --version
```

Optional: check the processor architecture:

```bash
uname -m
```

`arm64` means Apple Silicon; `x86_64` means Intel.

## Get the workspace

Clone the repository:

```bash
git clone https://github.com/YOUR-USERNAME/opengl-workspace-macos.git
cd opengl-workspace-macos
code .
```

If the `code` command is unavailable, open VS Code, select **File → Open Folder**, and choose `opengl-workspace-macos`.

> Always open the repository folder itself in VS Code. Do not open only a `.cpp` file or the `.vscode` folder.

## Run a program

Each `.cpp` file is treated as an independent program and should contain its own `main()` function.

1. Open the `.cpp` file you want to run.
2. Click inside its editor tab so it becomes the active file.
3. Save it with `Command+S`.
4. Press `Command+Shift+B` to build the active file.
5. Press `F5` to build and debug, or `Control+F5` to run without debugging.

For example, opening `triangle.cpp` builds an executable named `triangle` beside the source file.

You can also open the Command Palette with `Command+Shift+P`, select **Tasks: Run Task**, and choose **Build and Run Active OpenGL File**.

## Expected repository structure

```text
opengl-workspace-macos/
├── .vscode/
│   ├── c_cpp_properties.json
│   ├── launch.json
│   └── tasks.json
├── main.cpp
└── README.md
```

The Windows `toolchain/ucrt64` directory is neither needed nor compatible with macOS. This workspace uses `/usr/bin/clang++`.

## Source-code header

Use this conditional header to keep the same source code compatible with macOS and Windows:

```cpp
#ifdef __APPLE__
#define GL_SILENCE_DEPRECATION
#include <GLUT/glut.h>
#else
#include <GL/glut.h>
#endif
```

The macOS build command links the system frameworks:

```bash
/usr/bin/clang++ main.cpp -o main -framework OpenGL -framework GLUT
```

OpenGL and GLUT are deprecated by Apple but remain suitable for the legacy classroom examples in this repository.

## macOS security prompt

The first launch may cause macOS to request permission for VS Code or the program to access files or open a window. Approve the prompt if it refers to the workspace you just cloned.

## Troubleshooting

### `clang++` is unavailable

Run:

```bash
xcode-select --install
```

Then restart VS Code after installation.

### The task refers to `g++.exe` or `ucrt64`

You have opened the Windows repository or copied its `.vscode` configuration. The macOS task must use:

```text
/usr/bin/clang++
```

It must not contain `.exe`, Windows backslashes, or `toolchain/ucrt64` paths.

### `GL/glut.h` is not found

On macOS the header is:

```cpp
#include <GLUT/glut.h>
```

Check that the source uses the conditional header shown above.

### The program builds but no window appears

Check the Dock and Mission Control because the OpenGL window may open behind VS Code. Graphics appear in a separate window, not as terminal text.

### The Run Code button fails

The **Run Code** button usually belongs to the Code Runner extension and does not use this repository's framework options. Use `Command+Shift+B`, `F5`, `Control+F5`, or **Tasks: Run Task**.

### Multiple source files belong to one program

The active-file task compiles only the currently open `.cpp` file. For a program composed of several source files, use a dedicated build task or CMake.

## Platform note

This repository is configured for macOS. Windows users should clone the separate Windows repository, which includes its own portable compiler and freeglut libraries.
