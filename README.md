# SavitarLE

[![Conan Badge]][Conan]
[![Unit Test Badge]][Unit Test]

[![Size Badge]][Size]
[![Scorecard Badge]][Scorecard]
[![License Badge]][License]

This library contains C++ code and Python bindings for loading 3mf files.

## License

Savitar is released under terms of the LGPLv3 License. Terms of the license can be found in the LICENSE file. Or at
http://www.gnu.org/licenses/lgpl.html

> But in general it boils down to:  
> **You need to share the source of any SavitarLE modifications if you make an application with SavitarLE.**

## System Requirements

### Windows

- Python 3.6 or higher
- Ninja 1.10 or higher
- VS2022 or higher
- CMake 3.23 or higher
- nmake

### MacOS

- Python 3.6 or higher
- Ninja 1.10 or higher
- apply clang 11 or higher
- CMake 3.23 or higher
- make

### Linux

- Python 3.6 or higher
- Ninja 1.10 or higher
- gcc 12 or higher
- CMake 3.23 or higher
- make

## How To Build

> **Note:**  
> We are currently in the process of switch our builds and pipelines to an approach which uses [Conan](https://conan.io/)
> and pip to manage our dependencies, which are stored on our JFrog Artifactory server and in the pypi.org.
> At the moment not everything is fully ported yet, so bare with us.

If you have never used [Conan](https://conan.io/) read their [documentation](https://docs.conan.io/en/latest/index.html)
which is quite extensive and well maintained. Conan is a Python program and can be installed using pip

### 1. Configure Conan

```bash
pip install conan --upgrade
conan config install https://github.com/lulzbot3d/Conan_LulzBot_Config.git
conan profile new default --detect --force
```

Community developers would have to remove the Conan cura-le repository because it requires credentials. 

LulzBot developers need to request an account for our JFrog Artifactory server with IT

```bash
conan remote remove cura-le
```

### 2. Clone libSavitarLE

```bash
git clone https://github.com/lulzbot3d/libSavitarLE.git
cd libSavitarLE
```

### 3. Install & Build libSavitarLE (Release OR Debug)

#### Release

```bash
conan install . --build=missing --update
# optional for a specific version: conan install . savitarle/<version>@<user>/<channel> --build=missing --update
cmake --preset release
cmake --build --preset release
```

#### Debug

```bash
conan install . --build=missing --update build_type=Debug
cmake --preset debug
cmake --build --preset debug
```

## Creating a new SavitarLE Conan package

To create a new SavitarLE Conan package such that it can be used in CuraLE and UraniumLE, run the following command:

```shell
conan create . savitarle/<version>@<username>/<channel> --build=missing --update
```

This package will be stored in the local Conan cache (`~/.conan/data` or `C:\Users\username\.conan\data` ) and can be used in downstream
projects, such as CuraLE and UraniumLE by adding it as a requirement in the `conanfile.py` or in `conandata.yml`.

Note: Make sure that the used `<version>` is present in the conandata.yml in the pySavitarLE root

You can also specify the override at the commandline, to use the newly created package, when you execute the `conan install`
command in the root of the consuming project, with:

```shell
conan install . -build=missing --update --require-override=savitarle/<version>@<username>/<channel>
```

## Developing libSavitarLE In Editable Mode

You can use your local development repository downsteam by adding it as an editable mode package.
This means you can test this in a consuming project without creating a new package for this project every time.

```bash
    conan editable add . savitarle/<version>@<username>/<channel>
```

Then in your downsteam projects (CuraLE) root directory override the package with your editable mode package.  

```shell
conan install . -build=missing --update --require-override=savitarle/<version>@<username>/<channel>
```

<!---------------------------------------------------------->

[Conan Badge]: https://img.shields.io/github/actions/workflow/status/lulzbot3d/libSavitarLE/conan-package.yml?style=for-the-badge&logoColor=white&logo=Conan&label=Conan%20Package
[Unit Test Badge]: https://img.shields.io/github/actions/workflow/status/lulzbot3d/libSavitarLE/unit-test.yml?style=for-the-badge&logoColor=white&logo=Codacy&label=Unit%20Test
[Size Badge]: https://img.shields.io/github/repo-size/lulzbot3d/libSavitarLE?style=for-the-badge&logoColor=white&logo=GoogleAnalytics
[License Badge]: https://img.shields.io/github/license/lulzbot3d/libSavitarLE?style=for-the-badge&logoColor=white&logo=GNU
[Scorecard Badge]: https://img.shields.io/ossf-scorecard/github.com/lulzbot3d/libSavitarLE?style=for-the-badge&logo=GitHub&label=OpenSSF%20Scorecard

[Conan]: https://github.com/lulzbot3d/libSavitarLE/actions/workflows/conan-package.yml
[Unit Test]: https://github.com/lulzbot3d/libSavitarLE/actions/workflows/unit-test.yml
[Size]: https://github.com/lulzbot3d/libSavitarLE
[License]: LICENSE
[Scorecard]: https://api.securityscorecards.dev/projects/github.com/lulzbot3d/libSavitarLE
