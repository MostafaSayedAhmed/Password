# Password
#include <Keypad.h>
#include<Wire.h>
#include<LiquidCrystal_I2C.h>
LiquidCrystal_I2C lcd(0x27,16,2);
char x[]="1234567";
const byte ROWS = 4; //four rows
const byte COLS = 4; //four columns
//define the cymbols on the buttons of the keypads
char hexaKeys[ROWS][COLS] = {
  {'D','C','B','A'},
  {'#','9','6','3'},
  {'0','8','5','2'},
  {'*','7','4','1'}
};
byte rowPins[ROWS] = {2,3,4, 5}; //connect to the row pinouts of the keypad
byte colPins[COLS] = {6, 7, 8, 9}; //connect to the column pinouts of the keypad

//initialize an instance of class NewKeypad
Keypad customKeypad = Keypad( makeKeymap(hexaKeys), rowPins, colPins, ROWS, COLS);
void setup(){
  pinMode(11,OUTPUT);
  pinMode(12,OUTPUT);
  pinMode(13,OUTPUT);
 lcd.init();
lcd.backlight();
 lcd.clear();
  Serial.begin(9600);
}
  
void loop(){
digitalWrite(11,LOW);digitalWrite(12,LOW);digitalWrite(13,LOW);
 lcd.clear();
lcd.setCursor(1,0);
lcd.print("Enter Password = ");
digitalWrite(12,HIGH);
lcd.setCursor(4,1);
  int flag=1;
 //lcd.setCursor(0,1);
  char customKey[7]={0};
  for(int i=0;i<7;i++){
        customKey[i]=customKeypad.getKey();
  if(customKey[i]=='D') {customKey[i]=0; customKey[i-1]=0;i-=2;lcd.setCursor(5+i,1);lcd.print(' ');lcd.setCursor(5+i,1);}
  else if(customKey[i]){lcd.print('*');}
  else{i--;}
  }
  digitalWrite(12,LOW);
  for(int i=0;i<7;i++){
  if(customKey[i] != x[i]){
   flag=0  ;}}
  if(flag)
  {lcd.clear();lcd.setCursor(0,0);lcd.print("Correct Password ") ;digitalWrite(11,HIGH);delay(5000);}
  else
  {lcd.clear();lcd.setCursor(1,0);lcd.print("Wrong Password ");digitalWrite(13,HIGH);delay(3000);}
}
