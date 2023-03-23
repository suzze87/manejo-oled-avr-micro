# OLED for AVR mikrocontrollers
Library for oled-displays with SSD1306, SSD1309 or SH1106 display-controller connected with I2C or SPI at an AVR Atmel Atmega like Atmega328P.

<img src="https://github.com/suzze87/manejo-oled-avr-micro/blob/master/oled.jpg?raw=true" width="500">

Esta biblioteca le permite mostrar texto y / o gráfico en oled-display. 

La biblioteca necesita menos de 2 kilobytes de memoria flash y 3 bytes de sram en modo texto, en la biblioteca de modo gráfico necesita menos de 3 kilobytes de memoria flash y 1027 bytes de sram estática para que pueda usar pantallas oled, por ejemplo, con Atmega48PA (solo con textmode). 

La biblioteca solo se prueba con una pantalla de 128x64 píxeles, la resolución más baja no se ha probado, pero también debería funcionar. 

Para utilizar  una biblioteca I2C propia, debe ajustar la función i2c en lcd-library. La configuración para I2C-bus debe establecerse en i2c.h La configuración para la visualización debe establecerse en lcd.h Si desea utilizar caracteres como, por ejemplo, ä establezca el conjunto de caracteres de entrada del compilador en utf-8 y el conjunto de caracteres ejecutivo del compilador en iso-8859-15 (consulte la línea 115 de makefile). Condición de prueba: Pantalla: SSD1306 OLED, Compilador Optimizelevel: -Os, μC: Atmega328p @ 8 MHz RC interno

Memory:
<table>
  <tr>
    <th>Modul</th>
    <th>Flash</th>
    <th>Static RAM</th>
  </tr>
  <tr>
    <td>I2C-Core</td>
    <td>220 Bytes</td>
    <td>0 Bytes</td>
  </tr>
  <tr>
    <td>FONT</td>
    <td>660 Bytes</td>
    <td>0 Bytes</td>
  </tr>
  <tr>
    <td>OLED (Text-Mode)</td>
    <td>1437 Bytes</td>
    <td>2 Bytes</td>
  </tr>
  <tr>
    <td>OLED (Graphic-Mode)</td>
    <td>2561 Bytes</td>
    <td>1026 Bytes</td>
  </tr>
 </table>
  
  

Velocidad (imprima 20 caracteres (1 línea) en tamaño normal para mostrar):

<table>
  <tr>
    <th>Mode</th>
    <th>Time</th>
    <th>I2C-Speed</th>
  </tr>
  <tr>
    <td>OLED (Text-Mode)</td>
    <td>4.411 ms</td>
    <td>400 kHz</td>
  </tr>
  <tr>
    <td>OLED (Text-Mode)</td>
    <td>15.384 ms</td>
    <td>100 kHz</td>
  </tr>
  <tr>
    <td>OLED (Graphic-Mode)</td>
    <td>26.603 ms</td>
    <td>400 kHz</td>
  </tr>
  <tr>
    <td>OLED (Graphic-Mode)</td>
    <td>96.294 ms</td>
    <td>100 kHz</td>
  </tr>
 </table>


ejemplo:

```c
//****main.c****//
#include "lcd.h"


int main(void){
  lcd_init(LCD_DISP_ON);    // Inicie LCD y enciende
  
  lcd_puts("Hello World");  // pone una cadena desde la RAM al display (TEXTMODE) o al buffer (GRAPHICMODE)
  lcd_gotoxy(0,2);          // Establecer el cursor en la primera columna de la línea 3
  lcd_puts_p(PSTR("String from flash"));  //pone una cadena desde la flash al display (TEXTMODE) o al buffer (GRAPHICMODE)
#if defined GRAPHICMODE
  lcd_drawCircle(64,32,7,WHITE); //dibuja un circulo en el buffer
  lcd_display();                  // envia el buffer al display
#endif
  for(;;){
    //main loop
  }
  return 0;
}
```
Ejemplo para caracteres con doble tamaño:
```c
//****main.c****//
#include "lcd.h"

int main(void){
    
    lcd_init(LCD_DISP_ON);
    lcd_clrscr();
    lcd_set_contrast(0x00);
    lcd_gotoxy(4,1);
    lcd_puts("Normal Size");
    lcd_charMode(DOUBLESIZE);
    lcd_gotoxy(0,4);
    lcd_puts("  Double  \r\n   Size");
    lcd_charMode(NORMALSIZE);
        
#ifdef GRAPHICMODE
        lcd_display();
#endif
    for(;;){
    //main loop
    }   
    return 0;
}
```

<img src="https://github.com/suzze87/manejo-oled-avr-micro/blob/master/bigchars.JPG?raw=true" width="500">
