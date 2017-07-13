# RemoteSignal
### �������
���� ������������� �������, ������� ���������� ������������� � �������� ����. �� ������� �������� �������� ��������� �������. ����� ���� �����������-���������.
![�������� ����������� �������](https://bitbucket.org/superBroDeLock/tasksaltayctf17/downloads/img14.jpg)
���������:
![������ ��������� ](https://bitbucket.org/superBroDeLock/tasksaltayctf17/downloads/img15.jpg)
![������ ���������](https://bitbucket.org/superBroDeLock/tasksaltayctf17/downloads/img16.jpg)
### �������
������ ������� � ������������� ������� ��� ������ �������������� ����������, ����������� � ������� Arduino. ����� �������� ����, ���������� ����������, ����� ������ ������� ������������. �� ������ ��������� ������� ����� � ���� ��������, �� �������� ����� ����� ��������� �������� ������������� ��������� �������� ������. ������������ ������ ������ ����� ����� �������� �� ������ ���������.
![�������� ���������](https://bitbucket.org/superBroDeLock/tasksaltayctf17/downloads/img17.jpg)
![�������� ���������](https://bitbucket.org/superBroDeLock/tasksaltayctf17/downloads/img18.jpg)
��� ����������� ����� ������ ������ ���������� ��������������� ������� � ���������:
![���� �� ��������� ������ ����� ������](https://bitbucket.org/superBroDeLock/tasksaltayctf17/downloads/img19.jpg)
�������� ������������ �� �������� NEC, �������� ��������� �� ������ ������� �� ���������� ��������, �� ������� ������� �������������� ��������. � �������������� ��������� ���������� ����� ���������� ���������� (�� ��������� ���������� ��� 0x00ff) � ��� �������, � ������ ������ ��� ������� ������. ������ ��� ����������� � ��������� ����� �������������� ��� ���������� ������������������.
![������� �������� ��������� �� ������](https://bitbucket.org/superBroDeLock/tasksaltayctf17/downloads/img20.jpg)
�������� NEC ���������� ��� ����������� ���������� ���������� ����� ��������� ���������� �� 560 ���. ��� �������� ����������� ����, �� ��������� ������� ���������� ������������������ 560 ���, ��� �������� ���������� ������� �� ��������� ������� ���������� ������������������ 1680 ���.
![��������� ��������� ���� � �������](https://bitbucket.org/superBroDeLock/tasksaltayctf17/downloads/img21.jpg)
����� ���������� ������������� ������������� ����������� ���������, ��������������� � ���, ��� ������������ �� ��������� ������ ������. ������� ����� ��������� �������� �� �������.
![�������� ����� ����������](https://bitbucket.org/superBroDeLock/tasksaltayctf17/downloads/img22.jpg)
����������� ������� ������������ ������ ������� BMP. ����������� ����� ������ �������� 1 354 ������ ��������. ������� ������ ������������� ����������. ��� ��������� ��������� ���� ����� �������������� ������� ����� ����������� (���� �� ������ ������� �� ����������). ������� ��������� ��� ��������� �������, ����� �������� ���� ������ ������. ���������� ����� �������� ���������� ���������, ������� ���������� ������.
������� ���� �� ����� �� ��� ���������, ����������� ���� ������:
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
��������� ������������ ������ ������ �������� ����� �����, ����� �������� ASCII-��� �����.
![�������������� �������� ������������������](https://bitbucket.org/superBroDeLock/tasksaltayctf17/downloads/img23.jpg)
������������ �������� ����� ��������� ASCII-��� �����:
![���������� ������������������](https://bitbucket.org/superBroDeLock/tasksaltayctf17/downloads/img24.jpg)
����:
```
Altay{nice_remote_control}
```