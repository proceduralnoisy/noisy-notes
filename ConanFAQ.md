# Conan FAQ

## Misc

### How can a recipe validate options for other (e.g. upstream) recipes?
Dependencies can be accessed in **certain methods only** via the [self.dependencies attribute](https://docs.conan.io/2/reference/conanfile/methods/generate.html#conan-conanfile-model-dependencies). For example, see the [validate method](https://docs.conan.io/2/reference/conanfile/methods/validate.html)

### How can preprocessor defines be specified? 
There are several options:

#### Via profiles/command-line
The [tools.build:defines configuration](https://docs.conan.io/2/reference/config_files/global_conf.html) can be used (typically with a [configuration data operator](https://docs.conan.io/2/reference/config_files/global_conf.html#configuration-data-types)). For example:

```
[conf]
tools.build:defines+=["GLM_FORCE_CTOR_INIT"]
```

> **Puzzle:** How to set and access these within a recipe?
>
> It's possible to set these specifically for CMakeToolchain (see below). However, it doesn't appear possible to access flags set via profiles via CMakeToolchain.

#### Via CMakeToolchain
Preprocessor defines for the current recipe can be specified in [generate](https://docs.conan.io/2/reference/tools/cmake/cmaketoolchain.html#preprocessor-definitions). For example:

```
def generate(self):
    tc = CMakeToolchain(self)
    tc.preprocessor_definitions["MYDEF"] = "MyValue"
```

Note: It doesn't seem possible to set this attribute via profiles

#### Via package_info (for consumers only)
Preprocessor defines can be specified for consumers via [package_info](https://docs.conan.io/2/reference/conanfile/methods/package_info.html#package-info). Note that this only sets defines for consumers - it does not impact the build of the current package.

```
def package_info(self):
    self.cpp_info.defines = ["ZMQ_STATIC"]
```

> **Puzzle:** Syntax for specifying values? 

### How to set CMake variables?

#### Via profiles/command-line
CMake variables can be set via the [tools.cmake.cmaketoolchain:extra_variables](https://docs.conan.io/2/reference/tools/cmake/cmaketoolchain.html) attribut. For example:

```
[conf]
tools.cmake.cmaketoolchain:extra_variables={"MY_VAR": "MyValue"}
```

### Using Ninja
Relevant Conan documentation: [Ninja example](https://docs.conan.io/2/examples/tools/cmake/cmake_toolchain/use_different_toolchain_generator.html)

Ninja is integrated by setting the CMakeToolchain generator (e.g. in the `[conf]` section of a profile). There are two Ninja generators:
* Single-config (`tools.cmake.cmaketoolchain:generator=Ninja`)
* Multi-config (`tools.cmake.cmaketoolchain:generator=Ninja Multi-Config`)

Ninja can be obtained via Conan using a `tool_requires` statement. Example profile:
```
[conf]
tools.cmake.cmaketoolchain:generator=Ninja Multi-Config

[tool_requires]
cmake/3.31.7
ninja/1.12.1
```

### How to visualize the dependency graph of a package?
> **Reference:** [conan graph info](https://docs.conan.io/2/reference/commands/graph/info.html)

Example:
```
conan graph info . --format=html > graph.html
```

## Toolchain-specivic

### How to use an older MSVC compiler without installing an older Visual Studio version?

First, use the Visual Studio installer to install the desired MSVC build tools:
* Launch Visual Studio Installer and click "Modify"
* Open the "Individual components" tab
* Search for "MSVC"
* Install the desired versions (you probably want the x64/x86 build tools variant)

Modify the Conan profile to specify *both* the compiler version and the Visual Studio version. This example below specifies the VS2019 compiler version (`192`) and Visual Studio 2022 (`17`)
```
[settings]
...
compiler.version=192
...

[conf]
```

> **Tip:** Autodetect can be used to determine both the default compiler and version if we want to make the two explicit in autodetected profiles. See [this issue](https://github.com/conan-io/conan/issues/14487).

### How to use the Intel compiler with Conan?
Relevant Conan documentation: [conan.tools.intel](https://docs.conan.io/2/reference/tools/intel.html#intelcc).

My goal was to enable switching between MSVC and Intel via profiles without modifying recipes, so I didn't try the `IntelCC` generator.

I was able to get everything working with CMake+Ninja for command-line and VSCode install/build with the following profile:

```
[settings]
arch=x86_64
build_type=Release
compiler=intel-cc
compiler.cppstd=14
compiler.mode=icx
compiler.runtime=dynamic
compiler.version=2025.1
os=Windows

[buildenv]
CC=icx
CXX=icx

[conf]
tools.intel:installation_path=C:\Program Files (x86)\Intel\oneAPI
tools.cmake.cmaketoolchain:generator=Ninja Multi-Config

[tool_requires]
ninja/1.12.1
```

Considerations:
* Ninja (and Ninja Multi-Config) is the only supported CMake generator. 
* `conan build` works out of the box. Running CMake directly (e.g. `cmake --preset conan-release`) requires running `conanbuild.bat` (e.g. `.\build\generators\conanbuild.bat`) to setup the environment
* To use VSCode (with CMake plugin), must first run the `conanbuild.bat` script (or for a lighter touch, the `conanintelsetvars.bat` script) and then launch VSCode from the same terminal (e.g. via `code .`)
  * The [VSCode oneAPI Environment Configurator](https://marketplace.visualstudio.com/items?itemName=intel-corporation.oneapi-environment-configurator&ssr=false) didn't work for me (activated environment didn't seem to apply to CMake runs)
  * Tip: To verify that the environment is correctly configured, try running `icx`.
  * It might be possible to bypass this by adding various oneAPI folders to the PATH, but that would limit usage to a specific version on the machine

> **Bug:** Intel oneAPI environment doesn't work with powershell (`conanbuild.ps1`)
>
> A `conanintelsetvars.bat` file is generated, but not used by the `.ps1` script. The oneAPI `setvars` script is only provided as a `.bat` file, but there does appear to be an (ugly) trick to run that via powershell (see [here](https://www.intel.com/content/www/us/en/docs/oneapi/programming-guide/2023-2/use-the-setvars-script-with-windows.html)):
>
> `cmd.exe "/K" '"C:\Program Files (x86)\Intel\oneAPI\setvars.bat" && powershell'`
>
> Note that launches two additional nested shells (ew...)

## Conan Center Index

### How to locally create a recipe?
  * Clone the CCI repository to a local folder. If working with a fork, use the fork's URL and (if necessary) checkout the branch with the recipe:
    ```
      git clone https://github.com/proceduralnoisy/conan-center-index
      cd conan-center-index
      git checkout -t origin/add-geometrictools`
    ```
  * Use Conan to create the package and make it available in the local Conan cache:
    ```
    cd recipes/geometrictoolsengine/all
    conan create . --version=8.0
    ```
