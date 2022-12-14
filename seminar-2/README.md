# Семинар 2. Отладка программ на языке C с применением GDB.

## Домашняя работа

### Отчет с протоколом поиска ошибок

1. Скомпилируем программу в отладочном режиме `gcc -g shell-sort.c -o shell-sort.out`
2. Оценим работоспособность программы.
   Программа запускается, на корректном наборе данных завершается кодом 0.
3. Проверим корректность программы на наборе `2 1 5 -12 3 -9`. Программа выдает корректный ответ: `-12 -9 1 2 3 5 N`.
4. Проверим на произвольном наборе данных '4 3 8 4'. Выдает некорректный ответ: `0 3 4 4 N`.
5. Начинаем процесс дебагинга. `gdb ./shell-sort.out`.
6. Поставим _breakpoint_ на 36 строку, дабы убедиться в корректности поступающих данных в функцию _shell_sort_, `break 36`.
7. Запускаем процесс, `run 3 6 9 -3`.
8. Посмотрим на данные в текущем фрейме, `info locals`. Argc, переменная которая передается в качестве размера массива, равняется 5, в то время как размер веденного массива 4.
9. Устраняем ошибку, `shell_sort(a, argc - 1)`.
10. Запускаеи программу и убеждаемся, что ошибка была исправлена.
11. Проводим стресс-тестирование.
