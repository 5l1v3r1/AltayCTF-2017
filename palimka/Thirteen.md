# Название
#### Thirteen
# Стоимость
#### 352
# Категория
#### Stego, Misk
# Решение
Задание состоит из 13 частей.

Part 1

Un.bmp
Меняем высоту в Hex редакторе, видим, сверху написан флаг.

![image](https://github.com/SharLike-CTF-Team/AltayCTF-2017/blob/master/palimka/th_1.png)

Получаем строку case i of

Part 2

两.jpg
В свойствах написана строка if (flag[i]='{') and (i=1) then begin flag:='Nygnl'+flag; flag[7]:='g' end;

Part 3

Three.png
На картинке написана строка, которая приводит к неправильному результату.
Открываем в hex-редакторе и видим что там картинка не одна. Последняя картинка и есть искомая
Получаем строку : if (i=12) then begin flag[i-1]:=chr(122); flag[i+2]:='l'; flag[i+1]:=chr(97); end;

Part 4

τέσσερα.gif
Открываем в любом редакторе, на одном кадре написано, что нужно.
Получаем строку: while (i<=length(flag)) do begin

Part 5

חמש.txt
Убираем все лишние символы, оставляем только 0 и 1, получаем двоичный код, преобразуем в текст, получаем
Получаем строку: 8, 9: flag[i]:=chr(48);

Part 6

Sei.avi
Открываем видео, смотрим его, слегка заметными субтитрами в течении всего видео появляются куски строки
Строка: lol:writeln(flag+'sv1r5}');end.

Part 7

Sieben.png
Открываем в блокноте или hex-редакторе, смотрим в конец файла.
Строка: label lol, looooop;var flag: string; i: integer;

Part 8

Восемь.jpg
Всматриваемся в картинку, замечаем слегка заметными буквами надпись
Строка: Program thirteen;

Part 9

QR на окне

![image](https://github.com/SharLike-CTF-Team/AltayCTF-2017/blob/master/palimka/th_2.png)

Строка: 10, 15: flag[i]:=chr(95);

Part 10

10.pdf
Копируем текст написанный белыми буквами в любой текстовый редактор
Строка: if ((flag[i-1]='_') and (flag[i-2]='l')) then goto lol end;

Part 11

once.gif
Аналогично  τέσσερα.gif
Строка: i:=i+1;

Part 12

اثنا عشر.tif
Собираем QR из кусочков, лишние отличаются по оттенку

![image](https://github.com/SharLike-CTF-Team/AltayCTF-2017/blob/master/palimka/th_3.png)
Читаем, получаем строку
Строка: 12: flag[i]:=chr(64); end;

Part 13

ცამეტი.mp3
Открываем свойства, видим строку.
Получаем строку: begin flag:='{XXXXXXXXX'; i:=1;
### Ответ:
Из этого всего получаем код (надо расположить в верном порядке), выполняем программу и получаем флаг в Base64.

![image](https://github.com/SharLike-CTF-Team/AltayCTF-2017/blob/master/palimka/th_4.png)

### Флаг:
>Altay{t00_m@ny_fi1e5}
