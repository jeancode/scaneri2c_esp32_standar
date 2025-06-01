# I2C Scanner para Platformio

Este proyecto es un escáner básico para detectar dispositivos conectados al bus I2C usando Arduino.

---

## Descripción

El programa busca dispositivos en todas las direcciones posibles (1 a 126) del bus I2C y muestra por el puerto serial qué dispositivos responde y en qué dirección.

El código utiliza los pines 8 y 9 para la comunicación I2C con `Wire.begin(8,9);` (esto es útil si usas un microcontrolador que permite asignar pines SDA y SCL, como algunos ESP32).

---

## Requisitos

- Arduino IDE
- Placa compatible con la librería Wire
- Dispositivos conectados al bus I2C

---

## Código

```cpp
void setup()
{
  Wire.begin(8,9);
  Serial.begin(115200);
  while (!Serial);
  Serial.println("\nI2C Scanner");
}

void loop()
{
  byte error, address;
  int nDevices;

  Serial.println("Scanning...");
  nDevices = 0;

  for(address = 1; address < 127; address++ )
  {
    Serial.println(String(address));
    Wire.beginTransmission(address);
    error = Wire.endTransmission();
    Serial.println("Error: " + String(error));

    if (error == 0)
    {
      Serial.print("I2C device found at address 0x");
      if (address<16)
        Serial.print("0");
      Serial.print(address,HEX);
      Serial.println("  !");
      nDevices++;
    }
    else if (error == 4)
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
}
