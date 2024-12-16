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


Rezultate Obţinute


Concluzii
