/* TAREA # 2 (ENCENDIENDO Y APAGANDO UN LED CON CONTROL Y CONTADOR DE EVENTOS)
   INGENIERIA ELECTRONICA UNIVERSIDAD MARIANO GALVEZ
   CURSO:MICROCONTROLADORES Y MICROPROCESADORES
   NOMBRE: WALTER ANTULIO CACERES CORTEZ
   CARNET:091-00-2908
   CATEDRATICO: ING VICTOR VARGAS
  */

#include <stdint.h>

//ESTO SIRVE PARA DEFINIR LAS DIRECCIONES BASE DE LOS REGISTROS RCC Y GPIOA
#define RCC_BASE            0x40000000
#define GPIOA_BASE          0x40020000

//ESTO DE AQUI HABILITA EL RELOJ DE LOS GPIO´S
#define RCC_AHB1ENR         (*((unsigned int*)(RCC_BASE + 0x30)))

// DEFINO LOS REGISTROS GPIOA_MODER, GPIOA_IDR, y GPIOA_ODR CONFIGURA Y LEE ESL ESTADO DEL GPIOA
#define GPIOA_MODER         (*((unsigned int*)(GPIOA_BASE + 0x00)))
#define GPIOA_IDR           (*((unsigned int*)(GPIOA_BASE + 0x10)))
#define GPIOA_ODR           (*((unsigned int*)(GPIOA_BASE + 0x14)))

//AQUI DEFINO LOS PINES PARA EL LES Y EL BOTON
#define LED_PIN             1
#define BOTON_PIN          0

//DEFINO LOS VARIABLES DADO QUE NO SE PUEDE USAR EL HEADER .H HAYS QUE DEFINIRLOS MANUALMENTE
#define uint8_t unsigned char
#define uint32_t unsigned int

//ESTA VARIABLE SIRVE PARA CONTAR EL NUMERO DE VECES QUE SE APACHA EL BOTON
volatile uint8_t apachaboton = 0;

//
void delay(uint32_t ms) {
    // EL RETARDO DE MILI SEGUNDOS REQUERIDO
    for (uint32_t i = 0; i < ms * 5000; ++i) {}
}

int main(void) {

	// ACTIVA EL RELOJ GPIO´S A
    RCC_AHB1ENR |= (1 << 0);

    // ESTO LOQ UE HACE ES CONFIGURAR EL BOTON COMO ENTRADA
    GPIOA_MODER &= ~(3 << (BOTON_PIN * 2));

    // ESTE CONFIGURA EL PIN DEL LED COMO SALIDA
    GPIOA_MODER |= (1 << (LED_PIN * 2));

    while (1) {
        if (!(GPIOA_IDR & (1 << BOTON_PIN))) { // AQUI LE DIGO SI EL BOTON ESTA PRESIONADO
            GPIOA_ODR |= (1 << LED_PIN);       // QUE APAGUE EL LED
            delay(200);                        // EN ESTE TIEMPO DE DELAY
            apachaboton++;                 // DE UNA VEZ SUMA 1 ES DECIR VA CONTANDO E INCREMENTA 1
        } else {                                // Y SI NO
            GPIOA_ODR &= ~(1 << LED_PIN);       // QUE ENCIENDA EL LED
            delay(500);                         // CON ESTE DELAY DE 500 MILI SEG
        }
    }
}
