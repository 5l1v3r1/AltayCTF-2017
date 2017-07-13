# Название
#### Countries
# Стоимость
#### 172
# Категория
#### Stego, Misk
# Решение
Видим большой коллаж с фото, на нескольких фото имеется флаг. Присматриваемся к этим флагам и видим, что на флаге Франции есть маленький квадратик. 
>con_1
Вытаскиваем его. Квадрат состоит из полос разного оттенка, необходимо вытащить цвета. Далее есть несколько путей решения:
Первый путь, пишем программу, которая составляет массив только интенсивности синего цвета, так как остальные равны 0, получаем строку: 101 154 164 141 171 173 167 145 137 100 162 145 137 143 162 100 172 171 175
Пример решения, на языке C#:
using System;
using System.Collections.Generic;
using System.ComponentModel;
using System.Data;
using System.Drawing;
using System.Linq;
using System.Text;
using System.Threading.Tasks;
using System.Windows.Forms;
using System.IO;
using System.Media;

namespace pr
{
    public partial class Form1 : Form
    {
        string str;
        Bitmap im2;
        string[] array = new string[19];
        int[] arr = new int[19];
        public Form1()
        {
            InitializeComponent();
            
        }

        private void Form1_Load(object sender, EventArgs e)
        {
            picLoad();
        }
        void picLoad()
        {
            im2 = new Bitmap("C:\\Users\\palim\\YandexDisk\\CTF\\CTF_разработка\\AltayCTF2017\\Countries of my dream\\6.png");    
            str="";
            paint();
            textBox1.Text = str;

            
        }
        void paint()
        {
            Color c;
            for (int i = 0; i < 19; i++)
            {
                    c = im2.GetPixel(1, i);
                    str = str + c.B;
            }

        }
    }
}
Второй вариант решения: в фотошопе, либо любом фоторедакторе с помощью так же брать интенсивности синего цвета.
Далее получаем восьмеричный код, преобразовав который в текаст получаем флаг.
### Флаг:
>Altay{we_@re_cr@zy}
