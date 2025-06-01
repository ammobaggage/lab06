# Домашнее задание 3

## Задание 1

>Прописываю CMakeLists для библиотеки formatter
```
cd lab03/formatter_lib
cat > CMakeLists.txt << EOF

>cmake_minimum_required(VERSION 3.2)
project(formatter)
set(CMAKE_CXX_STANDARD 20)
set(CMAKE_CXX_STANDARD_REQUIRED ON)
add_library(formatter STATIC ${CMAKE_CURRENT_SOURCE_DIR}/formatter.cpp)
include_directories(${CMAKE_CURRENT_SOURCE_DIR})

> EOF
```
---

## Задание 2

>Прописываю CMakeLists для библиотеки formatter ex
```
cd ..
cd formatter_ex_lib
cat > CMakeLists.txt << EOF

>cmake_minimum_required(VERSION 3.2)
project(formatter_ex)
set(CMAKE_CXX_STANDARD 20)
set(CMAKE_CXX_STANDARD_REQUIRED ON)
add_library(formatter_ex STATIC formatter_ex.cpp)
target_include_directories(formatter_ex PUBLIC
    ${CMAKE_CURRENT_SOURCE_DIR}
    ${CMAKE_CURRENT_SOURCE_DIR}/../formatter_lib
)
cmake_policy(SET CMP0079 NEW)  
target_link_libraries(formatter_ex PRIVATE formatter)

>EOF
```
---

## Задание 3

>Прописываю CMakeLists для приложения hello_world с использованием библиотеки formatter ex и дополнительно formatter
```
cd ..
cd hello_world_application
cat > CMakeLists.txt << EOF

>cmake_minimum_required(VERSION 3.2)
project(hello_world)
set(CMAKE_CXX_STANDARD 20)
set(CMAKE_CXX_STANDARD_REQUIRED ON)

add_executable(hello_world hello_world.cpp)
target_link_libraries(hello_world formatter_ex)
target_include_directories(hello_world PRIVATE
    ${CMAKE_CURRENT_SOURCE_DIR}/../formatter_ex_lib
    ${CMAKE_CURRENT_SOURCE_DIR}/../formatter_lib
)

>EOF
```

>Теперь прописываю CMakeLists для библиоткеи solver
```
cd ..
cd solver_lib
cat > CMakeLists.txt << EOF

>cmake_minimum_required(VERSION 3.2)
project(solver_lib)

set(CMAKE_CXX_STANDARD 20)
set(CMAKE_CXX_STANDARD_REQUIRED ON)

add_library(solver_lib STATIC solver.cpp)

target_include_directories(solver_lib PUBLIC
    ${CMAKE_CURRENT_SOURCE_DIR}
)

> EOF
```

>Прописываю CMakeLists для приложения solver
```
cd ..
cd solver_application
cat > CMakeLists.txt << EOF

>cmake_minimum_required(VERSION 3.2)
project(solver)

set(CMAKE_CXX_STANDARD 20)
set(CMAKE_CXX_STANDARD_REQUIRED ON)

add_executable(solver equation.cpp)

target_link_libraries(solver formatter_ex solver_lib)

target_include_directories(solver PRIVATE
    ${CMAKE_CURRENT_SOURCE_DIR}/../formatter_lib
    ${CMAKE_CURRENT_SOURCE_DIR}/../formatter_ex_lib
    ${CMAKE_CURRENT_SOURCE_DIR}/../solver_lib
)

>EOF
```
---
>Прописываю общий CMakeLists для сборки всего проекта. Собираю все библиотеки и приложения
```
cd ..
cat >> CMakeLists.txt << EOF

>cmake_minimum_required(VERSION 3.2)
project(lab03)
set(CMAKE_CXX_STANDARD 20)
set(CMAKE_CXX_STANDARD_REQUIRED ON)
add_subdirectory(formatter_lib)
add_subdirectory(formatter_ex_lib)
add_subdirectory(solver_lib)
add_subdirectory(hello_world_application)
add_subdirectory(solver_application)

>EOF
```

```
cmake -B build
```
**Вывод**
```
CMake Deprecation Warning at CMakeLists.txt:1 (cmake_minimum_required):
  Compatibility with CMake < 3.5 will be removed from a future version of
  CMake.

  Update the VERSION argument <min> value or use a ...<max> suffix to tell
  CMake that the project does not need compatibility with older versions.


-- The C compiler identification is GNU 13.3.0
-- The CXX compiler identification is GNU 13.3.0
-- Detecting C compiler ABI info
-- Detecting C compiler ABI info - done
-- Check for working C compiler: /usr/bin/cc - skipped
-- Detecting C compile features
-- Detecting C compile features - done
-- Detecting CXX compiler ABI info
-- Detecting CXX compiler ABI info - done
-- Check for working CXX compiler: /usr/bin/c++ - skipped
-- Detecting CXX compile features
-- Detecting CXX compile features - done
CMake Deprecation Warning at formatter_lib/CMakeLists.txt:1 (cmake_minimum_required):
  Compatibility with CMake < 3.5 will be removed from a future version of
  CMake.

  Update the VERSION argument <min> value or use a ...<max> suffix to tell
  CMake that the project does not need compatibility with older versions.


CMake Deprecation Warning at formatter_ex_lib/CMakeLists.txt:1 (cmake_minimum_required):
  Compatibility with CMake < 3.5 will be removed from a future version of
  CMake.

  Update the VERSION argument <min> value or use a ...<max> suffix to tell
  CMake that the project does not need compatibility with older versions.


CMake Deprecation Warning at solver_lib/CMakeLists.txt:1 (cmake_minimum_required):
  Compatibility with CMake < 3.5 will be removed from a future version of
  CMake.

  Update the VERSION argument <min> value or use a ...<max> suffix to tell
  CMake that the project does not need compatibility with older versions.


CMake Deprecation Warning at hello_world_application/CMakeLists.txt:1 (cmake_minimum_required):
  Compatibility with CMake < 3.5 will be removed from a future version of
  CMake.

  Update the VERSION argument <min> value or use a ...<max> suffix to tell
  CMake that the project does not need compatibility with older versions.


CMake Deprecation Warning at solver_application/CMakeLists.txt:1 (cmake_minimum_required):
  Compatibility with CMake < 3.5 will be removed from a future version of
  CMake.

  Update the VERSION argument <min> value or use a ...<max> suffix to tell
  CMake that the project does not need compatibility with older versions.


-- Configuring done (1.0s)
-- Generating done (0.0s)
-- Build files have been written to: /home/opiates/ammobaggage/workspace/projects/lab03/build
```
---
```
cmake --build build
```
**Вывод**
```
[ 10%] Building CXX object formatter_lib/CMakeFiles/formatter.dir/formatter.cpp.o
[ 20%] Linking CXX static library libformatter.a
[ 20%] Built target formatter
[ 30%] Building CXX object formatter_ex_lib/CMakeFiles/formatter_ex.dir/formatter_ex.cpp.o
[ 40%] Linking CXX static library libformatter_ex.a
[ 40%] Built target formatter_ex
[ 50%] Building CXX object solver_lib/CMakeFiles/solver_lib.dir/solver.cpp.o
[ 60%] Linking CXX static library libsolver_lib.a
[ 60%] Built target solver_lib
[ 70%] Building CXX object hello_world_application/CMakeFiles/hello_world.dir/hello_world.cpp.o
[ 80%] Linking CXX executable hello_world
[ 80%] Built target hello_world
[ 90%] Building CXX object solver_application/CMakeFiles/solver.dir/equation.cpp.o
[100%] Linking CXX executable solver
[100%] Built target solver
```
---
## Тест сборки
```
cd build/hello_world_application
./hello_world
```
**Вывод**
```
-------------------------
hello, world!
-------------------------
```

[Ссылка на репрезиторий](https://github.com/ammobaggage/lab03)

**Карев Георгий | ИУ8-21**
