# PRACTICA5
## Codi sencer
```cpp
#include <Arduino.h>
#include <Wire.h> 


void setup() {
   Wire.begin(); 
    Serial.begin(115200); 
    while (!Serial); // Leonardo: wait for serial monitor 
     Serial.println("\nI2C Scanner");
}

void loop() {
   byte error, address; 
   int nDevices; 
   Serial.println("Scanning..."); 
   nDevices = 0; 
   for(address = 1; address < 127; address++ ) 
   { 
    // The i2c_scanner uses the return value of 
    // the Write.endTransmisstion to see if 
    // a device did acknowledge to the address. 
     Wire.beginTransmission(address); 
     error = Wire.endTransmission(); 
     if (error == 0) 
     { 
      Serial.print("I2C device found at address 0x"); 
      if (address<16) 
        Serial.print("0"); 
        Serial.print(address,HEX); 
        Serial.println("  !"); 
        nDevices++; 
     } 
     else if (error==4) 
     { 
      Serial.print("Unknown error at address 0x"); 
      if (address<16) 
        Serial.print("0"); 
        Serial.println(address,HEX); 
     }     
  } 
  if (nDevices == 0) 
    Serial.println("No I2C devices found\n"); 
  else 
    Serial.println("done\n"); 
    delay(5000);           // wait 5 seconds for next scan
}
```

## Explicació del codi per parts
### Biblioteques
```cpp
#include <Arduino.h>
#include <Wire.h>
```
Aquestes línies importen les biblioteques necessàries. Arduino.h és la biblioteca base per als programes Arduino, mentre que Wire.h és la biblioteca per a la comunicació I2C.

### Setup
```cpp
void setup() {
   Wire.begin(); 
    Serial.begin(115200); 
    while (!Serial); // Leonardo: wait for serial monitor 
     Serial.println("\nI2C Scanner");
}
```
- Wire.begin(): Inicialitza la comunicació I2C.
- Serial.begin(115200): Inicialitza la comunicació sèrie a una velocitat de 115200 bauds.
- while (!Serial);: Per a les plaques Arduino Leonardo, espera fins que el monitor sèrie estigui connectat.
- Serial.println("\nI2C Scanner");: Mostra un missatge inicial al monitor sèrie.

### Loop
```cpp
void loop() {
   byte error, address; 
   int nDevices; 
   Serial.println("Scanning..."); 
   nDevices = 0; 
   for(address = 1; address < 127; address++ ) 
   { 
    // The i2c_scanner uses the return value of 
    // the Write.endTransmisstion to see if 
    // a device did acknowledge to the address. 
     Wire.beginTransmission(address); 
     error = Wire.endTransmission(); 
     if (error == 0) 
     { 
      Serial.print("I2C device found at address 0x"); 
      if (address<16) 
        Serial.print("0"); 
        Serial.print(address,HEX); 
        Serial.println("  !"); 
        nDevices++; 
     } 
     else if (error==4) 
     { 
      Serial.print("Unknown error at address 0x"); 
      if (address<16) 
        Serial.print("0"); 
        Serial.println(address,HEX); 
     }     
  } 
  if (nDevices == 0) 
    Serial.println("No I2C devices found\n"); 
  else 
    Serial.println("done\n"); 
    delay(5000);           // wait 5 seconds for next scan
}
```
#### Variables
- byte error, address;: error s'utilitza per emmagatzemar el codi d'error del I2C, i address és la direcció I2C que s'està escanejant.
- int nDevices;: Emmagatzema el nombre de dispositius I2C trobats.
#### Procediment
1. Serial.println("Scanning...");: Mostra un missatge al monitor sèrie indicant que es comença l'escaneig.
2. nDevices = 0;: Inicialitza el comptador de dispositius I2C trobats.
3. for(address = 1; address < 127; address++ ): Bucle que escaneja totes les adreces possibles del bus I2C (de 1 a 126).
4. Wire.beginTransmission(address);: Comença una transmissió I2C a la direcció especificada.
5. error = Wire.endTransmission();: Finalitza la transmissió i obté un codi d'error. Aquest codi indica si el dispositiu a la direcció especificada va respondre.
- Si error == 0: Un dispositiu I2C va respondre a la direcció.
- ```cpp
  Serial.print("I2C device found at address 0x");
if (address < 16) 
  Serial.print("0"); 
Serial.print(address, HEX); 
Serial.println("  !");
nDevices++;
```
Mostra la direcció del dispositiu I2C trobat i incrementa el comptador de dispositius.
- Si error == 4: Hi ha un error desconegut.
```cpp
Serial.print("Unknown error at address 0x");
if (address < 16) 
  Serial.print("0"); 
Serial.println(address, HEX);
```
Mostra un missatge indicant un error desconegut a la direcció especificada.
6. Després de completar l'escaneig de totes les adreces:
```cpp
if (nDevices == 0) 
  Serial.println("No I2C devices found\n"); 
else 
  Serial.println("done\n"); 
```
Si no es troben dispositius, es mostra un missatge indicant-ho. Si es troben dispositius, es mostra "done".
7. delay(5000);: Espera 5 segons abans de tornar a començar el bucle per escanejar de nou.
