### Υλικά

1 Arduino

1 μπλε καλώδιο USB

1 αισθητήρα υπερύθυρων

1 τηλεχειριστήριο

1 breadboard

6 καλώδια Αρσενικό-Αρσενικό

9 καλώδια Αρσενικό-Θηλυκό

1 Σερβοκινητήρας

1 Driver TB6612FNG

1 DC κινητήρας

(προαιρετικά μπαταρία)



### Συνδεσμολογία

Τοποθετούμε τον αισθητήρα στο breadboard.

Κοιτάμε τον αισθητήρα από τη μεριά που είναι το "καρούμπαλο".

1ο άκρο αισθητήρα –> καλώδιο -> Arduino 11

2o άκρο αισθητήρα –> καλώδιο -> Breadboard μπλε γραμμή

3o άκρο αισθητήρα –> καλώδιο -> Breadboard κόκκινη γραμμή

Κόκκινο/πορτοκαλί καλώδιο του servo –> καλώδιο -> Breadboard κόκκινη γραμμή

Κίτρινο καλώδιο του servo καλώδιο -> καλώδιο –> Arduino 9

Μαύρο/καφέ καλώδιο του servo καλώδιο -> καλώδιο –> Breadboard μπλε γραμμή

PWMA του driver -> Arduino 5

INA1 του driver -> Arduino 2

INA2 του driver -> Arduino 4

STBY του driver -> Arduino 9 --> άλλο

VM του driver -> Breadboard κόκκινη γραμμή(*)

VCC του driver -> Breadboard κόκκινη γραμμή (*)

GND του driver -> Breadboard μπλε γραμμή

GND του driver -> Breadboard μπλε γραμμή

Breadboard κόκκινη γραμμή -> Arduino 5V

Breadboard μπλε γραμμή -> Arduino Ground

A01 του driver -> ένα άκρο DC κινητήρα

A02 του driver -> άλλο άκρο DC κινητήρα

(*) Αν συνδέσεις παραπάνω από έναν κινητήρα ή δώσεις στον κινητήρα ρεύμα από μπαταρία αντί για το arduino, τότε το VM και το VCC δεν πρέπει να είναι μαζί.
Το VM πρέπει να συνδεθεί στο κόκκινο καλώδιο μίας μπαταρίας και το ένα από το GND στο μαύρο καλώδιο.
Δες μαζί με έναν mentor πως πρέπει να κάνεις τη σύνδεση σε μία τέτοια περίπτωση


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

#define STBY 9 --> άλλο

IRrecv irrecv(RECV_PIN);

decode_results results;

Servo myservo;

int pos = 0;

const int offsetA = 1;

Motor motor1 = Motor(AIN1, AIN2, PWMA, offsetA, STBY);


void setup()

{

  myservo.attach(9);

  Serial.begin(9600);
  
  irrecv.enableIRIn(); // Start the receiver
  
}



void loop()

{

  if (irrecv.decode(&results))
  
    {
    
     Serial.println(results.value);
     
     //Serial.println(results.value, HEX);
     
     if (results.value == 1234) {
     
     myservo.write(45); 
     
     }
     
     else if (results.value == 4321) {
     
     myservo.write(135); 
     
     }

     else if (results.value == 5678) {
     
     motor1.drive(20, 1000); 
     
     }

     else if (results.value == 8765) {
     
     motor1.brake(); 
     
     }


     irrecv.resume(); // Receive the next value
     
    }
    
}


Ανοίγουμε το παράθυρο "Εργαλεία / Παρακολούθηση Σειριακής" ή "Tools / Serial Monitor"

Πατάμε τα κουμπιά του τηλεχειριστήριου και παρατηρούμε τις τιμές που εμφανίζει το Arduino για κάθε ένα από αυτά.

Αποφασίζουμε ποια κουμπιά θέλουμε για τον σερβοκινητήρα και τον DC κινητήρα και σημειώνουμε τους κωδικούς τους.

Βάζουμε αυτούς τους κωδικούς στη θέση των 1234, 4321, 5678, 7654 στον κώδικα

Ξαναγράφουμε τον κώδικα στο Arduino και δοκιμάζουμε να πατήσουμε αυτά τα κουμπιά.



(Βασισμένο στα άρθρα
https://www.instructables.com/id/Arduino-Infrared-Remote-tutorial/
https://www.arduino.cc/en/Tutorial/Sweep
)
