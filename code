class Particle {
  PVector position;
  PVector velocity;
  PVector acceleration;
  float lifespan;
  int diameter;

// Конструктор экземпляра класса
Particle(PVectorl) {
acceleration = new PVector(0, 0.03);
    velocity = new PVector(random(-1, 1), random(-2, 0));
    position = l.copy();
    diameter = 25;
  }

  // Метод запуска жизненного цикла частицы
  void run() {
    update();
    checkBorders();
    checkBehavior(myPlatformLow);
    checkBehavior(myPlatformHi);
    display();
  }

  // Метод обновления местоположения
  void update() {
    velocity.add(acceleration);
    position.add(velocity);
    //acceleration.mult(0);

}

  // Метод отображения частицы
  void display() {
stroke(255);
    fill(255);
    ellipse(position.x, position.y, diameter, diameter);
}
  // Метод проверки соударения с границами экрана
void checkBorders() {
    if (position.x >= width-diameter/2) {
            velocity.x *= -1;
            position.x = width-diameter/2; } 
        else if (position.x <= diameter/2) {
            velocity.x = velocity.x *  -1;
            position.x = diameter/2;} 
        if (position.y >= height-diameter/2) { 
            velocity.y = velocity.y * -1; 
            position.y = height-diameter/2;}
        else if (position.y <= diameter/2) {
            velocity.y = velocity.y * -1;
            position.y = diameter/2;}   
  }
  // Метод проверки взаимодействия с платформой
  void checkBehavior(Platform pL) {
    PVector plPos = pL.position;
     if ((abs(plPos.y - position.y)<=10) && ((abs(plPos.x - position.x) <= 75))) {
       print(abs(plPos.x - position.x));
       velocity.mult(-1);
     }
  }
}


Листинг кода модуля Platform.pde

class Platform {
  PVector position;
  PVector velocity;
  PVector acceleration;
  int pWidth;
  int pHeight;
  float mass;
  // Конструкторэкемпляракласса
  Platform(int _pWidth, int _pHeight, int _posX, int _posY) {
    acceleration = new PVector(3.05, 0);
    velocity = new PVector(random(-1, 1), 0);
    position = new PVector(_posX, _posY);
    pWidth = _pWidth;
    pHeight = _pHeight;
    mass = 30;
}

  // Метод запуска жизненного цикла частицы
  void run() {
    update();
    checkBorders();
    display();
}

  // Метод обновления местоположения платформы
void update() {
    velocity.add(acceleration);
    position.add(velocity);
    acceleration.mult(0);
    //lifespan -= 1.0;
  }

  // Метод отображения платформы
  void display() {
    stroke(255);
    fill(255);
    rectMode(CENTER);
    rect(position.x, position.y, pWidth, pHeight);
  }
  // Метод воздействия силы на платформу
  void applyForce(PVector force) {
    PVector f = force.copy();
    f.div(mass);
acceleration.add(f);
  }

  // Метод проверки соударения с границами экрана
void checkBorders() {
    if (position.x >= width-pWidth/2) {
            velocity.x *= -1; } 
        else if (position.x <= pWidth/2) {
            velocity.x = velocity.x *  -1; }
        if (position.y >= height-pHeight/2) { 
            velocity.y = velocity.y * -1; }
        else if (position.y <= pHeight/2) {
            velocity.y = velocity.y * -1; }   
  }
}


Листинг модуля Attractor.pde

class Attractor {
  float mass;
  PVector location;
  float G;

  Attractor() {
    location = new PVector(width/2,950);
mass = 100;
    G = 0.4;
  }
  // Метод-конструктор экземпляра класса
PVector attract(Platform p) {
    PVector force = PVector.sub(location,p.position);
    float distance = force.mag();
    distance = constrain(distance,10.0,50.0);
    force.normalize();
    float strength = (G * mass * p.mass) / (distance * distance);
    //float strength = 1 / (distance * distance);
    force.mult(strength);
    return force;
  }
  //Метод отображения аттрактора
  void display() {
    stroke(0);
  }
}

Листинг модуля ParticleSystem.pde


class ParticleSystem {
  ArrayList<Particle> particles;
  PVector origin;

  ParticleSystem(PVector position) {
    origin = position.copy();
    particles = new ArrayList<Particle>();
  }

  void addParticle() {
    particles.add(new Particle(origin));
  }

  void run() {
    for (int i = particles.size()-1; i >= 0; i--) {
      Particle p = particles.get(i);
      p.run();
    }
}
}


Листинг модуля ArcoGame.pde

import android.content.Intent;
import android.os.Bundle;

import ketai.net.bluetooth.*;
import ketai.ui.*;
import ketai.net.*;

import oscP5.*;

KetaiBluetooth bt;
String info = "";
KetaiList klist;
PVector remoteMouse = new PVector();

ArrayList<String> devicesDiscovered = new ArrayList();
boolean isConfiguring = true;
String UIText;

ParticleSystem ps;
Platform myPlatformLow, myPlatformHi;
Attractor plAttractor;
PVector attractionForce; 
PFont myFont;

//********************************************************************
// The following code is required to enable bluetooth at startup.
//********************************************************************
void onCreate(Bundle savedInstanceState) {
  super.onCreate(savedInstanceState);
  bt = new KetaiBluetooth(this);
}

void onActivityResult(int requestCode, int resultCode, Intent data) {
  bt.onActivityResult(requestCode, resultCode, data);
}

void setup(){
   size(900, 1024); 
   //orientation(PORTRAIT);
   ps = new ParticleSystem(new PVector(width/2, 50));
   ps.addParticle();
   myPlatformLow = new Platform(150, 20, width/2, 950);
   myPlatformHi = new Platform(150, 20, width/2, 50);
   plAttractor = new Attractor();
    //start listening for BT connections
   bt.start();

   UIText =  "d - discover devices\n" +
    "b - make this device discoverable\n" +
    "c - connect to device\n     from discovered list.\n" +
    "p - list paired devices\n" +
    "i - Bluetooth info";
}

void draw(){

  if (isConfiguring)
  {
    ArrayList<String> names;
    background(0, 0, 0);

    //based on last key pressed lets display
    //  appropriately
    if (key == 'i')
      info = getBluetoothInformation();
    else
    {
      if (key == 'p')
      {
        info = "Paired Devices:\n";
        names = bt.getPairedDeviceNames();
      }
      else
      {
        info = "Discovered Devices:\n";
        names = bt.getDiscoveredDeviceNames();
      }

      for (int i=0; i < names.size(); i++)
      {
info += "["+i+"] "+names.get(i).toString() + "\n";
      }
    }
    myFont = createFont("Georgia", 32);
    textFont(myFont);
    text(UIText + "\n\n" + info, 5, 90);
  }
  else {
  background(0);

  ps.run();
  attractionForce = plAttractor.attract(myPlatformLow);
  myPlatformLow.applyForce(attractionForce);
  myPlatformLow.run();
  myPlatformHi.run();
  }
}

String getBluetoothInformation()
{
  String btInfo = "Server Running: ";
  btInfo += bt.isStarted() + "\n";
  btInfo += "Discovering: " + bt.isDiscovering() + "\n";
  btInfo += "Device Discoverable: "+bt.isDiscoverable() + "\n";
  btInfo += "\nConnected Devices: \n";

  ArrayList<String> devices = bt.getConnectedDeviceNames();
  for (String device: devices)
  {
    btInfo+= device+"\n";
  }

  return btInfo;
}

void onBluetoothDataEvent(String who, byte[] data)
{ 
  String Str; 
  if (isConfiguring)
    return;

  //KetaiOSCMessage is the same as OscMessage
  //   but allows construction by byte array
  /*
  KetaiOSCMessage m = new KetaiOSCMessage(data);
  if (m.isValid())
  {
    if (m.checkAddrPattern("/remoteMouse/"))
    {
      if (m.checkTypetag("ii"))
      {
        remoteMouse.x = m.get(0).intValue();
        remoteMouse.y = m.get(1).intValue();
      }
    }

  }*/
  Str = new String(data);
  text(Str, 5, 300);
}

Листинг модуля UI.pde

void mousePressed()
{
  //keyboard button -- toggle virtual keyboard
  if (mouseY <= 50 && mouseX > 0 && mouseX < width/3)
    KetaiKeyboard.toggle(this);
  else if (mouseY <= 50 && mouseX > width/3 && mouseX < 2*(width/3)) //config button
  {
    isConfiguring=true;
  }
  else if (mouseY <= 50 && mouseX >  2*(width/3) && mouseX < width) // draw button
  {
    if (isConfiguring)
    {
      //if we're entering draw mode then clear canvas
      background(78, 93, 75);

      isConfiguring=false;
    }

  }
}

public void keyPressed() {
if (key =='c')
  {
    //If we have not discovered any devices, try prior paired devices
    if (bt.getDiscoveredDeviceNames().size() > 0)
      klist = new KetaiList(this, bt.getDiscoveredDeviceNames());
    else if (bt.getPairedDeviceNames().size() > 0)
      klist = new KetaiList(this, bt.getPairedDeviceNames());
  }
  else if (key == 'd')
  {
    bt.discoverDevices();
  }
  else if (key == 'x')
    bt.stop();
  else if (key == 'b')
  {
    bt.makeDiscoverable();
  }
  else if (key == 's')
  {
    bt.start();
  }
}


void drawUI()
{
  //Draw top shelf UI buttons

  pushStyle();
  fill(255);
  stroke(255);
  rect(0, 0, width/3, 50);

  if (isConfiguring)
  {
    noStroke();
    fill(255, 255, 255);
  }
  else
    fill(255);

  rect(width/3, 0, width/3, 50);

  if (!isConfiguring)
  {  
    noStroke();
    fill(255, 255, 255); 
  }
  else
  {
    fill(0);
    stroke(255);
  }
  rect((width/3)*2, 0, width/3, 50);

  fill(255);
  myFont = createFont("Georgia", 32);
  textFont(myFont);
  text("Keyboard", 5, 30); 
  text("Bluetooth", width/3+5, 120); 
  text("Interact", width/3*2+5, 120); 

  popStyle();
}  

void onKetaiListSelection(KetaiList klist)
{
  String selection = klist.getSelection();
  bt.connectToDeviceByName(selection);

  //dispose of list for now
  klist = null;
}
