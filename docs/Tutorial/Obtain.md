# Obtain

## Precompiled Binaries

The program is published in GitHub. [https://github.com/TLCFEM/suanPan/releases](https://github.com/TLCFEM/suanPan/releases)

The `dev` branch is built for every valid commit. Binaries on Windows, Ubuntu and MacOS are compiled and deployed automatically on the `master` branch.

In order to enable `VTK`, `CUDA` and `MKL`, the program shall be compiled locally with preinstalled external libraries.

## Execute Program

By default, the `AVX` support is turned on to utilize CPU capability. For CPUs that do not support `AVX`, the application **cannot** be successfully executed. Users can either compile the program by themselves or request a specific version by filing an issue. Processors that do not support `AVX` may be too old to perform HPC based simulations.

The parallelization is enabled mostly by the `TBB` library. If the program is compiled with **SUANPAN_MT** macro, parallelization is used by default. The **OpenMP** is enabled in several parts of the program, users can set environment variable **OMP_NUM_THREADS** to enable some **OpenMP** based parallelization. To do so, users can, for example in Windows, use the following command.

```powershell
set OMP_NUM_THREADS=6
```

On Linux, the dynamic loading path need to be set so that dynamic libraries such as `libtbb.so` can be successfully found. If the software is installed via snap, it is automatically done. Alternatively, users can execute the program via the provided `suanPan.sh` script.

```bash
# current path contains suanPan
export LD_LIBRARY_PATH:$LD_LIBRARY_PATH:$(pwd)
./suanPan
```

### CLI Mode

By running the program without any parameters, it enters CLI mode by default. Users can create models in an interactive manner.

```
+--------------------------------------------------+
|   __        __         suanPan is an open source |
|  /  \      |  \           FEM framework (64-bit) |
|  \__       |__/  __   __           Acrux (1.0.0) |
|     \ |  | |    |  \ |  |      maintained by tlc |
|  \__/ |__| |    |__X |  |    all rights reserved |
|                           10.5281/zenodo.1285221 |
+--------------------------------------------------+
|  https://github.com/TLCFEM/suanPan               |
|  https://github.com/TLCFEM/suanPan-manual        |
+--------------------------------------------------+
|  https://gitter.im/suanPan-dev/community         |
+--------------------------------------------------+

suanPan ~<>
```

### Batch Mode

To analyze the model written in a model file named as for example `example.supan`, the `-f` or `--file` parameter can be used. First we create the file `example.supan` with `exit` command.

```bash
echo exit > example.supan
```

Then we can run it by the following command.

```bash
./suanPan -f example.supan
```

Or on Windows,

```powershell
./suanPan.exe -f example.supan
```

If the model has been prechecked, it is possible to run the analysis without output. It is known that printing strings to terminals slows down the analysis. Users can use the `-np` or `--noprint` option to suppress output.

```
./suanPan.exe -np -f example.supan
```

In the CLI mode, it is possible to use `file` command to load the file.

```
+--------------------------------------------------+
|   __        __         suanPan is an open source |
|  /  \      |  \           FEM framework (64-bit) |
|  \__       |__/  __   __           Acrux (1.0.0) |
|     \ |  | |    |  \ |  |      maintained by tlc |
|  \__/ |__| |    |__X |  |    all rights reserved |
|                           10.5281/zenodo.1285221 |
+--------------------------------------------------+
|  https://github.com/TLCFEM/suanPan               |
|  https://github.com/TLCFEM/suanPan-manual        |
+--------------------------------------------------+
|  https://gitter.im/suanPan-dev/community         |
+--------------------------------------------------+

suanPan ~<> file example.supan
```

## Sublime Text Workspace

I personally use [Sublime Text](https://www.sublimetext.com/) as my model editor. Other tools like [Atom](https://atom.io/) and [VS Code](https://code.visualstudio.com/) can also be used.

### Syntax Highlighting

Create a new syntax file via `Tools -> Developer -> New Syntax...`, copy and paste the following sample content into the new file and save as `suanPan.sublime-syntax` under the default path. It provides syntax highlighting for comments only. Other components can be added accordingly.

```yaml
%YAML 1.2
---
file_extensions:
  - supan
  - sp
scope: source.supan
contexts:
  main:
    - match: '^[#!].*'
      scope: comment.line
    - match: '!.*'
      scope: comment.line
```

A syntax file with (almost) all commands is provided as `suanPan.sublime-syntax` in the archive. Please feel free to use/modify it.

### Autocomplete

All keywords used are stored in the JSON file `suanPan.sublime-completions`. Place the file in folder `~/.config/sublime-text-3/Packages/User/` (Linux) or `%appdata%\Sublime Text 3\Packages\User` (Windows) and you are good to go with the previous syntax file.

### Build System

In order to render ANSI color codes correctly in Linux like systems, you may wish to install [ANSIescape](https://packagecontrol.io/packages/ANSIescape) package. Now define a new build system via `Tools -> Build System -> New Build System...`, copy and paste the following contents in the file and save it as `suanPan.sublime-build`. You may need to replace command `suanpan` with the full path of the executable.

```json
{
   "cmd":[
      "suanpan",
      "-f",
      "$file"
   ],
   "selector":"source.supan",
   "file_patterns":[
      "*.supan",
      "*.sp"
   ],
   "target":"ansi_color_build",
   "syntax":"Packages/ANSIescape/ANSI.sublime-syntax"
}
```

Now models can be run in Sublime Text via the shortcut `Ctrl+B`.

## Automation

### Windows

A batch file named as `AddAssociation.bat` is provided in the archive. It associates `*.sp` and `*.supan` files with the program and copies configuration files to default folder if Sublime Text is installed.

Admin privilege is required.

### Linux

A bash script named as `suanPan.sh` is provided in the archive to setup the above configurations automatically. The script can be used to both execute the program and create symbolic links.

For the first time use, the following commands create a soft link under `$HOME/.local/bin` so that users can execute the program anywhere by invoking command `suanpan` in any folder. The Sublime Text configuration files are copied to the default folder if Sublime Text is installed.

```bash
# current path contains suanPan
chmod +x suanPan.sh
./suanPan.sh --create-link
# cd to other places such as folders that contain models
# now invoke the program
suanpan
```
