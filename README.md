// Define Encoder Pins

const int encoderPinA = 2; // Internal Interrupt 0

const int encoderPinB = 3; // Internal Interrupt 1

// Variables to store position

volatile long encoderCount = 0;

long lastReportedPos = 0;

void setup() {

  Serial.begin(9600);

  // Set pins as inputs
  
  pinMode(encoderPinA, INPUT);
  
  pinMode(encoderPinB, INPUT);

  // Attach interrupt to Pin A
  
  // We trigger on RISING edge to detect movement
  attachInterrupt(digitalPinToInterrupt(encoderPinA), handleEncoder, RISING);
  
  Serial.println("Encoder Ready. Rotate to see values...");
}

void loop() {

  // Only print when the value changes to keep Serial monitor clean
  
  if (encoderCount != lastReportedPos) {
  
    Serial.print("Position: ");
    
    Serial.println(encoderCount);
    
    lastReportedPos = encoderCount;
    
  }
}

// Interrupt Service Routine (ISR)

void handleEncoder() {

  // Check the state of Pin B when Pin A goes HIGH
  
  // If B is HIGH, we are moving in one direction; if LOW, the other.
  
  if (digitalRead(encoderPinB) == HIGH) {
    
    encoderCount++;
    
  } else {
    
    encoderCount--;
  }
}
