 RoboticsProject
Here i will walk you through my 2024 robotics project

CEAS DE ALARMA ARDUINO

Introducere
Proiectul propus este un ceas cu alarma digital, construit cu ajutorul unei placi Arduino si a unor componente electronice de baza, cum ar fi un LCD, un buzzer si butoane. Acest ceas va avea capacitatea de a afisa ora curenta si de a permite utilizatorului sa seteze o alarma folosind un joystick pentru navigarea prin interfata. Scopul proiectului este de a combina functionalitatea unui ceas cu o interfata de control interactiva.

Descriere generala
Proiectul consta in realizarea unui sistem hardware si software pentru un ceas cu alarma, avand urmatoarele componente principale:

LCD 16x2 cu modul I2C
LCD-ul este folosit pentru a afisa ora curenta, meniurile de setare si ora alarmei. Modulul I2C reduce numarul de pini utilizati, simplificand conexiunile hardware.

Buzzer
Buzzer-ul emite un sunet atunci cand alarma este activata. Poate fi configurat sa emita diferite tipuri de sunete (de exemplu, tonuri scurte repetate) pentru a face alarma mai eficienta.

Butoane Acestea permit navigarea prin meniuri (de exemplu, selectarea orei si minutului pentru alarma) si confirmarea optiunilor. 

Modul RTC (Real-Time Clock)
Modulul RTC ofera functionalitatea unui ceas in timp real, mentinand ora exacta chiar si atunci cand Arduino este oprit. Acesta comunica cu placa prin protocolul I2C, folosind aceiasi pini ca LCD-ul.


Hardware Design
![8b94dea0-598f-4c97-b3d4-81f6dfb152b3 (1)](https://github.com/user-attachments/assets/c1ff5f3b-906e-4793-9662-47f1eb9c2345)
![WhatsApp Image 2024-12-16 at 20 36 33_5d730e9b](https://github.com/user-attachments/assets/5309e52f-63d9-4d59-8b55-574bd87091d5)
![WhatsApp Image 2024-12-16 at 20 36 33_d19e81ab](https://github.com/user-attachments/assets/bad9d155-81be-4bff-88c1-8e0d59f2d3f0)

Software Design
```
#include <LiquidCrystal.h>


LiquidCrystal lcd(12, 11, 5, 4, 3, 2); // RS pe pinul 12, EN pe pinul 11, D4-D7 pe pinii 5, 4, 3, 2

const int buzzerPin = 9;
const int joystickBtn = 6;
const int stopAlarmBtn = 7;

// Variabile pentru ora
int hour = 10, minute = 14, second =30; 
unsigned long lastMillis = 0;

int alarmHour = -1, alarmMinute = -1;
bool isAlarmActive = false;
bool alarmSet = false;

unsigned long lastButtonPress = 0;
const unsigned long debounceDelay = 300; 

void setup() {
  lcd.begin(16, 2);
  pinMode(buzzerPin, OUTPUT);
  pinMode(joystickBtn, INPUT_PULLUP);
  pinMode(stopAlarmBtn, INPUT_PULLUP);
  Serial.begin(9600); 
}

void loop() {
  updateClock();

  if (!alarmSet) {
    //ora curenta si mesaj
    lcd.setCursor(0, 0);
    lcd.print("Ora: ");
    printTime(hour, minute);

    lcd.setCursor(0, 1);
    lcd.print("Press for alarm");
  } else {
    // afisare ora si alarma
    lcd.setCursor(0, 0);
    lcd.print("Ora: ");
    printTime(hour, minute);

    lcd.setCursor(0, 1);
    lcd.print("Alarm: ");
    printTime(alarmHour, alarmMinute);
  }

  // apasare pt setarea alarmei
  if (digitalRead(joystickBtn) == LOW && (millis() - lastButtonPress > debounceDelay)) {
    lastButtonPress = millis();
    if (!alarmSet) {
      setAlarm();
    }
  }

  // verif daca trb activata alarma
  if (alarmSet && hour == alarmHour && minute == alarmMinute && !isAlarmActive) {
    isAlarmActive = true;
  }

  // s
  if (isAlarmActive) {
    playAlarm();
  }

  // stop alarm daca apesi buton
  if (digitalRead(stopAlarmBtn) == LOW && (millis() - lastButtonPress > debounceDelay)) {
    lastButtonPress = millis();
    isAlarmActive = false;
    noTone(buzzerPin);
  }
}

// actualizare ora
void updateClock() {
  unsigned long currentMillis = millis();

  if (currentMillis - lastMillis >= 1000) {
    lastMillis = currentMillis;
    second++;
    if (second >= 60) {
      second = 0;
      minute++;
      if (minute >= 60) {
        minute = 0;
        hour = (hour + 1) % 24;
      }
    }
  }
}

// setarea alarmei
void setAlarm() {
  lcd.clear();
  lcd.print("Set Alarm Hour:");

  alarmHour = adjustValue(0, 23);

  lcd.clear();
  lcd.print("Set Alarm Minute:");

  alarmMinute = adjustValue(0, 59);

  alarmSet = true;

  lcd.clear();
  lcd.print("Alarm Set!");
  delay(2000);
}

// reglez val
int adjustValue(int minVal, int maxVal) {
  int value = minVal;
  while (true) {
    lcd.setCursor(0, 1);
    lcd.print("Value: ");
    lcd.print(value < 10 ? "0" : ""); lcd.print(value);

    delay(100);

    if (analogRead(A0) < 400) { //joystick stanga
      value = max(value - 1, minVal);
    }
    if (analogRead(A0) > 600) { //joystick dreapta
      value = min(value + 1, maxVal);
    }
    if (digitalRead(joystickBtn) == LOW && (millis() - lastButtonPress > debounceDelay)) {
      lastButtonPress = millis();
      return value;
    }
  }
}

void playAlarm() {
  tone(buzzerPin, 1000, 500); // Ton de 1000 Hz timp de 500 ms
  delay(500);
}

void printTime(int h, int m) {
  lcd.print(h < 10 ? "0" : ""); lcd.print(h);
  lcd.print(":");
  lcd.print(m < 10 ? "0" : ""); lcd.print(m);
}
```

Rezultate Obţinute


Concluzii
