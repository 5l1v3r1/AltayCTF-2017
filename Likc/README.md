# MoreIf
 На входе мы имеем файл elf, в функции main видим странное сравнение ‘A’==’A‘, которое всегда приводит к вызову функции FirstPart
![](https://bitbucket.org/Likc/writeupsaltay2017/downloads/image1.png)
Данная функция возвращает символ ‘l’
![](https://bitbucket.org/Likc/writeupsaltay2017/downloads/image2.png)
В самой функции идет сравнение с 3 символами
![](https://bitbucket.org/Likc/writeupsaltay2017/downloads/image3.png)
В итоге получаем строку 

> Altay

что является началом флага, затем переходим в функцию SecondPart
![](https://bitbucket.org/Likc/writeupsaltay2017/downloads/image4.png)
Видим перемещение в eax символов 
> {too

по относительным адресам адресам rbp+var_20+7, rbp+var_10, rbp+var_10+3 и  rbp+var_10+3 соответственно. После этого идет вызов функции ThirdPart и запись в eax символа 
> }

![](https://bitbucket.org/Likc/writeupsaltay2017/downloads/image5.png)
Здесь все ещё проще, находим соответствующий символ, по порядку выходит 
> _much_conditions

Складываем все полученные флаги и получаем строку
### Flag:
> Altay{too_much_conditions}

# BlackSquare
Запускаем приложение, видим gui и черный квадрат
![](https://bitbucket.org/Likc/writeupsaltay2017/downloads/image6.png)
Если популярная легенда, что под черным квадратом нарисовано что-то ещё, на эту легенду есть намек в описании задания.
По составу файлов в паке с программой ясно, что данная программа написана на Qt, и для вывода изображения там обычно используется QGraphicsScene, смотрим на инициализацию QGraphicsScene  и видим, что их инициализируется сразу две, причем с разницей в 4 байта.
![](https://bitbucket.org/Likc/writeupsaltay2017/downloads/image7.png)
 Одна необходима для вывода черного квадрата, а на второй с помощью QPainter рисуются различные элементы, в примере ниже - это линии.
![](https://bitbucket.org/Likc/writeupsaltay2017/downloads/image8.png)
Перейдем к выводу QGraphicsScene  на экран
![](https://bitbucket.org/Likc/writeupsaltay2017/downloads/image9.png)
Заменим QGraphicsScene, заменив байт 18h(24) на 1Ch(28)
![](https://bitbucket.org/Likc/writeupsaltay2017/downloads/image10.png)

### Flag:
>Altay{he_was_crazy}

# Atavism
На входе имеем elf файл, который принимает аргумент при запуске
![](https://bitbucket.org/Likc/writeupsaltay2017/downloads/image11.png)
Далее берется первый элемент и параметром отправляется в функцию to128
![](https://bitbucket.org/Likc/writeupsaltay2017/downloads/image12.png)
Затем, в цикле проверяется равенства каждого символа после обработки в to128 и константных символов в начале (их в дальнейшем и копируем в декодер).
![](https://bitbucket.org/Likc/writeupsaltay2017/downloads/image13.png)
Проверим, как именно обрабатывается строка в функции to 128
Проанализировав алгоритм понимаем, что это преобразование в base128 (оно аналогично с прочими base).  Далее приведен алгоритм декодинга.
```python
symb =[8,22,99,81,66,121,61,92,110,71,19,5,92,103,50,87,108,38,11,77,74,125]
bitstr = ''

for i in symb:
    bitstr+=str(bin(i)[2:]).zfill(7)
begin=len(bitstr)-8
end=len(bitstr)
res=''
while begin>0:
    res=chr(int(bitstr[begin:end],2))+res
    end=begin
    begin-=8
print(res)
```

### Flag:
> Altay{strange_base}

# GoodSwim
В описании по сути есть все необходимое для решения. Ключ, зашифрованный текст и подсказка, что использовался именно алгоритм симметричного шифрования. Далее есть 2 возможных пути решения, первый – перебирать все симметричные шифры, второй - это скопировать зашифрованный текст (первые 9 символов) в поисковик. А единственными ссылками, будут ссылки на fernet. Нам остается только написать код для расшифровки.
```python
from cryptography.fernet import Fernet

text = 'gAAAAABZQjBnepFsUY-0RNit8Zzb1ZdO58D3Thpg_AMaLY_DLiVZ26cvZmOAZiYdqSrkihNIJ5TWQarwygxwzQt2tAimosCi5AocbSjvS_EZFlmAhNP7GBdn83HhBekoQ3nc357CiMpC'
key = 'vulINFq8szgpRVjx0xEplLwFbnrdfw38Nr5OciGB60w='

FerKey = Fernet(key)
print(FerKey.decrypt(text.encode()))
```
### Flag:
> Altay{Some_Synchron1city_to_y0ur life}

# Дальтоник v2.0
Задача ясна и понятна, необходимо спарсить содержание веб страницы, найти картинку и проанализировать его, решается все достаточно просто.
```python
import requests
from PIL import Image
import re

s= requests.session()
adr = 'http://46.61.235.29:40580/daltonik2/'
res = s.get(adr)

while True:
    r = 0
    g = 0
    b = 0
    begin = res.text.find('(')+1
    end = res.text.find(')')
    picbyte = s.get(adr+res.text[begin:end])
    with open('pic.bmp','wb') as picfile:
        picfile.write(picbyte.content)
    pic = Image.open('pic.bmp')
    for x in range(pic.width):
        for y in range(pic.height):
            NowPix = pic.getpixel((x,y))
            if(NowPix==(255,0,0)):
                r+=1
            elif(NowPix==(0,255,0)):
                g+=1
            else:
                b+=1
    MaxColor = max(r,g,b)
    if MaxColor==r:
        ResultColor = 'red'
    elif MaxColor==g:
        ResultColor = 'green'
    else:
        ResultColor='blue'
    res = s.post(adr,data={'color':ResultColor})
    if 'Altay' in res.text:
        print(re.findall(r'(Altay.*)}', res.text)[0])
        break
```
### Flag:
> Altay{ti_daje_bolee_krut_chem_ya_dumal}

