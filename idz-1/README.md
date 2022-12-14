# ИДЗ-1. Работа с массивами.

## Вариант 18.

# Задание

Сформировать массив B из элементов массива A уменьшением всех элементов до первого положительного на 5.

# Оглавление

### Введение

- [О программе](#О-программе)
  - [Входные данные](#Входные-данные)
  - [Аргументы запуска](#Аргументы-запуска)
  - [Функции и локальные переменные в ASM](#Функции-и-локальные-переменные-в-ASM)
- [Представленные файлы](#Представленные-файлы)
- [О тестировании](#О-тестировании)

### Отчет

- [Mark 4](#Mark-4)
- [Mark 5](#Mark-5)
- [Mark 6](#Mark-6)
- [Mark 7](#Mark-7)
- [Mark 8](#Mark-8)
- [Mark 9](#Mark-9)

### Прочее

- [Checklist](#Checklist)

# Введение

## О программе

Программа на C изначально разрабатывалась с учетом всех требований включительно **до оценки 9**.
Поэтому, с самого начала, присутствует разделение на модули, работа с файлами, генерация случайных данных и подсчет времени работы программы.

После преобразования массива, в консоль выводится затраченное время в миллисекундах (без учета ввода и вывода).

## Входные данные

Ограничение на количество элементов: `1 <= n <= 300_000 (MAX_SIZE)`.
Реализована проверка на соответствие указываемого размера данному диапазону.

Тип элементов массива - `int32`. При вводе значений, программа подразумевает, что на вход подаются корректные числовые значение `int32`.

## Аргументы запуска

| Аргумент                               | Описание                                                                                                                                                                                                                                                                                                                                            | Пример                                                          |
| -------------------------------------- | --------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | --------------------------------------------------------------- |
| Без аргументов                         | Запуск программы в консольном режиме                                                                                                                                                                                                                                                                                                                | `./a.out`                                                       |
| `-file <входной файл> [выходной файл]` | Передача и получение данных из программы с использованием файлов. Выходной файл может быть опущен, в таком случае результат выполнения будет выведен в консоль                                                                                                                                                                                      | `./a.out -file input.txt`, `./a.out -file input.txt output.txt` |
| `-random <размер массива> <seed>`      | Запуск программы в режиме генерацией случайных данных. Сгенерированный массив и результат выполнения выводятся в консоль                                                                                                                                                                                                                            | `./a.out -random 10 123`                                        |
| `-test <seed>`                         | Стресс-тестирование программы. Генерируется массив случайных данных размером `MAX_SIZE`, после чего происходит его обработка. Данный процесс повторяется несколько тысяч раз (данные не меняются так как поражаются заданным `<seed>`. это позволяет получить ""среднее"" время обработки набора и сравнить производительность при разных запусках) | `./a.out -test`                                                 |

## Представленные файлы

```
| /c-source - исходный код программы на языке C
| /tests
|   | /temp - папка с временными файлами генерируемыми тестовой утилитой. которые подаются на вход программе
|   | /manual-tests - наборы данных для ручного тестирования консольного и файлового ввода
|   | main.py - утилита тестирования
| /version-0 - ASM c исполняемым файлом полученный из c-source
| /version-1 - ASM с комментариями и исполняемым файлом (на базе version-0)
| /version-2 - ASM с преимущественным использованием регистров и исполняемым файлом (на базе version-1)
| /with-optimization-flags
|   | /asms - ASM коды полученные с использованием различных опций оптимизации
|   | /bins - исполняемые файлы полученные с использованием различных опций оптимизации
```

## О тестировании

Для тестирования был написан специальный Python скрипт, который генерирует многочисленные наборы случайных данных вида `(input, correct_output)` на которых тестируется каждая программа. В рамках одного тестового прогона, **наборы подаваемые на вход каждой программы одинаковы**.

Тестирование файлового и консольного режима также происходит на одинаковых данных.

Для стресс-теста, каждая программа запускается скриптом несколько раз в режиме `-test`, при этом время выполнения суммируется.

Ознакомиться подробнее с алгоритмом можно в `main.py`

# Mark 4

- [Исходный код программы на языке C (c-source)](c-source/)
- [ASM код исходника на языке С (version-0)](version-0/)
- [АSM код с комментариями (version-1)](version-1/)

Для получения ASM кода использовался следующий набор опций

```
gcc -masm=intel \
    -fno-asynchronous-unwind-tables \
    -fno-jump-tables \
    -fno-stack-protector \
    -fno-exceptions \
    ./c-source/*.c \
    -S
```

Макросов в полученных ASM файлах не оказалось, но мною была удалена бОльшая часть служебных меток GCC.

Для получения исполняемого файла из ASM

```
gcc *.s
```

### Результаты тестовых прогонов

```
python3 tests/main.py c-source/a.out version-1/a.out
```

```
Running 300 tests of random on each executable...
✓ c-source/a.out:       Random Tests Passed
✓ version-1/a.out:      Random Tests Passed

Running 300 tests on each executable...
✓ c-source/a.out:       Console Tests passed
✓ version-1/a.out: Console Tests passed
✓ c-source/a.out:       File Tests passed
✓ version-1/a.out: File Tests passed
```

# Mark 5

- [АSM код с комментариями (version-1)](version-1/)
  Дополнительных изменений не потребовалось - все требуемые комментарии были написаны при выполнении пункта Mark 4, а функции и локальные переменные присутствовали с самого начала.

# Mark 6

- [АSM код с преимущественным использованием регистров (version-2)](version-2/)

Файл main.s остался не тронутым. В main не происходит никаких вычислений или работы с массивом, в следствии чего, ощутимой разницы в скорости выполнения при хранении данных в регистрах не будет. Пришлось так поступить, поскольку свободных preserved регистров не оставалось, а лезть в scratch регистры с учетом выше сказанного не имеет большого смысла.

Так же, в некоторых я переписал обращение к `array[i]` - вместо 5 строчек, в которых происходило вычисление смещения, получение адреса и только затем обращением, у меня получилось получилось `mov rdx, DWORD r11[rax*4]`

| Функция                 | Использованы регистры                                  | Дополнительно                                |
| ----------------------- | ------------------------------------------------------ | -------------------------------------------- |
| process_array           | `i => r8`, `&array => rdi`, `size => esi`              | На стеке больше ничего не хранится           |
| read_array_from_console | `&array => r15`, `max_size => r14`, `i => r13`         | Выделяется меньше памяти на стеке            |
| print_array             | `&array => r15`, `size => r14`, `i => r13`             | На стеке больше ничего не хранится           |
| read_array_from_file    | `&filepath => rsi`, `&array => r15`, `max_size => r14` | Выделяется в два раза меньше памяти на стеке |
| save_array_to_file      | `&filepath => rdx`, `&array => r15`, `size => r14`     | Выделяется в три раза меньше памяти на стеке |
| random_fill_array       | `&array => r15`, `size => r14`, `seed => edx`          |                                              |

В итоге, программа использует следующие регистры:
|Регистр|Используется|
|---|---|
|**Preserved**||
|r13|`consoleio: i`|
|r13|`random: arra_size`, `fileio: array_size (print), max_size (read)`, `consoleio: size (print)`|
|r15|`random: &array`, `fileio: &array`, `consoleio: &array`|
|**Scratch**| |
|rsi|`fileio: filepath`|
|r8|`arrayprocessor: i`|
|r9|`random: seed`|

### Результаты тестовых прогонов

**В тестировании принимали участие:**

- `/c-source/a.out` - "нетронутый" исходник
- `/version-1/a.out` - версия с комментариями
- `/version-2/a.out` - версия с преимущественным использованием регистров

```
python3 tests/main.py c-source/a.out version-1/a.out version-2/a.out
```

```
Running tests on executables: c-source/a.out, version-1/a.out, version-2/a.out
Running 300 tests on each executable...
✓ c-source/a.out:       Console Tests passed
✓ version-1/a.out:      Console Tests passed
✓ version-2/a.out:      Console Tests passed
✓ c-source/a.out:       File Tests passed
✓ version-1/a.out:      File Tests passed
✓ version-2/a.out:      File Tests passed

Running stress tests on each executable...
c-source/a.out:     6.630322 seconds
version-1/a.out:    6.650191 seconds
version-2/a.out:    1.121077 seconds
```

Использование регистров в `process_array` позволило увеличить скорость обработки массива в 6 раз!

# Оценка 7

- [АSM код с преимущественным использованием регистров (version-2)](version-2/)
  Корректность работы файлового режима была проверена с использованием тестовой-утилиты и ручного тестирования.

```
Running 300 tests on each executable...
✓ version-2/a.out:      Console Tests passed
✓ version-2/a.out:      File Tests passed
```

# Mark 8

- [ASM код исходника на языке С (version-0)](version-0/)
- [АSM код с комментариями (version-1)](version-1/)
- [АSM код с преимущественным использованием регистров (version-2)](version-2/)

### Результаты тестирования

Тестирование производилось на **1 ядре AMD 3200G**.
|Версия|Размер ASM кода (байт)|Размер исполняемого файла (байт)|Результат стресс-теста (сек)|
|---|---|---|---|
|c-source|12260|17712|6.642092|
|version-1|26692|17456|6.641976|
|version-2 (регистры)|26914|17320|1.120018|

Использование регистров в `process_array` позволило увеличить скорость обработки массива в 6 раз!

# Mark 9

- [ASM код исходника на языке С (version-0)](version-0/)
- [АSM код с комментариями (version-1)](version-1/)
- [АSM код с преимущественным использованием регистров (version-2)](version-2/)
- [Различные опции оптимизации](with-optimization-flags/)

Для получения ASM кода использовались следующие опции

```
gcc *.c -masm=intel -fno-asynchronous-unwind-tables -fno-jump-tables -fno-stack-protector -fno-exceptions -Os -S
```

Для получения исполняемых файлов

```
gcc *.c -masm=intel -fno-asynchronous-unwind-tables -fno-jump-tables -fno-stack-protector -fno-exceptions -Os
```

## Сравнение полученных программ

| Версия               | Размер ASM кода (байт) | Размер исполняемого файла (байт) | Результат стресс-теста (сек) |
| -------------------- | ---------------------- | -------------------------------- | ---------------------------- |
| c-source             | 12260                  | 17712                            | 6.642092                     |
| version-1            | 26692                  | 17456                            | 6.641976                     |
| version-2 (регистры) | 26914                  | 17320                            | 1.120018                     |
| O0                   | 12260                  | 17664                            | 6.661868                     |
| O1                   | 9431                   | 17696                            | 1.170315                     |
| O2                   | 9927                   | 17696                            | 1.054121                     |
| O3                   | 9927                   | 17696                            | 1.055933                     |
| Ofast                | 9927                   | 19280                            | 1.059444                     |
| Os                   | 8727                   | 17696                            | 1.176046                     |

\*Размер ASM кода - сумма полученных ASM файлов.

Заметим, что `-O2` и `-O3` полностью идентичны. `-Ofast` не уступает `-O2` и `-О3`, однако имеет самый большой размер исполняемого файла. Худшие результаты времени у `c-source`, `version-1`, `version-1` (время исполнение одинаковое, что и следовало ожидать). Размер ASM `version-1` и `version-2` больше всех остальных из-за наличия комментариев в коде.

Если сравнивать `version-2` и лучшую версию полученную компилятором `-O2`, у `version-2` размер исполняемого файла меньше на 3%, время выполнения больше на 6%

# Функции и локальные переменные в ASM

### main()

#### Локальные переменные

```
- DWORD -4[rbp]     - array_size
- DWORD -8[rbp]     - rand_seed
- DWORD -12[rbp]    - input_mode
- DWORD -16[rbp]    - clocks_elapsed
- DWORD -20[rbp]    - i
- QWORD -32[rbp]    - file_path
- QWORD -40[rbp]    - start_time
- QWORD -48[rbp]    - end_time
- DWORD -52[rbp]    - количество аргументов запуска
- QWORD -64[rbp]    - указатель на аргументы
```

#### Параметры и возвращаемый результат

```
- edi - argc
- rsi - argc
- rax (return) - exit code
```

### random_fill_array()

#### Локальные переменные

```
- DWORD -4[rbp]     - i
- QWORD -24[rbp]	- &array
- DWORD -28[rbp]    - size
- DWORD -32[rbp]	- seed
```

#### Параметры и возвращаемый результат

```
- rdi - &array
- esi - size
- edx - seed
- rax (return) - array_size
```

### process_array()

#### Локальные переменные

```
- DWORD -4[rbp]     - i
- QWORD -24[rbp]    - &array
- DWORD -28[rbp]    - size
```

#### Параметры и возвращаемый результат

```
- rdi - &array
- esi - size
```

### read_array_from_console()

#### Локальные переменные

```
- DWORD -4[rbp]     - i
- DWORD -8[rbp]     - n
- QWORD -24[rbp]    - &array
- DWORD -28[rbp]    - max_size
```

#### Параметры и возвращаемый результат

```
- rdi - &array
- esi - max_size
- rax (return) - array_size
```

### print_array()

#### Локальные переменные

```
- DWORD -4[rbp]     - i
- QWORD -24[rbp]    - &array
- DWORD -28[rbp]    - size
```

#### Параметры и возвращаемый результат

```
- rdi - &array
- esi - size
```

### read_array_from_file()

#### Локальные переменные

```
- DWORD -4[rbp]     - i
- QWORD -16[rbp]    - &in_file
- DWORD -20[rbp]    - n
- QWORD -40[rbp]    - &array
- QWORD -48[rbp]    - &filepath
- DWORD -52[rbp]    - max_size
```

#### Параметры и возвращаемый результат

```
- rdi - &array
- rsi - &filepath
- edx - max_size
- rax (return) - array_size
```

### save_array_to_file() !!!!!!!!

#### Локальные переменные

```
- QWORD -16[rbp]    - &out_file
- QWORD -24[rbp]    - &array
- DWORD -28[rbp]    - size
- QWORD -40[rbp]    - &filepath
```

#### Параметры и возвращаемый результат

```
- rdi - &array
- rsi - size
- edx - &filepath
```

# Checklist

### Оценка 4

- [x] Приведено решение задачи на C.
- [x] В полученную ассемблерную программу, откомпилированную без оптимизирующих и отладочных опций, добавлены комментарии, поясняющие
      эквивалентное представление переменных в программе на C.
- [x] Из ассемблерной программы убраны лишние макросы за счет использования соответствующих аргументов командной строки и/или за счет
      ручного редактирования исходного текста ассемблерной программы.
- [x] Модифицированная ассемблерная программа отдельно откомпилирована
      и скомпонована без использования опций отладки.
- [x] Представлено полное тестовое покрытие, дающее одинаковый результат
      на обоих программах. Приведены результаты тестовых прогонов для обоих программ, демонстрирующие эквивалентность функционирования.
- [x] Сформировать отчет, описывающий результаты тестовых прогонов и используемых опций компиляции и/или описания проведенных модификаций.

### Оценка 5

- [x] В реализованной программе использовать функции с передачей данных
      через параметры.
- [x] Использовать локальные переменные.
- [x] В ассемблерную программу при вызове функции добавить комментарии,
      описывающие передачу фактических параметров и перенос возвращаемого результата.
- [x] В функциях для формальных параметров добавить комментарии, описывающие связь между параметрами языка Си и регистрами (стеком).
- [x] Добавить информацию о проведенных изменениях в отчет.

### Оценка 6

- [x] Рефакторинг программы на ассемблере за счет максимального использования регистров процессора. Добавление этой программы к уже представленным.
- [x] Добавление комментариев в разработанную программу, поясняющих эквивалентное использование регистров вместо переменных исходной программы на C.
- [x] Представление результатов тестовых прогонов для разработанной программы. Оценка корректности ее выполнения на основе сравнения тестовых прогонов результатами тестирования предшествующих программ.
- [x] Добавить новую информацию в отчет

### Оценка 7

- [x] Реализация программы на ассемблере, полученной после рефакторинга,
      в виде двух или более единиц компиляции.
- [x] Задание файлов с исходными данными и файла для вывода результатов
      с использованием аргументов командной строки.
- [x] Добавить в отчет информацию о проделанных изменениях и результаты
      работы с тестовыми файлами.

### Оценка 8

- [x] Добавление в программу генератора случайных наборов данных, расширяющих возможности тестирования. Подключение генератора к программе с выбором в командной строке варианта ввода данных.
- [x] Расширение анализа командной строки для выбора способа порождения
      исходных данных. Добавление данных, порождаемых генератором.
- [x] Модификация программы на C и программы на ассемблере, полученной
      после рефакторинга, для проведения сравнения на производительность.
      Необходимо добавить замеры во времени, которые не учитывают время
      ввода и вывода данных. Для увеличения времени работы минимум до 1
      секунды, в зависимости от особенностей программы, можно либо выбирать соответствующие размеры исходных данных, либо зацикливать для
      многократного выполнения ту часть программы, которая выполняет вычисления.
- [x] Представить полученные данные в отчете для разных вариантов тестовых прогонов

### Оценка 9

- [x] Используя опции оптимизации по скорости, сформировать из модифицированной программы на C исходный код ассемблере. Провести сравнительный анализ с предыдущими ассемблерными программами по размеру ассемблерного кода, размеру исполняемого файла и производительности.
- [x] Аналогично, используя опции оптимизации по размеру, сформировать
      код на ассемблере. Провести сравнительный анализ с предыдущими ассемблерными программами по размеру ассемблерного кода, размеру исполняемого файла и производительности.
- [x] Представить в отчете полученные результаты, дополнив данные представленные в предыдущем отчете.

### Оценка 10

- [ ] Избавиться от инфляции