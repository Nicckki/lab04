# Лабораторная работа №4

Данная лабораторная работа посвещена изучению систем непрерывной интеграции.

---

## Домашнее задание

Так как _Travis_ мы не используем, сразу переходим к ДЗ и делаем все через _Github Actions_ для линукса и _Appveyor_ на винде.

Рассмотрим сначала файл для Github Actions:

```bash
name: linux
on: push
jobs:
  build:
    runs-on: ubuntu-latest
    steps:
      - name: Get repo files
        uses: actions/checkout@v4
      
      - name: Configure build directory
        run: cmake -H. -B build
      
      - name: Build project
        run: cmake --build build
      
      - name: Upload entire build directory
        uses: actions/upload-artifact@v4
        with: 
          name: build-folder
          path: ./build/
  
  test:
    needs: build
    runs-on: ubuntu-latest
    steps:
      - name: Download build folder
        uses: actions/download-artifact@v4
        with:
          name: build-folder
          path: ./build
      
      - name: Find and run hello_world
        run: |
          echo "Searching for hello_world..."
          HELLO=$(find ./build -type f -name "hello_world" -o -name "hello_world.exe" | head -1)
          if [ -n "$HELLO" ]; then
            chmod +x "$HELLO"
            echo "Running: $HELLO"
            "$HELLO"
          else
            echo "ERROR: hello_world not found"
            find ./build -type f -executable | head -10
            exit 1
          fi
      
      - name: Find and run solver
        run: |
          echo "Searching for solver..."
          SOLVER=$(find ./build -type f \( -name "solver_app" -o -name "solver" -o -name "solver_app.exe" -o -name "solver.exe" \) | head -1)
          if [ -n "$SOLVER" ]; then
            chmod +x "$SOLVER"
            echo "Running: $SOLVER with input '1 5 -6'"
            echo "1 5 -6" | "$SOLVER"
          else
            echo "ERROR: solver not found"
            find ./build -type f -executable | head -10
            exit 1
          fi
```

Я разбил workflow на два jobs , один для сборки, другой для первичного тестирования. Чтобы получить доступ к файлам из репозитория, использовал actions/checkout@v4, а чтобы передать собранные приложения между jobs использовал actions/upload-artifact@v4 и actions/download-artifact@v4. Также стоит отметить, что сначала в test приложения не запускались из-за отсутствия разрешения, поэтому пришлось добавить команды chmod +x.

Теперь рассмотрим appveyor.yml:

```bash
version: 1.0.{build}

image:
  - Visual Studio 2022
  - Visual Studio 2019

platform:
  - x64
  - x86

configuration:
  - Debug
  - Release

matrix:
  fast_finish: true

build_script:
  - cmake -B build -DCMAKE_BUILD_TYPE=%CONFIGURATION%
  - cmake --build build --config %CONFIGURATION%

test_script:
  - .\build\%CONFIGURATION%\hello_world.exe
  - echo 1 5 -6 | .\build\%CONFIGURATION%\solver_app.exe
```

Здесь я провожу тесты в компиляторах Visual Studio 2022 и Visual Studio 2019, на платформах x64 и x86, в режимах debug и release. Также стоит отметить, что при компиляции в appveyor возникла ошибка, что якобы sqrt не является членом std. Выяснилось, что на компиляторе MSVS нужно подключать cmath, а не math.h. Поэтому пришлось внести следующее изменение в solver.cpp:

```bash
#ifdef _WIN32
    #include <cmath>
#else
    #include <math.h>
#endif
```

которое подключает одну из библиотек в зависимости от ОС.


