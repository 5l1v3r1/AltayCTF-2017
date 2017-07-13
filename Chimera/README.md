# Color
Открывая задание Color, сначала мы видим картинку, изображенную на рисунке 1.

![1](https://github.com/SharLike-CTF-Team/AltayCTF-2017/blob/master/Chimera/1.png)

Нам нужно понять, что с этим делать. Т.к. на рисунке много разных оттенком, можно догадаться, что номера этих оттенков нам понадобятся. Открываем стандартную программу Paint и с помощью пипетки смотрим номера оттенков каждой звезды (рисунок 2).

![2](https://github.com/SharLike-CTF-Team/AltayCTF-2017/blob/master/Chimera/2.png)

Таким образом, если мы выпишем все оттенки, мы получим следующие числа: 65 108 116 97 121 123 105 95 108 111 118 101 95 99 111 108 111 114 125. Можно догадаться, что это символы Аски (рисунок 3).

![3](https://github.com/SharLike-CTF-Team/AltayCTF-2017/blob/master/Chimera/3.jpg)

Расшифруем и получим флаг: ```Altay{i_love_color}```

# PNK
Мы получаем файл неизвестного содержания с расширением .png (рисунок 4), который не открывается никакими стандартными программами. Отсюда следует, что нужно посмотреть код этого файла. Код файла мы посмотрим программой HxD (рисунок 5).

![4](https://github.com/SharLike-CTF-Team/AltayCTF-2017/blob/master/Chimera/4.png)

![5](https://github.com/SharLike-CTF-Team/AltayCTF-2017/blob/master/Chimera/5.png)

Если это файл png, то можно заметить, что некоторые чанки переписаны (рисунок 6).

![6](https://github.com/SharLike-CTF-Team/AltayCTF-2017/blob/master/Chimera/6.png)

Итак, нужно переписать чанки на нормальные. Переписываем и у нас открывается картинка. На ней мы и видим флаг:```Altay{lol_kek_cheburek}``` (рисунок 7).

![7](https://github.com/SharLike-CTF-Team/AltayCTF-2017/blob/master/Chimera/7.png)

# Malbolge
Открывая файл, можно заметить следующий текст (рисунок 8):

![8](https://github.com/SharLike-CTF-Team/AltayCTF-2017/blob/master/Chimera/8.jpg)

Т.к. мы не знаем что с этим делать, первое, что приходит на ум – это воспользоваться Google. Пишем название таска в Google и он выдает нам следующее (рисунок 9):

![9](https://github.com/SharLike-CTF-Team/AltayCTF-2017/blob/master/Chimera/9.png)

Воспользуемся Malbolge - interpreter online и получим флаг ```Altay{unusual_code}``` (рисунок 10).

![10](https://github.com/SharLike-CTF-Team/AltayCTF-2017/blob/master/Chimera/10.png)

# Mendeleev
Вначале мы получаем текстовый файл (рисунок 11) и картинку (рисунок 12).

![11](https://github.com/SharLike-CTF-Team/AltayCTF-2017/blob/master/Chimera/11.png)

![12](https://github.com/SharLike-CTF-Team/AltayCTF-2017/blob/master/Chimera/12.png)

Можно догадаться, что по таблице Менделеева нужно найти вышеуказанные массы элементов и по их первым буквам записать флаг: ```Altay{o_boze_himiya}```.

# Sln
Нам дан архив с названием «Labirint». Мы понимаем, что это игра на языке C# и запускаем ее любой соответствующей программой (рисунок 12, 13).

![13](https://github.com/SharLike-CTF-Team/AltayCTF-2017/blob/master/Chimera/13.png)

![14](https://github.com/SharLike-CTF-Team/AltayCTF-2017/blob/master/Chimera/14.png)

Если нам даны исходники игры, то 100% вероятность, что не нужно проходить ее. Мы ищем в коде нужные нам подсказки для решения задачи. Итак, в на рисунке 13 видно, что мы нашли Hint. Анализируя данную подсказку, понимаем, что нам необходимо найти на GitHub юзера с именем 7sle1i (рисунок 15).
![15](https://github.com/SharLike-CTF-Team/AltayCTF-2017/blob/master/Chimera/15.png)

Заходим на его страницу и ищем дальнейшие подсказки или флаг ```Altay{oh_yes_you_did_it}``` (Рисунок 16).
![16](https://github.com/SharLike-CTF-Team/AltayCTF-2017/blob/master/Chimera/16.png)
