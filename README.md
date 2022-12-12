# Verkefni-5

### Dagbók

21 Nóvember: Prufaði jarðvegsmælinn og fékk kóðasýnidæmi til að virka.

22 Nóvember - 24 Nóvember: Fór að skoða BH1750.

28 Nóvember - 5 Desember: Átti vandræði með að tengja Raspberry Pi við tölvuna gengnum netið. Ég komst að því að það var vonlaust að bæta við nýrri kóðaskrá inn á kortið og þurfti að nota Raspberry Pi Imager til að setja upp nettenginguna. 

6 Desember: Skoðaði BH1750.

8 Desember: Fékk ljósskynjarann til að virka.

9 - 11 Desember: Fór að skoða hvernig vatnsdælan virkar en fann því miður ekkert mikið.


### Kóðinn fyrir jarðsvegsmælinn
/***************************************************
 This example reads Capacitive Soil Moisture Sensor.
 Created 2015-10-21
 By berinie Chen <bernie.chen@dfrobot.com>
 GNU Lesser General Public License.
 See <http://www.gnu.org/licenses/> for details.
 All above must be included in any redistribution
 ****************************************************/
/***********Notice and Trouble shooting***************
 1.Connection and Diagram can be found here: https://www.dfrobot.com/wiki/index.php?title=Capacitive_Soil_Moisture_Sensor_SKU:SEN0193
 2.This code is tested on Arduino Uno.
 3.Sensor is connect to Analog 0 port.
 ****************************************************/

const int AirValue = 520; //you need to replace this value with Value_1
const int WaterValue = 260; //you need to replace this value with Value_2
int intervals = (AirValue - WaterValue)/3;
int soilMoistureValue = 0;
void setup() {
 Serial.begin(115200); // open serial port, set the baud rate to 9600 bps
}
void loop() {
soilMoistureValue = analogRead(12); //put Sensor insert into soil
if(soilMoistureValue > WaterValue && soilMoistureValue < (WaterValue + intervals))
{
 Serial.println("Very Wet");
}
else if(soilMoistureValue > (WaterValue + intervals) && soilMoistureValue < (AirValue - intervals))
{
 Serial.println("Wet");
}
else if(soilMoistureValue < AirValue && soilMoistureValue > (AirValue - intervals))
{
 Serial.println("Dry");
}
delay(1000);
}


### Kóðinn fyrir ljósskynjarann http://www.pibits.net/code/raspberry-pi-bh1750-light-sensor.php

#!/usr/bin/python
import smbus
import time
 
#Define some constants from the datasheet
DEVICE     = 0x23 # Default device I2C address
POWER_DOWN = 0x00 # No active state
POWER_ON   = 0x01 # Power on
RESET      = 0x07 # Reset data register value
ONE_TIME_HIGH_RES_MODE = 0x20
 
bus = smbus.SMBus(1)  #Rev 2 Pi uses 1
 
def convertToNumber(data):
  #Simple function to convert 2 bytes of data
  #into a decimal number
  return ((data[1] + (256 * data[0])) / 1.2)
 
def readLight(addr=DEVICE):
  data = bus.read_i2c_block_data(addr,ONE_TIME_HIGH_RES_MODE)
  return convertToNumber(data)
 
def main():
 
  while True:
    print("Light Level : " + str(readLight()) + " lux")
    time.sleep(0.5)
 
if __name__=="__main__":
   main()
