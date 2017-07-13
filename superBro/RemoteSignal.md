# RemoteSignal
### Задание
Дана осциллограмма сигнала, который необходимо раскодировать и получить флаг. На рисунке приведен фрагмент исходного сигнала. Также даны изображения-подсказки.
![Фрагмент изображения сигнала](https://bitbucket.org/superBroDeLock/tasksaltayctf17/downloads/img14.jpg)
Подсказки:
![Первая подсказка ](https://bitbucket.org/superBroDeLock/tasksaltayctf17/downloads/img15.jpg)
![Вторая подсказка](https://bitbucket.org/superBroDeLock/tasksaltayctf17/downloads/img16.jpg)
### Решение
Сигнал получен с инфракрасного датчика для пульта дистанционного управления, популярного в наборах Arduino. Чтобы получить флаг, необходимо определить, какие кнопки нажимал пользователь. На второй подсказке показан адрес в сети Интернет, по которому можно найти подробное описание используемого протокола передачи данных. Соответствие кнопок пульта кодам флага показано на второй подсказке.
![Фрагмент подсказки](https://bitbucket.org/superBroDeLock/tasksaltayctf17/downloads/img17.jpg)
![Фрагмент подсказки](https://bitbucket.org/superBroDeLock/tasksaltayctf17/downloads/img18.jpg)
Для определения кодов кнопок пульта необходимо воспользоваться поиском в интернете:
![Один из вариантов поиска кодов кнопок](https://bitbucket.org/superBroDeLock/tasksaltayctf17/downloads/img19.jpg)
Согласно документации на стандарт NEC, типичное сообщение от пульта состоит из начального импульса, за которым следуют информационные импульсы. В информационном сообщении передаются адрес устройства назначения (по умолчанию посылается код 0x00ff) и код команды, в данном случае код нажатой кнопки. Каждый бит дублируется в сообщении путем инвертирования для увеличения помехоустойчивости.
![Образец кодового сообщения от пульта](https://bitbucket.org/superBroDeLock/tasksaltayctf17/downloads/img20.jpg)
Протокол NEC использует для кодирования информации расстояние между короткими импульсами по 560 мкс. При передаче логического нуля, за импульсом следует промежуток продолжительностью 560 мкс, при передаче логической единицы за импульсом следует промежуток продолжительностью 1680 мкс.
![Сравнение импульсов нуля и единицы](https://bitbucket.org/superBroDeLock/tasksaltayctf17/downloads/img21.jpg)
Также стандартом предусмотрено использование специальных сообщений, сигнализирующих о том, что пользователь не отпускает кнопку пульта. Образец таких сообщений приведен на рисунке.
![Описание кодов повторения](https://bitbucket.org/superBroDeLock/tasksaltayctf17/downloads/img22.jpg)
Изображение сигнала представлено файлом формата BMP. Изображение имеет размер примерно 1 354 тысячи пикселей. Поэтому ручное декодирование невозможно. Для получения цифрового кода можно просканировать среднюю линию изображения (шумы не окажут влияния на информацию). Написав программу для обработки сигнала, можно получить коды кнопок пульта. Существует много способов реализации программы, которая декодирует сигнал.
Вариант кода на языке Си для программы, считывающей коды кнопок:
```C
#include <stdio.h>
typedef unsigned char byte;
int main(){
	FILE *file = fopen("SYGNAL.bmp", "rb");
	if(file==NULL){
		printf("Error open file\n");
		getchar();
		return 0;
	}
	fseek(file, 0x12, SEEK_SET);
	int wFile, hFile;
	fread(&wFile, 4, 1, file);
	fread(&hFile, 4, 1, file);
	printf("W = %d\nH = %d\n", wFile, hFile);
	int lineLength = wFile / 8;
	if(wFile%8){
		lineLength +=1;
	}
	int t;
	if(t = lineLength%4){
		lineLength += 4-t;
	}
	byte *track = (byte*) malloc(lineLength);
	fseek(file, 0x3e, SEEK_SET);
	fread(track, lineLength, 1, file);
	fread(track, lineLength, 1, file);
	fread(track, lineLength, 1, file);
	fread(track, lineLength, 1, file);
	fread(track, lineLength, 1, file);
	byte *mar = malloc(wFile+8);
	int i, j;
	for(i = 0, j = 0; (i<lineLength)&&(j<wFile);i++){
		t = track[i];
		mar[j++] = (t>>7)&0x1;
		mar[j++] = (t>>6)&0x1;
		mar[j++] = (t>>5)&0x1;
		mar[j++] = (t>>4)&0x1;
		mar[j++] = (t>>3)&0x1;
		mar[j++] = (t>>2)&0x1;
		mar[j++] = (t>>1)&0x1;
		mar[j++] = t&0x1;
	}
	free(track);
	const int
		LONG_IMPULSE = 670,
		SHORT_IMPULSE = 40,
		SIGN1_LIMIT = 165,
		SIGN0_LIMIT = 80,
		SIGN1_GAP = 120,
		SIGN0_GAP = 40,
		LONG_PREFACE = 330,
		SHORT_PREFACE = 160;
	int mode = 0;
	/*
	0 - start, before event
	1 - preface
	2 - address
	3 - nAddress
	4 - command
	5 - nCommand
	6 - repeat
	*/
	int
	   ctrlActive = 0,
	   ctrlGap = 0,
	   lastGap = 0;
	int adr = 0, com = 0, command = 0, ncommand = 0;
	byte bufCom[8];
	byte bufNCom[8];
	byte codes[100];
	int iCodes = 0;
	for(i = 0; i<wFile; i++){
		t = mar[i];
		if(t){
			ctrlActive++;
			lastGap = ctrlGap;
			ctrlGap = 0;
		}
		else{
			ctrlActive = 0;
			ctrlGap++;
		}
		switch(mode){
			case 0:
				if(ctrlActive>=LONG_IMPULSE){
					mode = 1;
				}
				break;
			case 1:
				if(ctrlActive==1){
					if(lastGap>=LONG_PREFACE){
						mode = 2;
					}
					else if(lastGap>=SHORT_PREFACE){
						mode = 6;
					}
				}
				
				break;
			case 2:
				if(ctrlActive==1){
					if(lastGap>=SIGN1_GAP){
						adr++;
					}
					else if(lastGap>=SIGN0_GAP){
						adr++;
					}
					if(adr==8){
						mode = 3;
						adr = 0;
					}
				}
				break;
			case 3:
				if(ctrlActive==1){
					if(lastGap>=SIGN1_GAP){
						adr++;
					}
					else if(lastGap>=SIGN0_GAP){
						adr++;
					}
					if(adr==8){
						mode = 4;
						adr = 0;
					}
				}
				
				break;
			case 4:
				if(ctrlActive==1){
					if(lastGap>=SIGN1_GAP){
						bufCom[com] = 1;
						com++;
					}
					else if(lastGap>=SIGN0_GAP){
						bufCom[com] = 0;
						com++;
					}
					if(com==8){
						mode = 5;
						com = 0;
						command = command|(bufCom[0]<<7);
						command = command|(bufCom[1]<<6);
						command = command|(bufCom[2]<<5);
						command = command|(bufCom[3]<<4);
						command = command|(bufCom[4]<<3);
						command = command|(bufCom[5]<<2);
						command = command|(bufCom[6]<<1);
						command = command|(bufCom[7]);
						codes[iCodes++] = command;
						command = 0;
					}
				}
				break;
			case 5:
				if(ctrlActive==1){
					if(lastGap>=SIGN1_GAP){
						bufNCom[com] = 1;
						com++;
					}
					else if(lastGap>=SIGN0_GAP){
						bufNCom[com] = 0;
						com++;
					}
					if(com==8){
						mode = 0;
						com = 0;
						ncommand = ncommand|(bufNCom[0]<<7);
						ncommand = ncommand|(bufNCom[1]<<6);
						ncommand = ncommand|(bufNCom[2]<<5);
						ncommand = ncommand|(bufNCom[3]<<4);
						ncommand = ncommand|(bufNCom[4]<<3);
						ncommand = ncommand|(bufNCom[5]<<2);
						ncommand = ncommand|(bufNCom[6]<<1);
						ncommand = ncommand|(bufNCom[7]);
						mode = 0;
					}
				}
				break;
			case 6:
				if(ctrlActive>=SHORT_IMPULSE){
					mode = 0;
				}
				break;
		}
	}
	free(mar);
	fclose(file);
	FILE *resf = fopen("RES.TXT", "wt");
	for(i=0;i<iCodes;i++){
		fprintf(resf, "code = %x\n", codes[i]);
	}
	fclose(resf);
	return 0;
}
```
Используя соответствие кнопок пульта цифровым кодам флага, можно получить ASCII-код флага.
![Декодированная цифровая последовательность](https://bitbucket.org/superBroDeLock/tasksaltayctf17/downloads/img23.jpg)
Совокупность цифровых кодов формирует ASCII-код флага:
![Полученная последовательность](https://bitbucket.org/superBroDeLock/tasksaltayctf17/downloads/img24.jpg)
Флаг:
```
Altay{nice_remote_control}
```