#include <Servo.h>

Servo Servoleft;     // Creates Servo object for the left servo
Servo Servoright;    // Creates Servo object for the right servo

const int servoleftPin = 4;     // Pin for left servo
const int servorightPin = 6;    // Pin for right servo
int arr[7] = {A0,A1, A2, A3, A4, A5,A6};
float preverror=0;  // IR sensor pins
int kp =4;
float ki= 0.5;
float kd =0.2;
float correction =0;
int P =0;
int I=0;
int D =0;
void setup() {
  Serial.begin(9600);

  Servoleft.attach(servoleftPin);    // Attach left servo
  Servoright.attach(servorightPin);  // Attach right servo

  // Servoleft.write(55);      // Move left servo to 180°
  // Servoright.write(125);       // Move right servo to 0°
  for (int i = 0; i < 7; i++) {
    pinMode(arr[i], INPUT);
  }
}

void loop() {
  int ir[7]={0,0,0,0,0,0,0};
  bool outofline = true;
  for (int i = 0; i < 7; i++) {
    if(analogRead(arr[i])<220){
      ir[i]=1;
      outofline= false;

    }  
  }
  // if line lost
  //   bool lineLost = true;
  // for (int i = 0; i < 5; i++) {
  //   if (ir[i] == 1) {
  //     lineLost = false;
  //     break;
  //   }
  // }

  // if (lineLost) {
  //   Servoleft.write(90);   // Stop left
  //   Servoright.write(90);  // Stop right
  //   Serial.println("Line lost — stopping.");
  //   delay(100);
  //   return;  // Skip rest of loop
  // }
  // for proportionality
  int error =0;
  if (ir[3]) error = 1; 
  if (ir[4]){
    error = 3;
    if(ir[3]==1){
      error = 2;
    }
  } 
  if (ir[2]) {
    error = -1;
    if(ir[3]==1){
      error=0;
    }
  } 
  if (ir[1]) {
    error = -3;
    if(ir[2]==1){
      error=-2;
    }
  }
  if (ir[0]) {
    error = -5;
    if(ir[1]==1){
      error=-4;
    }
  }
  if (ir[5]) {
    error = 5;
    if(ir[4]==1){
      error=4;
    }
  } 
  // if (ir[6]) {
  //   error = 7;
  //   if(ir[5]==1){
  //     error=5.5;
  //   }
  // }

    
  I += error;
   D = error-preverror;
   P=error;

  correction = kp*P;
  preverror=error;
  int leftspeed = 50+correction;
  int rightspeed= 130+correction;
  if (outofline){
      leftspeed=130;
      rightspeed=50;
  }


    Servoleft.write(leftspeed);    
  Servoright.write(rightspeed);

    
  
  
}
