## Laboratory work III

Данная лабораторная работа посвещена изучению систем автоматизации сборки проекта на примере **CMake**


## Теория

**CMake файл** — набор инструкций по сборке проекта. Должен называться `CMakeLists.txt`.

**Таргет** — что-то, что можно собрать, то есть библиотека или исполняемый файл.

**Собрать** — значит из исходного кода получить файл библиотеки формата `.a` или исполняемый файл.

Ещё у CMake есть свои переменные, как и у консоли, но нам пока нужна только одна:

**CMAKE_CURRENT_SOURCE_DIR** — путь к директории, где лежит `CMakeLists.txt`.

**Процесс сборки** делится на два этапа:

- Подготовка файлов сборки
- Непосредственно сборка

**Команды сборки:**

**cmake -B \<build_dir>** — готовит файлы сборки и собирает их в папочку `<build_dir>`.

**cmake --build \<build_dir>** — на основе файлов из папки `<build_dir>` собирает библиотеку или исполняемый файл.

Директорию для сборочных файлов обычно называем `build`:

```sh
$ cmake -B build
$ cmake --build build
```

**Как писать эти  CMake файлы?**

Очень просто. Рассмотрим структуру CMake файла для сборки некоторого исполняемого файла `app`, использующего некую библиотеку `library`:

```cmake
cmake_minimum_required(VERSION 3.4)
```

Эта строка задаёт минимальную версию утилиты **cmake**, необходимую для сборки.

```cmake
project(app)
```

Задаём название проекта.

```cmake
set(CMAKE_CXX_STANDARD 11)
set(CMAKE_CXX_STANDARD_REQUIRED ON)
```

Задаём требуемый стандарт C++.

```cmake
add_subdirectory(${CMAKE_CURRENT_SOURCE_DIR}/../library library_dir)
```

Включаем в проект библиотеку, лежащую в директории `../library`. Предполагается, что в `../library` есть CMake файл для сборки этой библиотеки.

Здесь:

**${CMAKE_CURRENT_SOURCE_DIR}/../library** — путь к директории с CMake файлом библиотеки.

**library_dir** — под каким именем директория файлов сборки библиотеки будет записана в файлах сборки нашего исполняемого файла.

```cmake
add_executable(app ${CMAKE_CURRENT_SOURCE_DIR}/app.cpp)
```

Создаём таргет `app` исполняемого файла, исходный код которого лежит в файле `app.cpp`.

```cmake
target_include_directories(app PUBLIC
${CMAKE_CURRENT_SOURCE_DIR}/../library
)
```

Открываем таргету доступ к директории `../library`. Там лежит  `library.h` — заголовочный файл библиотеки.

```cmake
target_link_libraries(app library)
```

Подключаем библиотеку `library` к таргету `app`.

___

## Homework

Представьте, что вы стажер в компании "Formatter Inc.".
### Задание 1
Вам поручили перейти на систему автоматизированной сборки **CMake**.
Исходные файлы находятся в директории [formatter_lib](formatter_lib).
В этой директории находятся файлы для статической библиотеки *formatter*.
Создайте `CMakeList.txt` в директории [formatter_lib](formatter_lib),
с помощью которого можно будет собирать статическую библиотеку *formatter*.

1)
![изображение](https://github.com/lepeha81/timp_lab3/blob/main/10.PNG)

2)
![изображение](https://github.com/lepeha81/timp_lab3/blob/main/11.PNG)
3)
![изображение](https://github.com/lepeha81/timp_lab3/blob/main/12.PNG)
> Проверим, что проект собирается. Для этого **в директории** `formatter_lib` введём команды:
> ```sh
> $ cmake -B build
> $ cmake --build build
> ```
> Последней строкой консоль должна выдать `[100%] Built target formatter_lib`



`cmake_minimum_require` задает минимальную версию Cmake

Затем задаем стандарт C++

`project(formatter)` создаем проект formatter, к которому можно подключать исполняемы файлы и библиотеки

`add_library(formatter STATIC formatter.cpp)` создаем статическую библиотеку из данных файлов.

### Задание 2
У компании "Formatter Inc." есть перспективная библиотека,
которая является расширением предыдущей библиотеки. Т.к. вы уже овладели
навыком созданием `CMakeList.txt` для статической библиотеки *formatter*, ваш 
руководитель поручает заняться созданием `CMakeList.txt` для библиотеки 
*formatter_ex*, которая в свою очередь использует библиотеку *formatter*.

1)
![изображение](https://github.com/lepeha81/timp_lab3/blob/main/13.PNG)
2)
![изображение](https://github.com/lepeha81/timp_lab3/blob/main/14.PNG)

Прописывается основная информация для сборки через CMake.
cmake_minimum_required(VERSION 3.4)    # Минимальная версия CMake

set(CMAKE_CXX_STANDARD 17)             # Стандарт C++

set(CMAKE_CXX_STANDARD_REQUIRED ON)    # Обязательность установки указанного стандарта (если OFF (по умолчанию), то воспринимается как совет)

Название проекта
project()

Библиотека
add_library(

  my_very_interesting_target_library              # Название цели
  
  STATIC                                          # Тип библиотеки (SHARED или STATIC)
  
  ${CMAKE_CURRENT_SOURCE_DIR}/path_to_cpp_file
  
  ${CMAKE_CURRENT_SOURCE_DIR}/path_to_another_cpp_file
)

Указание директорий с заголовочными файлами
target_include_directories(

  target_name                           # Цель, при сборке которой учитываются указанные пути к заголовочным файлам
  
  PUBLIC                                # область видимости (PRIVATE, INTERFACE или PUBLIC)
  
  ${CMAKE_CURRENT_SOURCE_DIR}/include
)
Указание библиотек для линковки

target_link_libraries(

  target_name                         # Цель, к которой просходит линковка
  
  library_name                        # Библиотеки, которые линкуются к цели
  
)

`add_subdirectory` подключаем в сборку проект formatter из заданной директории


### Задание 3
Конечно же ваша компания предоставляет примеры использования своих библиотек.
Чтобы продемонстрировать как работать с библиотекой *formatter_ex*,
вам необходимо создать два `CMakeList.txt` для двух простых приложений:
* *hello_world*, которое использует библиотеку *formatter_ex*;

![изображение](https://github.com/lepeha81/timp_lab3/blob/main/21.PNG)
 В папке `build` появится исполняемый файл `hello_world`. Запустим его:
![image](https://github.com/lepeha81/timp_lab3/blob/main/15.PNG)
> Есть лишь одна проблема. В коде библиотеки есть **ошибка**: не хватает библиотеки `cmath`, а `sqrtf` не лежит в `std`. Исправим это. В файле
![image](https://github.com/lepeha81/timp_lab3/blob/main/16.PNG)
![image](https://github.com/lepeha81/timp_lab3/blob/main/17.PNG)
> Далее соберём библиотеку.
> 
> Директория `solver_lib`, файл `CMakeLists.txt`:
![image](https://github.com/lepeha81/timp_lab3/blob/main/18.PNG)
> Директория `solver_application`, файл `CMakeLists.txt`:
> ![image](https://github.com/lepeha81/timp_lab3/blob/main/19.PNG)
 > Разница лишь в том, что к таргету `solver` подключаются две библиотеки.
>  ![image](https://github.com/lepeha81/timp_lab3/blob/main/20.PNG)
* *solver*, приложение которое испольует статические библиотеки *formatter_ex* и *solver_lib*.
* 
add_executable(main ${SOURCE_EXE})	# Создает исполняемый файл с именем main
