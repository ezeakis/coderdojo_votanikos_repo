### Υλικά

1 Arduino

1 μπλε καλώδιο USB

1 αισθητήρα υπερύθυρων

1 τηλεχειριστήριο

1 breadboard

5 καλώδια Αρσενικό-Αρσενικό

1 led

Μία αντίσταση 330 Ω



### Συνδεσμολογία

Τοποθετούμε τον αισθητήρα στο breadboard.

Κοιτάμε τον αισθητήρα από τη μεριά που είναι το "καρούμπαλο".

1ο άκρο αισθητήρα –> καλώδιο -> Arduino 11

2o άκρο αισθητήρα –> καλώδιο -> Arduino GND

3o άκρο αισθητήρα –> καλώδιο -> Arduino 5V

Θετικό ποδαράκι του LED –> Arduino D12

Αρνητικό ποδαράκι του LED –> 1ο άκρο αντίστασης

2ο άκρο αντίστασης -> Arduino GND



### Κώδικας

Πριν μεταφέρουμε τον κώδικα χρειάζεται να ελέγξουμε αν υπάρχει η βιβλιοθήκη για τις υπέρυθρες στην εφαρμογή Arduino του υπολογιστή μας. Ζητάμε τη βοήθεια ενός mentor.

Αν δεν υπάρχει την κατεβάζουμε από εδώ

https://github.com/shirriff/Arduino-IRremote

και την προσθέτουμε



#include <IRremote.h>

int RECV_PIN = 11;

IRrecv irrecv(RECV_PIN);

decode_results results;

int led = 12;



void setup()

{

 pinMode(led, OUTPUT);
 
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
     
     digitalWrite(led, HIGH);
     
     }
     
     else if (results.value == 4321) {
     
     digitalWrite(led, LOW);
     
     }
     
     irrecv.resume(); // Receive the next value
     
    }
    
}


Ανοίγουμε το παράθυρο "Εργαλεία / Παρακολούθηση Σειριακής" ή "Tools / Serial Monitor"

Πατάμε τα κουμπιά του τηλεχειριστήριου και παρατηρούμε τις τιμές που εμφανίζει το Arduino για κάθε ένα από αυτά.

Αποφασίζουμε ποια κουμπιά θέλουμε για να ανάβουν και να σβήνουν το λαμπάκι και σημειώνουμε τους κωδικούς τους.

Βάζουμε αυτούς τους κωδικούς στη θέση των 1234 και 4321 στον κώδικα

Ξαναγράφουμε τον κώδικα στο Arduino και δοκιμάζουμε να πατήσουμε αυτά τα κουμπιά.



(Βασισμένο στο άρθρο https://www.instructables.com/id/Arduino-Infrared-Remote-tutorial/)
