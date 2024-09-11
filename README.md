# lcd-ekranla-mesafe-olcme
#include <LiquidCrystal.h>
int trigPin = 7;
int echoPin = 6;
int sure;
int uzaklik;
int oncekiUzaklik = -1;
int rs = 12, en = 11, d4 = 5, d5 = 4, d6 = 3, d7 = 2;
LiquidCrystal lcd(rs, en, d4, d5, d6, d7);
const int led=10;
int buzzerPin = 9; 

void setup() {
  pinMode(trigPin, OUTPUT);
  pinMode(echoPin, INPUT);
  lcd.begin(16, 2); 
  lcd.setCursor(0, 0); // Sabit başlık yazısı
  lcd.print("Uzaklik:");
  pinMode(led,OUTPUT);
  pinMode(buzzerPin,OUTPUT);
}

void loop() {
  digitalWrite(trigPin, LOW);
  delayMicroseconds(5);
  digitalWrite(trigPin, HIGH);
  delayMicroseconds(10);
  digitalWrite(trigPin, LOW);
  sure = pulseIn(echoPin, HIGH, 11600);
  uzaklik = sure * 0.0345 / 2;
  delay(250);

  if (uzaklik != oncekiUzaklik) {
    lcd.setCursor(0, 1); 
    lcd.print("            ");
    lcd.setCursor(0, 1);
    lcd.print(uzaklik); // Yeni mesafeyi yazdır
    lcd.print(" cm"); 
    oncekiUzaklik = uzaklik; // Önceki uzaklığı güncelle
  }
  if(uzaklik<10){
    digitalWrite(buzzerPin,HIGH);
    digitalWrite(led,1);
  }
  else {
    digitalWrite(led,0);
    digitalWrite(buzzerPin,LOW);
  }
}
