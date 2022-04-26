Autor: Henrique Rosa  [link](https://github.com/https://github.com/Henriquer88)

Repositóio no MBED [link](https://os.mbed.com/users/henriquer/code/)

# Menu Display TFT

Utilização do Encoder/Decoder KY-040 Rotacional com Display TFT

# Objetivo

 Criação de um Menu básico utilizando o Encoder/Decoder KY-040
 
# Materiais

* Nucleo F103
* Compilador MBED [link](https://os.mbed.com/)
* Display 2.4 Touch 
* Encoder/Decoder KY-040 Rotacional

# Orientações

  Para a realizção dessa prototipação vamos utilizar tutorial Shield TFT [link](https://github.com/Henriquer88/Shield-TFT) como base para criação dos botões no display
  
# Encoder/Decoder KY-040 Rotacional   

 No tutorial Shield TFT criamos três botôes retângulares, sendo que cada botão corresponde a um determinado comando, só que dessa vez não utilizaremos a função Touch Screen, mas sim o Encoder/Decoder KY-040. 
 
 
![encoder](https://user-images.githubusercontent.com/60757810/164022619-56b37e97-23e2-4164-972b-78bad606735d.PNG)


# Leitura dos Pulsos do KY-040

* Código 

```javascript
//***********************Encoder/Decoder KY-040*******************************//
//**19/04/2022
//**************************** Henrique **************************************//

#include "mbed.h"
#include "Encoder.h"  // Biblioteca responsável pela leitura dos pulsos

//****************************************************************************//
//****************************************************************************//



//****************************************************************************//
int pulse;  // Variável responsável atribuída para leitura dos pulsos
//****************************************************************************//





//****************************************************************************//
int main()
{


    EncoderAli Enc(PB_13,PB_14,PB_15); //  Ligação do KY-040 nos pinos da Nucleo  -  DT, CLK, SW
    Enc.setRange(1,20);               // Função responsável por setar o Range do Encoder
    while(1) {
        printf("\n\r PULSOS: %d; ",Enc.getState()); // Enc.getState()      -> Função Responsável por ler os pulsos 
                                                    // Enc.getButtonState()-> Função responsável por ler o estado do botão SW
        
        pulse = Enc.getState();
        

    }
}

//****************************************************************************//

```

Obs: Devido ao efeito de Debounce é recomendado o uso de capacitores 100nf nos pinos CLK , DT e SW


# Menu básico com o KY-040
 Usando como base o código do Menu Touch, vamos apenas substituir algumas linhas.
* Código 
* 
```javascript


//******************************Henrique**************************************//

//********************Programa Exemplo  Menu com botões***********************//


//***********************Display TFT-ILI9341 Toutch***************************//


//*****************************Biblioteca*************************************//


#include "mbed.h"
#include "Arduino.h"
#include <MCUFRIEND_kbv.h>
MCUFRIEND_kbv tft;
#include "TouchScreen_kbv_mbed.h"
#include "Encoder.h"                            // Biblioteca para uso do KY-040

//************************Configuração do Display*****************************//

//****************************************************************************//

const int TS_LEFT=121,TS_RT=922,TS_TOP=82,TS_BOT=890;
const PinName XP = D8, YP = A3, XM = A2, YM = D9;   //next common configuration
DigitalInOut YPout(YP);
DigitalInOut XMout(XM);


long map(long x, long in_min, long in_max, long out_min, long out_max)
{
   return (x - in_min) * (out_max - out_min) / (in_max - in_min) + out_min;
}

TouchScreen_kbv ts = TouchScreen_kbv(XP, YP, XM, YM, 300);
TSPoint_kbv tp;

// Valores para detectar a pressão do toque

#define MINPRESSURE 10
#define MAXPRESSURE 1000

//****************************************************************************//

//****************************************************************************//

//***********************Orientação Display***********************************//

uint8_t Orientation = 0;

//****************************************************************************//

//****************************************************************************//


bool botao_1 = 0;
bool botao_2 = 0;
bool botao_3 = 0;
int pulse;                            //  Variável responsável por ler os pulsos 
bool sw;

//***********************Tabela de Cores**************************************//

#define BLACK   0x0000
#define BLUE    0x001F
#define RED     0xF800
#define GREEN   0x07E0
#define CYAN    0x07FF
#define MAGENTA 0xF81F
#define YELLOW  0xFFE0
#define WHITE   0xFFFF

//****************************************************************************//

//****************************************************************************//


void draw()

{

   tft.drawRoundRect(5, 15, 154, 50, 5, WHITE);
   tft.setTextColor(GREEN);
   tft.setTextSize(3);
   tft.setCursor(15, 30);
   tft.println("Button 1");

   tft.drawRoundRect(5, 70, 154, 50, 5, WHITE);
   tft.setTextColor(GREEN);
   tft.setTextSize(3);
   tft.setCursor(15,85);
   tft.println("Button 2");

   tft.drawRoundRect(5, 125, 154, 50, 5, WHITE);
   tft.setTextColor(GREEN);
   tft.setTextSize(3);
   tft.setCursor(15,140);
   tft.println("Button 3");

}


void show_tft(void)
{
    
    EncoderAli Enc(PB_13,PB_14,PB_15); //DT, CLK, SW
    Enc.setRange(1,16);               //  Configurando o Range do Encoder 


   tft.setTextSize(2);


   tft.setTextColor(MAGENTA,BLUE);
   
   while (1) {

    pulse = Enc.getState();
    sw = Enc.getButtonState();
   printf("\n\r PULSOS: %d; ",Enc.getState()); 
       tp = ts.getPoint();
       YPout.output();
       XMout.output();

       if (tp.z < MINPRESSURE && tp.z > MAXPRESSURE)

           tp.x = tft.width() - (map(tp.x, TS_RT, TS_LEFT, tft.width(), 0));
           tp.y = tft.height() - (map(tp.y, TS_BOT, TS_TOP, tft.height(), 0));


if (pulse>=2 && pulse<=5) {  // Criação de uma janela para seleção do primeito botão
           
           if(sw==true) {

               tft.fillRoundRect(5, 15, 154, 50, 5, RED);
               tft.setCursor(15, 30);
               tft.println("Button 1");
               //tft.fillScreen(BLUE);
              // sw =!sw;
           }

           else {

               tft.fillRoundRect(5, 15, 154, 50, 5, BLACK);
               tft.drawRoundRect(5, 15, 154, 50, 5, WHITE);
               tft.setTextColor(GREEN);
               tft.setTextSize(3);
               tft.setCursor(15, 30);
               tft.println("Button 1");
               //tft.fillScreen(BLACK);

               //sw =!sw;

           }
       }


if (pulse>=6 && pulse<=10) {// Criação de uma janela para seleção do segundo botão
           
           if(sw==true) {

               tft.fillRoundRect(5, 70, 154, 50, 5, BLUE);
               tft.setTextColor(GREEN);
               tft.setTextSize(3);
               tft.setCursor(15, 85);
               tft.println("Button 2");
               botao_2 =!botao_2;
           }

           else {


               tft.fillRoundRect(5, 70, 154, 50, 5, BLACK);
               tft.drawRoundRect(5, 70, 154, 50, 5, WHITE);
               tft.setTextColor(GREEN);
               tft.setTextSize(3);
               tft.setCursor(15,85);
               tft.println("Button 2");
               botao_2 =!botao_2;
           }
       }

if (pulse>11 && pulse<=14) {// Criação de uma janela para seleção do terceiro botão
           
           if(sw==true) {

               tft.fillRoundRect(5, 125, 154, 50, 5, MAGENTA);
               tft.setTextColor(GREEN);
               tft.setTextSize(3);
               tft.setCursor(15, 140);
               tft.println("Button 3");
               botao_3 =!botao_3;
           }

           else {
               
               tft.fillRoundRect(5, 125, 154, 50, 5, BLACK);
               tft.drawRoundRect(5, 125, 154, 50, 5, WHITE);
               tft.setTextColor(GREEN);
               tft.setTextSize(3);
               tft.setCursor(15,140);
               tft.println("Button 3");
               botao_3 =!botao_3;
           }
       }
       
   }
}





void setup(void)
{
    


   tft.reset();
   tft.begin();
   tft.setRotation(Orientation);
   tft.fillScreen(BLACK);
   draw();
   show_tft();


   delay(1000);
}

void loop()
{


}

//****************************************************************************//

//****************************************************************************//

```
Compilando o código teremos o resultado abaixo :





https://user-images.githubusercontent.com/60757810/165302732-546c6fd7-507b-44c0-94a0-5d50f1abaf4e.mp4












