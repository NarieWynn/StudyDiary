 ## CMake Architecture:
---
> [!ABSTRACT] Definition
> **CMakeLists.txt** is a meta-build system for c/c++ to communicate with other libraries (for example: Qt6, OpenCV).
^cmake-def
## Core functions:
* **Dependency Management:** Automatic finding driver/ libraries in system.
* **Meta-Build:** CMake can not compile code. 
* **Cross Compilation:** CMake help run code on different device (OS). 

> [!WARNING] NOTED:
>Case-insensitive => there is no different between add_executable and ADD_EXECUTABLE
## *CMake standard format*
```cmake
# define cmake version
cmake_minimum_required(VERSION 3.10)

#define project name, version, and progamming language use
project(MyProject VERSION 1.0 LANGUAGES CXX)

#set standard language version for project
set(CMAKE_CXX_STANDARD 17)

#turn on required language version if not satisfy version project can run
set(CMAKE_CXX_STANDARD_REQUIRED ON)
#find package require of library Qt6 on system
find_package(Qt6 COMPONENTS Core Gui Qml Quick REQUIRED)
#combine all file *.cpp and *.h into 1 file name MyProject 
add_executable(MyProject main.cpp)
#
qt_add_qml_module(MyProject #name of the app need to use qml
	URI project #virtual address for other qml file to import and use 
	VERSION x.x
	QML_FILES #list of qml file 
	xxx.qml 
)
#connect libraries to c/c++ code
target_link_libraries(polaris_launcher PRIVATE
        Qt6::Core
        Qt6::Gui
        Qt6::Qml
        Qt6::Quick
)
```
# 🛠️ CMAKELISTS.TXT SYNTAX DICTIONARY (POLARIS LAUNCHER)
---

* **<font color="cyan">1. cmake_minimum_required(VERSION 3.10)</font>**
	*  *#note:* **Define cmake version**
	*  *Explanation:* Specifies the minimum version of CMake required to build and run this project.

* ****<font color="cyan">2. project(MyProject VERSION 1.0 LANGUAGES CXX)</font>**
	*  *#note:* **Define project name, version, and programming language use**
	*  *Explanation:* Declares the project's name as `MyProject`, sets its version to `1.0`, and specifies C++ (`CXX`) as the programming language.

* **<font color="cyan">3. set(CMAKE_CXX_STANDARD 17)</font>**
	*  *#note:* **Set standard language version for project**
	*  *Explanation:* Forces the compiler to build the entire project using the C++17 standard features.

* **<font color="cyan">4. set(CMAKE_CXX_STANDARD_REQUIRED ON)</font>**
	*  *#note:* **Turn on required language version if not satisfy version project can run**
	*  *Explanation:* Enforces the C++ standard requirement. If the compiler does not support C++17, CMake will stop the build process immediately instead of failing later.

* ****<font color="cyan">5. find_package(Qt6 COMPONENTS Core Gui Qml Quick REQUIRED)</font>** ^9abf85
	*  *#note:* **Find package require of library Qt6 on system**
	*  *Explanation:* Instructs CMake to search the system (your Arch Linux setup) to ensure all required Qt6 modules are installed.

* ****<font color="cyan">6. add_executable(MyProject main.cpp)</font>**
	*  *#note:* **Combine all file .cpp and .h into 1 file name MyProject**
	*  *Explanation:* Creates the final executable binary by compiling all raw source files (`.cpp`) into machine code `0101` under the target name `MyProject`.

* **<font color="cyan">7. qt_add_qml_module(MyProject URI project VERSION 1.0 QML_FILES main.qml)</font>**
	*  *#note:* **Embed QML into C++ Binary & Create virtual address for import**
	*  *Explanation:* Embeds and compresses all `.qml` interface files directly into the application binary. It also assigns a virtual address (`URI`) named `project` so other QML files can `import` and reference each other smoothly.

* **<font color="cyan">8. target_link_libraries(polaris_launcher PRIVATE Qt6::Core Qt6::Gui ...)</font>**
	*  *#note:* **Connect libraries to c/c++ code**
	*  *Explanation:* Acts as the ultimate linker! It links the compiled binary with the external library files (Qt6, OpenCV). Without this line, the build will fail immediately with an `Undefined reference` error.

---
# THE RESOURCE BOUNDARY: QML IMPORT VS. C++ VIRTUAL PATH
---

* **1. QML-to-QML Communication**
	*  *#note:* **file qml import other find qml by import URI**
	* *Explanation:* QML operates inside a modern, abstract virtual universe. Once a module is registered in CMake, QML files can easily locate and reuse each other using a simple, clean `import <URI>` statement without worrying about messy relative file paths (like `../../components/`).

* **2. C++-to-QML Communication**
	*  *#note:* **while c/c++ can not import URI, using virtual address instead => qrc:/qt/qml/...**
	*  *Explanation:* C++ is a raw, compiled native language—it does not understand the QML `import` keyword. Therefore, to boot up the initial user interface from `main.cpp`, C++ must point directly to the exact location where CMake compressed and embedded those files into the binary. It does this by using the hardcoded virtual resource URL pattern: `qrc:/qt/qml/<URI>/`.

---
> [!TIP] Note:
> * **QML** uses high-level logical mapping (`import URI`).
> * **C++** acts as the blueprint engine that boots the system by pointing to the exact virtual RAM drive coordinates (`qrc:/qt/qml/...`).