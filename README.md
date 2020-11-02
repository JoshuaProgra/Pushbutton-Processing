# Pushbutton-Processing
import processing.serial.*;

boolean estadoBoton = false;
Serial miSerial;
color colorApagado = color(200, 0, 0);
color colorEncendido = color(0, 200, 0);
float distancia;
float val;      // Data received from the serial port
Serial myPort;
String portStream;

int B1in = 0;

void setup() {
  size(800, 800);
  background(255);
  stroke(160);
  fill(50);
  String portName = Serial.list()[0];
  myPort = new Serial(this, portName, 9600);
  myPort.bufferUntil('\n');
}

void draw() {
  if ( myPort.available() > 0) {  // If data is available,
    val = myPort.read();         // read it and store it in val
  }
  if (estadoBoton) {
    fill(colorEncendido);
  } else {
    fill(colorApagado);
  }
  ellipse(600,200,350,350);  
   if (portStream != null) {    
    if (portStream.length() == 6 && portStream.charAt(0) == 'S' && portStream.charAt(3) == 'E') {
      B1in = int(portStream.substring(1, 2));   

      if (B1in == 1) {
        fill(0, 255, 0);
      } else {
        fill(255, 0, 0);
      }      
      rect(100, 400, 250, 250);
    }
  }
}

void mouseClicked() {
 println(mouseX,mouseY);
 distancia = dist(600,200,mouseX,mouseY);
 println(distancia);
 if (distancia<175){
     estadoBoton = !estadoBoton;
     myPort.write('a');
   }
}

void serialEvent(Serial myPort) {
  portStream = myPort.readString();
}
