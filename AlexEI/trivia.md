# Название
#### Trivia
# Стоимость
#### 41
# Категория
#### Trivia.
# Решение
Командам предложен сайт, который состоит из 12 блоков разного цвета. Цвета, как и сама страница, не несут в себе никакой информации. Всё находится в исходниках.

``` 
<div id="container">
	<div id="header"></div>
	<div id="sidebar"></div>	 
	<div id="tent"></div>
	<div id="side"></div>
	<div id="nader"></div>
</div>
<div id="container">
	<div id="eader"></div>
	<div id="ar"></div>
	<div id="lent"></div>
	<div id="game"></div>
	<div id="tader"></div>
</div>
<div id="container">
	<div id="ader"></div>
	<div id="ydebar"></div>
	<div id="ant"></div>
	<div id="ide"></div>
	<div id="lader"></div>
</div>
```

Здесь можно увидеть, что все блоки имеют разные названия и, так как ни на начальной странице, ни в исходниках флага нет, то остаётся проверить только стили.

```    
<link rel="stylesheet" type="text/css" href="style.css">
```

Собрав первые буквы блоков (начиная с 4), мы получим флаг.

```
	#ader {...}
	#lent {...}
	#tent {...}
	#ar {...}
	#ydebar {...}
	#sidebar {...}
	#tent {...}
	#ydebar {...}
	#lader {...}
	#eader {...}
	#side {...}
	#ant {...}
	#game {...}
	#ader {...}
	#ide {...}
	#nader {...}
```


### Флаг:
>Altay{styles_again}