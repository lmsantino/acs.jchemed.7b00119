# Code for Arduino

```
int smDirectionPin = 2; //Direction pin
int smStepPin = 3; //Stepper pin
int smEnablePin = 7; //Motor enable pin
byte byteRead;

void setup(){
  /*Sets all pin to output; the microcontroller will send them (the pins) bits, it will not expect to receive any bits from these pins.*/
  pinMode(smDirectionPin, OUTPUT);
  pinMode(smStepPin, OUTPUT);
  pinMode(smEnablePin, OUTPUT);
 
  digitalWrite(smEnablePin, HIGH); //Disables the motor, so it can rest until it is called upon
 
  Serial.begin(9600);
}
void loop(){
   if (Serial.available()) {
    /* read the most recent byte */
    byteRead = Serial.read();

    if (byteRead==101){
  /*Here we are calling the rotate function to rotate the stepper motor*/
  rotate(-1008, 1); } //The motor rotates 1008 steps making the probe head move vertically up 1 mm at a slow speed (pause 300ms between each step)

// Attention: the steps depend on the motor and on the acme rods used

    if (byteRead==100){
 rotate(1008, 1); }//The motor rotates 1008 steps making the probe head move vertically down 1 mm at the same speed

   if (byteRead==119){
 rotate(-100, 1); } //The motor rotates 100 steps making the probe head move vertically up 100um at the same speed

   if (byteRead==115){
  rotate(100, 1); }//The motor rotates 100 steps making the probe head move vertically down 100um at the same speed

   if (byteRead==113){
 rotate(-10, 1); }//The motor rotates 10 steps making the probe head move vertically up 10um at the same speed

   if (byteRead==97){
  rotate(10, 1); }//The motor rotates 10 steps making the probe head move vertically down 10um at the same speed
   }}
/*The rotate function rotates the stepper motor. It accepts two arguments: 'steps' and 'speed'*/
void rotate(int steps, float speed){
  digitalWrite(smEnablePin, LOW); //Enabling the motor, so it will move when asked to
 
  /*This section looks at the 'steps' argument and stores 'HIGH' in the 'direction' variable if */
  /*'steps' contains a positive number and 'LOW' if it contains a negative*/
  int direction;
   if (steps > 0){
    direction = HIGH;
  }else{
    direction = LOW;
  }
   speed = 1/speed *300; //Calculating speed
  steps = abs(steps); //Stores the absolute value of the content in 'steps' back into the 'steps' variable
   digitalWrite(smDirectionPin, direction); //Writes the direction (from our if statement above), to the EasyDriver DIR pin
   /*Steppin'*/
  for (int i = 0; i < steps; i++){
    digitalWrite(smStepPin, HIGH);
    delayMicroseconds(speed);
    digitalWrite(smStepPin, LOW);
    delayMicroseconds(speed);
  }
 
  digitalWrite(smEnablePin, HIGH); }//Disables the motor, so it can rest until the next time when is called upon
```
