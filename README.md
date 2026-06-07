# Лабораторная работа №3

Данная лабораторная работа посвящена изучению систем автоматизации сборки проекта на примере **CMake**.

---

## Туториал

### Подготовка и копирование репозитория

Настроим переменные, необходимые для работы, переместимся в рабочую директорию и выполним активацию предварительно созданных скриптов.

```bash
export GITHUB_USERNAME=Nicckki
cd ${GITHUB_USERNAME}/workspace
pushd .
source scripts/activate
```
Теперь скопируем репозиторий из прошлой лабораторной работы и привяжем его к новому репозиторию.

```bash
git clone https://github.com/${GITHUB_USERNAME}/lab02.git projects/lab03
cd projects/lab03
git remote remove origin
git remote add origin https://github.com/${GITHUB_USERNAME}/lab03.git
git branch -M main
```

## Ручная компиляция (без CMake)

Скомпилируем print.cpp, указывая с помощью -I папку с заголовочными файлами.

```bash
g++ -std=c++11 -I./include -c sources/print.cpp
```

Появится объектный файл print.o. Посмотрим его содержимое с помощью hexdump.

```bash
hexdump print.o
```

<details> 
  <summary>Вывод hexdump print.o</summary>
      
    0000000 457f 464c 0102 0001 0000 0000 0000 0000
    0000010 0001 003e 0001 0000 0000 0000 0000 0000
    0000020 0000 0000 0000 0000 0710 0000 0000 0000
    0000030 0000 0000 0040 0000 0000 0040 0010 000f
    0000040 0ff3 fa1e 4855 e589 8348 10ec 8948 f87d
    0000050 8948 f075 8b48 f855 8b48 f045 8948 48d6
    0000060 c789 00e8 0000 9000 c3c9 0ff3 fa1e 4855
    0000070 e589 8348 10ec 8948 f87d 8948 f075 8b48
    0000080 f045 8b48 f855 8948 48d6 c789 00e8 0000
    0000090 9000 c3c9 0ff3 fa1e 4855 e589 8348 10ec
    00000a0 7d89 89fc f875 7d83 01fc 3b75 7d81 fff8
    00000b0 00ff 7500 4832 058d 0000 0000 8948 e8c7
    00000c0 0000 0000 8d48 0005 0000 4800 c289 8d48
    00000d0 0005 0000 4800 c689 8b48 0005 0000 4800
    00000e0 c789 00e8 0000 9000 c3c9 0ff3 fa1e 4855
    00000f0 e589 ffbe 00ff bf00 0001 0000 93e8 ffff
    0000100 5dff 00c3 0000 0000 0000 0000 0000 0000
    0000110 4700 4343 203a 5528 7562 746e 2075 3131
    0000120 342e 302e 312d 6275 6e75 7574 7e31 3232
    0000130 302e 2e34 2933 3120 2e31 2e34 0030 0000
    0000140 0004 0000 0010 0000 0005 0000 4e47 0055
    0000150 0002 c000 0004 0000 0003 0000 0000 0000
    0000160 0014 0000 0000 0000 7a01 0052 7801 0110
    0000170 0c1b 0807 0190 0000 001c 0000 001c 0000
    0000180 0000 0000 002a 0000 4500 100e 0286 0d43
    0000190 6106 070c 0008 0000 001c 0000 003c 0000
    00001a0 0000 0000 002a 0000 4500 100e 0286 0d43
    00001b0 6106 070c 0008 0000 001c 0000 005c 0000
    00001c0 0000 0000 0056 0000 4500 100e 0286 0d43
    00001d0 0206 0c4d 0807 0000 001c 0000 007c 0000
    00001e0 0000 0000 0019 0000 4500 100e 0286 0d43
    00001f0 5006 070c 0008 0000 0000 0000 0000 0000
    0000200 0000 0000 0000 0000 0000 0000 0000 0000
    0000210 0001 0000 0004 fff1 0000 0000 0000 0000
    0000220 0000 0000 0000 0000 0000 0000 0003 0001
    0000230 0000 0000 0000 0000 0000 0000 0000 0000
    0000240 0000 0000 0003 0004 0000 0000 0000 0000
    0000250 0000 0000 0000 0000 000b 0000 0001 0005
    0000260 0000 0000 0000 0000 0001 0000 0000 0000
    0000270 0026 0000 0001 0004 0000 0000 0000 0000
    0000280 0001 0000 0000 0000 0035 0000 0002 0001
    0000290 0054 0000 0000 0000 0056 0000 0000 0000
    00002a0 0065 0000 0002 0001 00aa 0000 0000 0000
    00002b0 0019 0000 0000 0000 0074 0000 0012 0001
    00002c0 0000 0000 0000 0000 002a 0000 0000 0000
    00002d0 00b6 0000 0010 0000 0000 0000 0000 0000
    00002e0 0000 0000 0000 0000 011a 0000 0012 0001
    00002f0 002a 0000 0000 0000 002a 0000 0000 0000
    0000300 0172 0000 0010 0000 0000 0000 0000 0000
    0000310 0000 0000 0000 0000 018a 0000 0210 0000
    0000320 0000 0000 0000 0000 0000 0000 0000 0000
    0000330 0197 0000 0010 0000 0000 0000 0000 0000
    0000340 0000 0000 0000 0000 01ad 0000 0010 0000
    0000350 0000 0000 0000 0000 0000 0000 0000 0000
    0000360 01c5 0000 0010 0000 0000 0000 0000 0000
    0000370 0000 0000 0000 0000 7000 6972 746e 632e
    0000380 7070 5f00 535a 4c74 3931 6970 6365 7765
    0000390 7369 5f65 6f63 736e 7274 6375 0074 5a5f
    00003a0 7453 384c 5f5f 6f69 6e69 7469 5f00 345a
    00003b0 5f31 735f 6174 6974 5f63 6e69 7469 6169
    00003c0 696c 617a 6974 6e6f 615f 646e 645f 7365
    00003d0 7274 6375 6974 6e6f 305f 6969 5f00 4c47
    00003e0 424f 4c41 5f5f 7573 5f62 5f49 5a5f 7035
    00003f0 6972 746e 4b52 534e 3774 5f5f 7863 3178
    0000400 3131 6232 7361 6369 735f 7274 6e69 4967
    0000410 5363 3174 6331 6168 5f72 7274 6961 7374
    0000420 6349 5345 4961 4563 4545 5352 006f 5a5f
    0000430 7453 736c 6349 7453 3131 6863 7261 745f
    0000440 6172 7469 4973 4563 6153 6349 4545 5352
    0000450 3174 6233 7361 6369 6f5f 7473 6572 6d61
    0000460 5449 545f 5f30 5345 5f37 4b52 534e 3774
    0000470 5f5f 7863 3178 3131 6232 7361 6369 735f
    0000480 7274 6e69 4967 3453 535f 5f35 3154 455f
    0000490 0045 5a5f 7035 6972 746e 4b52 534e 3774
    00004a0 5f5f 7863 3178 3131 6232 7361 6369 735f
    00004b0 7274 6e69 4967 5363 3174 6331 6168 5f72
    00004c0 7274 6961 7374 6349 5345 4961 4563 4545
    00004d0 5352 3174 6234 7361 6369 6f5f 7366 7274
    00004e0 6165 496d 5363 5f32 0045 5a5f 534e 3874
    00004f0 6f69 5f73 6162 6573 4934 696e 4374 4531
    0000500 0076 5f5f 7364 5f6f 6168 646e 656c 5f00
    0000510 4c47 424f 4c41 4f5f 4646 4553 5f54 4154
    0000520 4c42 5f45 5f00 4e5a 7453 6938 736f 625f
    0000530 7361 3465 6e49 7469 3144 7645 5f00 635f
    0000540 6178 615f 6574 6978 0074 0000 0000 0000
    0000550 0023 0000 0000 0000 0004 0000 0009 0000
    0000560 fffc ffff ffff ffff 004d 0000 0000 0000
    0000570 0004 0000 0009 0000 fffc ffff ffff ffff
    0000580 0078 0000 0000 0000 0002 0000 0003 0000
    0000590 fffc ffff ffff ffff 0080 0000 0000 0000
    00005a0 0004 0000 000b 0000 fffc ffff ffff ffff
    00005b0 0087 0000 0000 0000 0002 0000 000c 0000
    00005c0 fffc ffff ffff ffff 0091 0000 0000 0000
    00005d0 0002 0000 0003 0000 fffc ffff ffff ffff
    00005e0 009b 0000 0000 0000 002a 0000 000e 0000
    00005f0 fffc ffff ffff ffff 00a3 0000 0000 0000
    0000600 0004 0000 000f 0000 fffc ffff ffff ffff
    0000610 0000 0000 0000 0000 0001 0000 0002 0000
    0000620 00aa 0000 0000 0000 0020 0000 0000 0000
    0000630 0002 0000 0002 0000 0000 0000 0000 0000
    0000640 0040 0000 0000 0000 0002 0000 0002 0000
    0000650 002a 0000 0000 0000 0060 0000 0000 0000
    0000660 0002 0000 0002 0000 0054 0000 0000 0000
    0000670 0080 0000 0000 0000 0002 0000 0002 0000
    0000680 00aa 0000 0000 0000 2e00 7973 746d 6261
    0000690 2e00 7473 7472 6261 2e00 6873 7473 7472
    00006a0 6261 2e00 6572 616c 742e 7865 0074 642e
    00006b0 7461 0061 622e 7373 2e00 6f72 6164 6174
    00006c0 2e00 6572 616c 692e 696e 5f74 7261 6172
    00006d0 0079 632e 6d6f 656d 746e 2e00 6f6e 6574
    00006e0 472e 554e 732d 6174 6b63 2e00 6f6e 6574
    00006f0 672e 756e 702e 6f72 6570 7472 0079 722e
    0000700 6c65 2e61 6865 665f 6172 656d 0000 0000
    0000710 0000 0000 0000 0000 0000 0000 0000 0000
    *
    0000750 0020 0000 0001 0000 0006 0000 0000 0000
    0000760 0000 0000 0000 0000 0040 0000 0000 0000
    0000770 00c3 0000 0000 0000 0000 0000 0000 0000
    0000780 0001 0000 0000 0000 0000 0000 0000 0000
    0000790 001b 0000 0004 0000 0040 0000 0000 0000
    00007a0 0000 0000 0000 0000 0550 0000 0000 0000
    00007b0 00c0 0000 0000 0000 000d 0000 0001 0000
    00007c0 0008 0000 0000 0000 0018 0000 0000 0000
    00007d0 0026 0000 0001 0000 0003 0000 0000 0000
    00007e0 0000 0000 0000 0000 0103 0000 0000 0000
    00007f0 0000 0000 0000 0000 0000 0000 0000 0000
    0000800 0001 0000 0000 0000 0000 0000 0000 0000
    0000810 002c 0000 0008 0000 0003 0000 0000 0000
    0000820 0000 0000 0000 0000 0103 0000 0000 0000
    0000830 0001 0000 0000 0000 0000 0000 0000 0000
    *
    0000850 0031 0000 0001 0000 0002 0000 0000 0000
    0000860 0000 0000 0000 0000 0103 0000 0000 0000
    0000870 0001 0000 0000 0000 0000 0000 0000 0000
    *
    0000890 003e 0000 000e 0000 0003 0000 0000 0000
    00008a0 0000 0000 0000 0000 0108 0000 0000 0000
    00008b0 0008 0000 0000 0000 0000 0000 0000 0000
    00008c0 0008 0000 0000 0000 0008 0000 0000 0000
    00008d0 0039 0000 0004 0000 0040 0000 0000 0000
    00008e0 0000 0000 0000 0000 0610 0000 0000 0000
    00008f0 0018 0000 0000 0000 000d 0000 0006 0000
    0000900 0008 0000 0000 0000 0018 0000 0000 0000
    0000910 004a 0000 0001 0000 0030 0000 0000 0000
    0000920 0000 0000 0000 0000 0110 0000 0000 0000
    0000930 002e 0000 0000 0000 0000 0000 0000 0000
    0000940 0001 0000 0000 0000 0001 0000 0000 0000
    0000950 0053 0000 0001 0000 0000 0000 0000 0000
    0000960 0000 0000 0000 0000 013e 0000 0000 0000
    0000970 0000 0000 0000 0000 0000 0000 0000 0000
    0000980 0001 0000 0000 0000 0000 0000 0000 0000
    0000990 0063 0000 0007 0000 0002 0000 0000 0000
    00009a0 0000 0000 0000 0000 0140 0000 0000 0000
    00009b0 0020 0000 0000 0000 0000 0000 0000 0000
    00009c0 0008 0000 0000 0000 0000 0000 0000 0000
    00009d0 007b 0000 0001 0000 0002 0000 0000 0000
    00009e0 0000 0000 0000 0000 0160 0000 0000 0000
    00009f0 0098 0000 0000 0000 0000 0000 0000 0000
    0000a00 0008 0000 0000 0000 0000 0000 0000 0000
    0000a10 0076 0000 0004 0000 0040 0000 0000 0000
    0000a20 0000 0000 0000 0000 0628 0000 0000 0000
    0000a30 0060 0000 0000 0000 000d 0000 000b 0000
    0000a40 0008 0000 0000 0000 0018 0000 0000 0000
    0000a50 0001 0000 0002 0000 0000 0000 0000 0000
    0000a60 0000 0000 0000 0000 01f8 0000 0000 0000
    0000a70 0180 0000 0000 0000 000e 0000 0008 0000
    0000a80 0008 0000 0000 0000 0018 0000 0000 0000
    0000a90 0009 0000 0003 0000 0000 0000 0000 0000
    0000aa0 0000 0000 0000 0000 0378 0000 0000 0000
    0000ab0 01d2 0000 0000 0000 0000 0000 0000 0000
    0000ac0 0001 0000 0000 0000 0000 0000 0000 0000
    0000ad0 0011 0000 0003 0000 0000 0000 0000 0000
    0000ae0 0000 0000 0000 0000 0688 0000 0000 0000
    0000af0 0085 0000 0000 0000 0000 0000 0000 0000
    0000b00 0001 0000 0000 0000 0000 0000 0000 0000
    0000b10
</details>

Теперь убедимся, что в объектном файле присутствуют символы функции print.

```bash
nm print.o | grep print
```

<details> 
  <summary>Вывод nm print.o | grep print</summary>

    00000000000000aa t _GLOBAL__sub_I__Z5printRKNSt7__cxx1112basic_stringIcSt11char_traitsIcESaIcEEERSo
    0000000000000000 T _Z5printRKNSt7__cxx1112basic_stringIcSt11char_traitsIcESaIcEEERSo
    000000000000002a T _Z5printRKNSt7__cxx1112basic_stringIcSt11char_traitsIcESaIcEEERSt14basic_ofstreamIcS2_E
</details>

Создадим статическую библиотеку из объектного файла.

```bash
ar rvs print.a print.o
```

<details> 
  <summary>Вывод ar rvs print.a print.o</summary>

    ar: creating print.a
    a - print.o
</details>

Проверим тип файла.

```bash
file print.a
```

<details> 
  <summary>Вывод file print.a</summary>

    print.a: current ar archive
</details>

Скомпилируем example1.cpp и соберём исполняемый файл.

```bash
g++ -std=c++11 -I./include -c examples/example1.cpp
g++ example1.o print.a -o example1
./example1 && echo
```

<details> 
  <summary>Вывод ./example1 && echo</summary>

    hello
</details>

Аналогично для example2.cpp.

```bash
g++ -std=c++11 -I./include -c examples/example2.cpp
g++ example2.o print.a -o example2
./example2
cat log.txt && echo
```

<details> 
  <summary>Вывод ./example2 и cat log.txt</summary>

    hello
</details>

Удалим все временные файлы перед переходом к CMake.

```bash
rm -rf example1.o example2.o print.o print.a example1 example2 log.txt
```

## Сборка через CMake (базовая версия)
Создадим файл конфигурации CMakeLists.txt для создания библиотеки print.

```bash
cat > CMakeLists.txt <<EOF
cmake_minimum_required(VERSION 3.4)
project(print)
EOF

cat >> CMakeLists.txt <<EOF
set(CMAKE_CXX_STANDARD 11)
set(CMAKE_CXX_STANDARD_REQUIRED ON)
EOF

cat >> CMakeLists.txt <<EOF
add_library(print STATIC \${CMAKE_CURRENT_SOURCE_DIR}/sources/print.cpp)
EOF

cat >> CMakeLists.txt <<EOF
include_directories(\${CMAKE_CURRENT_SOURCE_DIR}/include)
EOF
```

Теперь соберём библиотеку с помощью CMake.

```bash
cmake -H. -B_build
```

<details>
  <summary>Вывод cmake -H. -B_build</summary>

    -- The C compiler identification is GNU 11.4.0
    -- The CXX compiler identification is GNU 11.4.0
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
    -- Configuring done
    -- Generating done
    -- Build files have been written to: /home/vboxuser/Nicckki/workspace/projects/lab03/_build
</details>

```bash
cmake --build _build
```

<details> 
  <summary>Вывод cmake --build _build</summary>

    [ 50%] Building CXX object CMakeFiles/print.dir/sources/print.cpp.o
    [100%] Linking CXX static library libprint.a
    [100%] Built target print
</details>

Теперь добавим в CMakeLists.txt строки для создания исполняемых файлов.

```bash
cat >> CMakeLists.txt <<EOF
add_executable(example1 \${CMAKE_CURRENT_SOURCE_DIR}/examples/example1.cpp)
add_executable(example2 \${CMAKE_CURRENT_SOURCE_DIR}/examples/example2.cpp)
target_link_libraries(example1 print)
target_link_libraries(example2 print)
EOF
```

Пересоберём проект.

```bash
cmake --build _build
```

<details> 
  <summary>Вывод cmake --build _build после добавления примеров</summary>

    -- Configuring done
    -- Generating done
    -- Build files have been written to: /home/vboxuser/Nicckki/workspace/projects/lab03/_build
    Consolidate compiler generated dependencies of target print
    [ 33%] Built target print
    [ 50%] Building CXX object CMakeFiles/example1.dir/examples/example1.cpp.o
    [ 66%] Linking CXX executable example1
    [ 66%] Built target example1
    [ 83%] Building CXX object CMakeFiles/example2.dir/examples/example2.cpp.o
    [100%] Linking CXX executable example2
    [100%] Built target example2
</details>

Можно собрать цели по отдельности.

```bash
cmake --build _build --target print
cmake --build _build --target example1
cmake --build _build --target example2
```

Проверим работу исполняемых файлов.

```bash
_build/example1 && echo
```

<details> 
  <summary>Вывод _build/example1 && echo</summary>

    hello
</details>

```bash
_build/example2
cat log.txt && echo
```

<details> 
  <summary>Вывод _build/example2 и cat log.txt</summary>

    hello
</details>

```bash
rm -f log.txt
```

## Продвинутый CMakeLists.txt (с установкой)

Заменим наш CMakeLists.txt на файл из репозитория с методичкой.

```bash
git clone https://github.com/tp-labs/lab03 tmp
mv -f tmp/CMakeLists.txt .
rm -rf tmp
```

Убедимся, что CMakeLists.txt действительно изменился.

```bash
cat CMakeLists.txt
```

<details> 
  <summary>Содержимое продвинутого CMakeLists.txt</summary>
    cmake
    cmake_minimum_required(VERSION 3.4)
    
    set(CMAKE_CXX_STANDARD 11)
    set(CMAKE_CXX_STANDARD_REQUIRED ON)
    
    option(BUILD_EXAMPLES "Build examples" OFF)
    
    project(print)
    
    add_library(print STATIC ${CMAKE_CURRENT_SOURCE_DIR}/sources/print.cpp)
    
    target_include_directories(print PUBLIC
      $<BUILD_INTERFACE:${CMAKE_CURRENT_SOURCE_DIR}/include>
      $<INSTALL_INTERFACE:include>
    )
    
    if(BUILD_EXAMPLES)
      file(GLOB EXAMPLE_SOURCES "${CMAKE_CURRENT_SOURCE_DIR}/examples/*.cpp")
      foreach(EXAMPLE_SOURCE ${EXAMPLE_SOURCES})
        get_filename_component(EXAMPLE_NAME ${EXAMPLE_SOURCE} NAME_WE)
        add_executable(${EXAMPLE_NAME} ${EXAMPLE_SOURCE})
        target_link_libraries(${EXAMPLE_NAME} print)
        install(TARGETS ${EXAMPLE_NAME}
          RUNTIME DESTINATION bin
        )
      endforeach(EXAMPLE_SOURCE ${EXAMPLE_SOURCES})
    endif()
    
    install(TARGETS print
        EXPORT print-config
        ARCHIVE DESTINATION lib
        LIBRARY DESTINATION lib
    )
    
    install(DIRECTORY ${CMAKE_CURRENT_SOURCE_DIR}/include/ DESTINATION include)
    install(EXPORT print-config DESTINATION cmake)
</details>

Теперь выполним установку библиотеки в папку _install.

```bash
cmake -H. -B_build -DCMAKE_INSTALL_PREFIX=_install
cmake --build _build --target install
```

<details> 
  <summary>Вывод установки</summary>

    -- Configuring done
    -- Generating done
    -- Build files have been written to: /home/vboxuser/Nicckki/workspace/projects/lab03/_build
    Consolidate compiler generated dependencies of target print
    [100%] Built target print
    Install the project...
    -- Install configuration: ""
    -- Installing: /home/vboxuser/Nicckki/workspace/projects/lab03/_install/lib/libprint.a
    -- Installing: /home/vboxuser/Nicckki/workspace/projects/lab03/_install/include
    -- Installing: /home/vboxuser/Nicckki/workspace/projects/lab03/_install/include/print.hpp
    -- Installing: /home/vboxuser/Nicckki/workspace/projects/lab03/_install/cmake/print-config.cmake
    -- Installing: /home/vboxuser/Nicckki/workspace/projects/lab03/_install/cmake/print-config-noconfig.cmake
</details>

Выведем дерево директории _install.

```bash
tree _install
```

<details> 
  <summary>Вывод tree _install</summary>
  
    _install
    ├── cmake
    │   ├── print-config.cmake
    │   └── print-config-noconfig.cmake
    ├── include
    │   └── print.hpp
    └── lib
        └── libprint.a
    
    3 directories, 4 files
</details>

## Фиксация в репозитории
Добавим CMakeLists.txt в репозиторий и отправим изменения на GitHub.

```bash
git add CMakeLists.txt
git commit -m "added CMakeLists.txt"
git push origin main
```

<details> 
  <summary>Вывод git push</summary>

    Enumerating objects: 4, done.
    Counting objects: 100% (4/4), done.
    Compressing objects: 100% (3/3), done.
    Writing objects: 100% (3/3), 750 bytes | 750.00 KiB/s, done.
    Total 3 (delta 1), reused 0 (delta 0), pack-reused 0
    remote: Resolving deltas: 100% (1/1), completed with 1 local object.
    To https://github.com/Nicckki/lab03.git
       25ecd5b..ad880c5  main -> main
</details>

# Домашнее задание

Как было сказано на занятии, я не буду приводить здесь содержимое файлов CMakeLists.txt, лишь объясню как вся конструкция работает.

Рассмотрим дерево папки homework:

```bash
homework
├── CMakeLists.txt
├── formatter_ex_lib
│   ├── CMakeLists.txt
│   ├── formatter_ex.cpp
│   ├── formatter_ex.h
│   └── formatter_lib
│       ├── CMakeLists.txt
│       ├── formatter.cpp
│       └── formatter.h
├── hello_world_application
│   └── hello_world.cpp
├── solver_application
│   └── equation.cpp
└── solver_lib
    ├── CMakeLists.txt
    ├── solver.cpp
    └── solver.h
```
В этой папке находится CMakeLists.txt, который использует директории formatter_ex_lib и solver_lib, беря из них "местные" CMakeLists.txt, которые собирают соответствующие библиотеки, также использует файлы из директорий hello_world_application и solver_application и собирает два исполняемых файла. Это задание 3.

Дальше рассмотрим папку formatter_ex_lib. Она собирает библиотеку formatter_ex, опираясь на библиотеку formatter из папки formatter_lib, но также используя файлы formatter_ex.cpp и formatter_ex.h. Это задание 2.

И наконец папка formatter_lib выполняет задание 1.

Для проверки запустим исполняемые файлы из задания три.

```bash
-------------------------
hello, world!
-------------------------
1 5 -6
-------------------------
x1 = -6.000000
-------------------------
-------------------------
x2 = 1.000000
-------------------------
```
Домашнее задание работает.
