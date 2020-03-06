### Υλικά

1 Arduino ή Funduino

1 μπλε καλώδιο USB

1 αισθητήρα υπερύθυρων

1 τηλεχειριστήριο

1 breadboard

5 καλώδια Αρσενικό-Αρσενικό

11 καλώδια Αρσενικό-Θηλυκό

4 καλώδια Θηλυκό - Θηλυκό

1 Driver TB6612FNG

2 DC κινητήρες

1 θήκη μπαταριών 4xΑΑ και μπαταρίες



### Συνδεσμολογία

Τοποθετούμε τον αισθητήρα στο breadboard.

Κοιτάμε τον αισθητήρα από τη μεριά που είναι το "καρούμπαλο".

1ο άκρο αισθητήρα –> καλώδιο -> Arduino 11

2o άκρο αισθητήρα –> καλώδιο -> Breadboard 2η μπλε γραμμή

3o άκρο αισθητήρα –> καλώδιο -> Breadboard 2η κόκκινη γραμμή

PWMA του driver -> Arduino 5

AIN1 του driver -> Arduino 2

AIN2 του driver -> Arduino 4

STBY του driver -> Arduino 10

PWMB του driver -> Arduino 6

BIN1 του driver -> Arduino 8

BIN2 του driver -> Arduino 12

GND του driver -> Breadboard 2η μπλε γραμμή

VM του driver -> Breadboard 1η κόκκινη γραμμή

VCC του driver -> Breadboard 2η κόκκινη γραμμή

GND του driver -> Breadboard 1η μπλε γραμμή

Breadboard 1η κόκκινη γραμμή -> Κόκκινο καλώδιο θήκης μπαταριών

Breadboard 1η μπλε γραμμή -> Μαύρο καλώδιο θήκης μπαταριών

Breadboard 2η κόκκινη γραμμή -> Arduino 5V

Breadboard 2η μπλε γραμμή -> Arduino Ground

A01 του driver -> ένα άκρο DC κινητήρα 1

A02 του driver -> άλλο άκρο DC κινητήρα 1

B01 του driver -> ένα άκρο DC κινητήρα 2

B02 του driver -> άλλο άκρο DC κινητήρα 2


### Κώδικας

Πριν μεταφέρουμε τον κώδικα χρειάζεται να ελέγξουμε αν υπάρχει η βιβλιοθήκη για τις υπέρυθρες στην εφαρμογή Arduino του υπολογιστή μας. Ζητάμε τη βοήθεια ενός mentor.

Αν δεν υπάρχει την κατεβάζουμε από εδώ

https://github.com/shirriff/Arduino-IRremote

και την προσθέτουμε

Ομοίως ελέγχουμε αν το πρόγραμμα του Arduino έχει τη βιβλιοθήκη για τον driver που έχουμε (TB6612FNG).

Αν όχι την κατεβάζουμε από εδώ

https://github.com/sparkfun/SparkFun_TB6612FNG_Arduino_Library

και την εγκαθιστούμε


#include <IRremote.h>

#include <Servo.h>

#include <SparkFun_TB6612.h>

int RECV_PIN = 11;

#define AIN1 2

#define AIN2 4

#define PWMA 5

#define BIN1 8

#define BIN2 12

#define PWMB 6

#define STBY 10

IRrecv irrecv(RECV_PIN);

decode_results results;

int pos = 0;

const int offsetA = 1;

const int offsetB = 1;

Motor motor1 = Motor(AIN1, AIN2, PWMA, offsetA, STBY);

Motor motor2 = Motor(BIN1, BIN2, PWMB, offsetB, STBY);


void setup()

{

  Serial.begin(9600);
  
  irrecv.enableIRIn(); // Start the receiver
  
}



void loop()

{

  if (irrecv.decode(&results))
  
    {
    
     Serial.println(results.value);
     
     //Serial.println(results.value, HEX);
     
     if (results.value == 1000) {
     
     motor1.drive(255, 1000); 

     motor2.drive(255, 1000); 

     
     }
     
     else if (results.value == 1001) {
     
     motor1.drive(-255, 1000); 

     motor2.drive(-255, 1000); 
     
     }

     else if (results.value == 1002) {
     
     motor1.drive(255, 1000); 

     motor2.drive(-255, 1000); 
     
     }

     else if (results.value == 1003) {
     
     motor1.drive(-255, 1000); 

     motor2.drive(255, 1000); 
     
     }

     else if (results.value == 1004) {
     
     motor1.brake(); 

     motor2.brake(); 
     
     }


     irrecv.resume(); // Receive the next value
     
    }
    
}


Ανοίγουμε το παράθυρο "Εργαλεία / Παρακολούθηση Σειριακής" ή "Tools / Serial Monitor"

Πατάμε τα κουμπιά του τηλεχειριστήριου και παρατηρούμε τις τιμές που εμφανίζει το Arduino για κάθε ένα από αυτά.

Αποφασίζουμε ποια κουμπιά θέλουμε για να κινείται το όχημα μπρος, πίσω, δεξιά, αριστερά και να σταματάει και σημειώνουμε τους κωδικούς τους.

Βάζουμε αυτούς τους κωδικούς στη θέση των 1000, 1001, 1002, 1003, 1004 στον κώδικα

Ξαναγράφουμε τον κώδικα στο Arduino και δοκιμάζουμε να πατήσουμε αυτά τα κουμπιά.



(Βασισμένο στα άρθρα
https://www.instructables.com/id/Arduino-Infrared-Remote-tutorial/
https://www.instructables.com/id/Driving-Small-Motors-With-the-TB6612FNG/
)
