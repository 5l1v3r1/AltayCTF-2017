# ReturnTo0
### Задание
Дана осциллограмма сигнала, который необходимо раскодировать и получить флаг.

![Фрагмент сигнала](https://bitbucket.org/superBroDeLock/tasksaltayctf17/downloads/ing1.jpg)

###  Решение
Сначала нужно выяснить, какой использован принцип кодирования. Название задания подсказывает, что это кодирование называется Return To Zero.

![Статья на сайте wikipedia.org](https://bitbucket.org/superBroDeLock/tasksaltayctf17/downloads/img2.jpg)

Логический ноль кодируется отрицательным выступом, а логическая единица – положительным выступом. Объем закодированной в сигнале информации 23 байта, поэтому длина изображения сигнала позволяет произвести декодирование вручную.

![Демонстрация декодирования](https://bitbucket.org/superBroDeLock/tasksaltayctf17/downloads/img3.jpg)

Совокупность цифровых кодов формирует ASCII-код флага.

![Полученная последовательность](https://bitbucket.org/superBroDeLock/tasksaltayctf17/downloads/img4.jpg)

Флаг:
```
Altay{take_some_signal}
```
