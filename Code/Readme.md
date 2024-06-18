# Code for microcontrollers and sensors 

double light (int RawADC0)                     
{                       
 double Vout=RawADC0*0.0048828125;                      
 //int lux=500/(10*((5-Vout)/Vout));//use this equation if the LDR is in the upper part of the divider  int lux=(2500/Vout-500)/10;              
 return lux;              
}                   
//Temp Measure                 
#include <OneWire.h>                
#include <DallasTemperature.h>                   
// Data wire is plugged into digital pin 2 on the Arduino                   
#define ONE_WIRE_BUS 1                       
// Setup a oneWire instance to communicate with any OneWire device                               
OneWire oneWire(ONE_WIRE_BUS);                          
// Pass oneWire reference to DallasTemperature library                              
DallasTemperature sensors(&oneWire);                           
#include <SPI.h>                      
#include <SD.h>                           
int T = 0;                        
const int chipSelect = SDCARD_SS_PIN;                      
void setup() {                        
 // Open serial communications and wait for port to open:                      
 Serial.begin(9600); sensors.begin(); // Start up the library                       
 Serial.print("Initializing SD card...");                        
 // see if the card is present and can be initialized:                         
 if (!SD.begin(chipSelect)) {                         
 Serial.println("Card failed, or not present");                        
 }                                 
 Serial.println("card initialized.");                     
}                     
void loop() {                          
 // Send the command to get temperatures                
 sensors.requestTemperatures();                          
 // make a string for assembling the data to log:                      
 int Light = (int(light(analogRead(1))));                   
 int Temp = sensors.getTempCByIndex(0);                
 T = T + 1;                                  
 // open the file. note that only one file can be open at a time,                    
 // so you have to close this one before opening another.                             
 File dataFile = SD.open("datalog.txt", FILE_WRITE); // if the file is available, write to it:        
 if (dataFile) {            
 Serial.print("Time");               
 Serial.print(T);                      
 dataFile.println("Time" );                
 dataFile.println(T);                 
 dataFile.println("Temperature:");                      
 Serial.println("Temperature");             
 dataFile.println(Temp);               
Serial.println(Temp);                
 dataFile.println(" Light:");              
 Serial.println("Light");                   
 dataFile.println(analogRead(A4));                  
Serial.println(analogRead(A4));                  
//Serial.println(" Lux");                
 dataFile.close();                 
 // print to the serial port too:                     
 }                          
 // if the file isn't open, pop up an error:               
 else {              
 Serial.println("error opening datalog.txt");         
 }                       
 delay(1000);                  
}                       
