#include <LiquidCrystal.h>
#include <Wire.h>
#include <SFE_MMA8452Q.h>

MMA8452Q accel;
const int pingPin = 6;
const int numRows = 2;
const int numCols = 16;
LiquidCrystal lcd(12,11,5,4,3,2);

int ping(int pingPin)
{
long duration, cm;
pinMode(pingPin, OUTPUT);
digitalWrite(pingPin, LOW);
delayMicroseconds(500);
digitalWrite(pingPin, HIGH);
delayMicroseconds(10);
pinMode(pingPin, INPUT);
duration = pulseIn(pingPin, HIGH);
cm = microsecondsToCentimeters(duration);
return cm;
}

long microsecondsToCentimeters(long microseconds){
  return microseconds/59;
}
  
  
void setup()
{
  accel.init();
Serial.begin(9600);
lcd.begin(numCols,numRows);
}

void loop(){
  
  int cm = ping(pingPin);
  Serial.println(cm);
  if (accel.available())
  accel.read();
  
    lcd.clear();
    if(accel.cx<=-.5 && cm <=300){
  
  lcd.print("I'm Slowing Down");
 
}else if(accel.cx>=-.5 && cm<=300){
  lcd.print("Back Off");
  lcd.setCursor(0,1);
  lcd.print("Distance: ");
  lcd.setCursor(9,1);
  lcd.print(cm);
  lcd.setCursor(12,1);
  lcd.print("cm");
  lcd.print("   ");
}}
