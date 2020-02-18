### Υλικά

1 Arduino

1 μπλε καλώδιο USB

1 αισθητήρα υπερύθυρων

1 τηλεχειριστήριο

1 breadboard

6 καλώδια Αρσενικό-Αρσενικό

1 Σερβοκινητήρας




### Συνδεσμολογία

Τοποθετούμε τον αισθητήρα στο breadboard.

Κοιτάμε τον αισθητήρα από τη μεριά που είναι το "καρούμπαλο".

1ο άκρο αισθητήρα –> καλώδιο -> Arduino 11

2o άκρο αισθητήρα –> καλώδιο -> Arduino GND

3o άκρο αισθητήρα –> καλώδιο -> Arduino 5V

Κόκκινο/πορτοκαλί καλώδιο του servo –> καλώδιο -> Arduino 5V

Κίτρινο καλώδιο του servo καλώδιο -> καλώδιο –> Arduino 9

Μαύρο/καφέ καλώδιο του servo καλώδιο -> καλώδιο –> Arduino GND



### Κώδικας

Πριν μεταφέρουμε τον κώδικα χρειάζεται να ελέγξουμε αν υπάρχει η βιβλιοθήκη για τις υπέρυθρες στην εφαρμογή Arduino του υπολογιστή μας. Ζητάμε τη βοήθεια ενός mentor.

Αν δεν υπάρχει την κατεβάζουμε από εδώ

https://github.com/shirriff/Arduino-IRremote

και την προσθέτουμε



#include <IRremote.h>

#include <Servo.h>

int RECV_PIN = 11;

IRrecv irrecv(RECV_PIN);

decode_results results;

Servo myservo;

int pos = 0;




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
     
     irrecv.resume(); // Receive the next value
     
    }
    
}


Ανοίγουμε το παράθυρο "Εργαλεία / Παρακολούθηση Σειριακής" ή "Tools / Serial Monitor"

Πατάμε τα κουμπιά του τηλεχειριστήριου και παρατηρούμε τις τιμές που εμφανίζει το Arduino για κάθε ένα από αυτά.

Αποφασίζουμε ποια κουμπιά θέλουμε για να κινούν τον σερβοκινητήρα και σημειώνουμε τους κωδικούς τους.

Βάζουμε αυτούς τους κωδικούς στη θέση των 1234 και 4321 στον κώδικα

Ξαναγράφουμε τον κώδικα στο Arduino και δοκιμάζουμε να πατήσουμε αυτά τα κουμπιά.



(Βασισμένο στα άρθρα
https://www.instructables.com/id/Arduino-Infrared-Remote-tutorial/
https://www.arduino.cc/en/Tutorial/Sweep
)
