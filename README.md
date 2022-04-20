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
