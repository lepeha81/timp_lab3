# timp_lab3
## Laboratory work III

Данная лабораторная работа посвещена изучению систем автоматизации сборки проекта на примере **CMake**



## Homework

Представьте, что вы стажер в компании "Formatter Inc.".
### Задание 1
Вам поручили перейти на систему автоматизированной сборки **CMake**.
Исходные файлы находятся в директории [formatter_lib](formatter_lib).
В этой директории находятся файлы для статической библиотеки *formatter*.
Создайте `CMakeList.txt` в директории [formatter_lib](formatter_lib),
с помощью которого можно будет собирать статическую библиотеку *formatter*.

1)
![изображение](https://github.com/lepeha81/timp_lab3/blob/main/1.PNG)

2)
![изображение](https://github.com/lepeha81/timp_lab3/blob/main/8.PNG)

cat >> ФАЙЛ <<_EOF_ - Эта конструкция похожа на предыдущую, в ней используются\

>> (перенаправление вывода в файл с дописыванием данных)\

<< (here document, то есть многострочный ввод)\

В результате выполнения следующего кода:

cat >> ФАЙЛ <<_EOF_
foo
bar
bar bar
foo foo
_EOF_

В ФАЙЛ будут сохранены строки:

foo
bar
bar bar
foo foo

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
![изображение](https://github.com/lepeha81/timp_lab3/blob/main/2.PNG)
2)
![изображение](https://github.com/lepeha81/timp_lab3/blob/main/3.PNG)

`target_include_directories()` подключаем а таргету(проекту) заданную директорию с установленной видимостью
`add_subdirectory` подключаем в сборку проект formatter из заданной директории
`target_link_libraries()` линкуем все проекты с установленной видимостью


### Задание 3
Конечно же ваша компания предоставляет примеры использования своих библиотек.
Чтобы продемонстрировать как работать с библиотекой *formatter_ex*,
вам необходимо создать два `CMakeList.txt` для двух простых приложений:
* *hello_world*, которое использует библиотеку *formatter_ex*;

![изображение](https://github.com/lepeha81/timp_lab3/blob/main/4.PNG)
![image](https://github.com/lepeha81/timp_lab3/blob/main/9.PNG)
![image](https://github.com/lepeha81/timp_lab3/blob/main/5.PNG)
![image](https://github.com/lepeha81/timp_lab3/blob/main/6.PNG)
![image](https://github.com/lepeha81/timp_lab3/blob/main/7.PNG)
* *solver*, приложение которое испольует статические библиотеки *formatter_ex* и *solver_lib*.
`add_executable()` добавляем в проект исполняемый файл(-ы) 
