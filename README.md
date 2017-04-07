# tarea_proyecto2

import ddf.minim.*;
Minim minim;
AudioPlayer sonido1;
AudioPlayer sonido2;
AudioPlayer sonido3;
AudioPlayer sonido4;
//

import shiffman.box2d.*;
import org.jbox2d.collision.shapes.*;
import org.jbox2d.common.*;
import org.jbox2d.dynamics.*;

import java.util.concurrent.*; 

CopyOnWriteArrayList<Ball> balls;

Box2DProcessing box2d;   

Pin[]      pins;

Slide[]    slides;
Bottom     bottom;
int[]      cilCounter;

int n = 8;         
int k = 5;         
int np;             

int maxBalls = 500; 
float radius = 5;   
float c = 10.0;      
float A = 30.0;     
float wopening = 5*c;

float hcyl = 50.0; 
float wcyl;
float lowMargin = 60;
float Hlowest;
float slideLowBorder;
int inc;
int escenario=0;
float peX2=100;
float peY2=200;
float tam2=100;
float ojox2=20;

float personaje=0;;
//
float peX=470;
float peY=200;
float tam=100;
float ojox=20;
//
PFont font;
String cadena ="Elige tu PERSONAJE";
String cadena2 ="Para iniciar el juego";
int x=width-400;
float ct=255;
String titulo ="RELOJ DE ARENA";
String inicio ="Inicio";
int y=height-100;
people[] personnages;


void setup() {
  size(600, 600);
 
  //musica
minim = new  Minim(this);
if (escenario==0){
  sonido1 = minim.loadFile("pista1.mp3", 1024);
   sonido1 .loop();
}

  frameRate(60);
  smooth();
    noStroke();
  stroke(255,128);
  float pas = 20;
  smooth();
  personnages = new people[0];
  for(float a=0;a<(width/pas-1);a++){
  for(float b=0;b<(height/pas-1);b++){
    if(random(3)>1){
    new people(a*pas+pas*0.5, b*pas+pas*0.5, pas*0.8);
    }
  }
     
  }
   font = loadFont("SitkaSmall-Italic-48.vlw");
  textFont(font);
  box2d = new Box2DProcessing(this);
  box2d.createWorld();
  
  Hlowest = height - lowMargin;
  wcyl = (width - c * k) / (float)k;
  np = (n/2)*(2*k) + (n ) * k/2;
  slideLowBorder = A*(2+n) - Hlowest + hcyl;
 
  balls = new CopyOnWriteArrayList<Ball>(); 
  

  bottom = new Bottom(new Vec2(width/2, height-10), width, c); 
  
  slides = new Slide[2];
  Vec2[] vertices = new Vec2[4];

  vertices[0] = new Vec2( -c/2, 500);
  vertices[1] = new Vec2( -c/2, slideLowBorder);
  vertices[2] = new Vec2( width/2 - wopening/2, -c);
  vertices[3] = new Vec2( width/2 - wopening/2,  0);
  slides[0] = new Slide(vertices, 4, new Vec2(0, -slideLowBorder), 0);
  
  vertices[0] = new Vec2( +c/2, 500);
  vertices[1] = new Vec2( +c/2, slideLowBorder);
  vertices[2] = new Vec2( -(width/2 - wopening/2), -c);
  vertices[3] = new Vec2( -(width/2 - wopening/2),  0);
  slides[1] = new Slide(vertices, 4, new Vec2(width, -slideLowBorder), 0);
  
  pins = new Pin[np];
  int count = 0;
  int mult = 0;
  for (int i=0; i < np; i++) {
    count = i%(2*k );
    if( (count == 0) || ((count % k) == 0 ) ) {
      mult++;
    }
    float x = ((c + wcyl)/2) + (count%k)*(wcyl+c) + (count/k)*((wcyl+c)/2.0);
    float y = Hlowest - ( (mult*A) + (hcyl + A) );
    pins[i] = new Pin(x, y+100,c/2);
  }
  escenario=0;

}

void draw() {
   if (inc==1){
      movimiento();
   }
   background(0);
          if (escenario==1){
  botonpersonaje1();
  botonpersonaje2();
  personajemujer();
  personajehombre();
  anunciodepersonaje();
  }
   if (escenario==0){
     fondo();
      botonInicio();
      titulo();
   }

     if (escenario==2){
       
  background(#F1F7AC);
  playgame();
 iniciaJuego();
    if (inc==1){
      
        if(balls.size() < maxBalls) {
      // generate new balls
      Ball p = new Ball(radius, 70, 0.02);
      balls.add(p);
      
    }
      fill(#D3BF26);
        for(Ball p: balls) {
    p.display();
  }
    }
    
  translate(peX2+100,peY2+360);
   scale(0.1);
    personajemujer();
     
 }
  if (escenario==3){
  background(#F1F7AC);
  playgame();
 iniciaJuego();
    if (inc==1){
        if(balls.size() < maxBalls) {
      // generate new balls
      Ball p = new Ball(radius, 70, 0.02);
      balls.add(p);
      
    }
      fill(#D3BF26);
        for(Ball p: balls) {
    p.display();
  }
    }
  
   scale(0.1);
    personajehombre();
  
 }
  
}

void playgame(){
   box2d.step();
  
  stroke(0);
  fill(120);  

  for (int i=0; i < np; i++) {
    pins[i].display();
  }

  bottom.display();

  for (int i=0; i < 2; i++) {
    slides[i].display();
  }

}
void iniciaJuego(){
  int x= width/20;
  int y=height/5;
  int w=width/10;
  int h=height/5;
  
  if ((mouseX>x)&&(mouseX<x+w)&&(mouseY>y)&&(mouseY<y+h)){
    if (mousePressed==true){
       inc=1;
    }
    fill(0);
   
}
else{
  
  fill(255);
}

rect(x,y,w,h,200);
fill(#FCED75);
textSize(20);
  text("\nP\nL\nA\nY",60,130);
}
void botonpersonaje1(){
    int x= width/50;
  int y=height/50;
  int w=width-400;
  int h=height-150;
  
  if ((mouseX>x)&&(mouseX<x+w)&&(mouseY>y)&&(mouseY<y+h)){
    fill(#8D24FF);
       if (mousePressed==true){
       escenario=2;
       sonido2.pause();
        
          sonido3 = minim.loadFile("we.mp3", 1024);
          sonido3 .loop();
    }
}
else{
  
  fill(255);
}

rect(x,y,w,h,10);
}
void botonpersonaje2(){
    int x= 350+width/50;
  int y=height/50;
  int w=width-400;
  int h=height-150;
  
  if ((mouseX>x)&&(mouseX<x+w)&&(mouseY>y)&&(mouseY<y+h)){
    fill(#52F2FF);
       if (mousePressed==true){
       escenario=3;
      sonido2.pause();
          sonido4 = minim.loadFile("we.mp3", 1024);
          sonido4 .loop();
    }
}
else{
  
  fill(255);
}

rect(x,y,w,h,10);
}
void personajemujer(){
  fill(#C497E5);  
  stroke(255);
rect(peX2-40,peY2-50,tam2-20,tam2-20,30);//cuerpo
ellipse(peX2,peY2-100,tam2,tam2);//cabeza
//
fill(0);
arc(peX2,peY2-100,tam2,tam2,PI,TWO_PI);//cabeza
//
  fill(#C497E5);
rect(peX2-30,peY2+90,tam2-65,tam2-50,20);//pierna 
rect(peX2+10,peY2+90,tam2-65,tam2-50,20);//pierna
rect(peX2-30,peY2+130,tam2-65,tam2-50,20);//pantorrilla 

rect(peX2+10,peY2+130,tam2-65,tam2-50,20);//pantorrilaa
rect(peX2-30,peY2+110,tam2-70,tam2-70,30);//rodilla
rect(peX2+10,peY2+110,tam2-70,tam2-70,30);//rodilla
//falda
rect(peX2-40,peY2+30,tam2-15,tam2-70,30);//f1
rect(peX2-40,peY2+40,tam2-15,tam2-70,30);//f2
rect(peX2-42,peY2+50,tam2-5,tam2-70,30);//f3
rect(peX2-44,peY2+50,tam2-5,tam2-70,30);//f4
//
rect(peX2-25,peY2-20,tam2-40,tam2-30,30);//estomago
//
ellipse(peX2-50,peY2,tam2-75,tam2);//mano
ellipse(peX2+50,peY2,tam2-75,tam2);//mano
//
ellipse(peX2-40,peY2-50,tam2-70,tam2-70);//hombro_izquierdo
ellipse(peX2+40,peY2-50,tam2-70,tam2-70);//hombro_derecho
//
rect(peX2-44,peY2+60,tam2+1,tam2-70,30);//f5
rect(peX2-45,peY2+70,tam2+5,tam2-75,30);//f6
rect(peX2-46,peY2+80,tam2+10,tam2-80,30);//f7
//
 rotate(radians(10));
ellipse(peX2-32,peY2-102,tam2-85,tam2-50);//pelo_izquierdo
 rotate(radians(-10));
  rotate(radians(-10));
ellipse(peX2+25,peY2-70,tam2-85,tam2-50);//pelo_izquierdo
 rotate(radians(10));
 rotate(radians(30));
ellipse(peX2-20,peY2-140,tam2-85,tam2-50);//pelo_derecho
 rotate(radians(-30));
  rotate(radians(-30));
ellipse(peX2-10,peY2-40,tam2-85,tam2-50);//pelo_derecho
 rotate(radians(30));
fill(0);
//
ellipse(peX2+45,peY2-120,tam2-85,tam2-85);//chonga_derecho
ellipse(peX2-45,peY2-120,tam2-85,tam2-85);//chonga_izquierda
//
fill(0);
stroke(255);
ellipse(peX2+ojox2,peY2-100,tam2-80,tam2-80);//ojo
ellipse(peX2-ojox2,peY2-100,tam2-80,tam2-80);//ojo
//
fill(255);
ellipse(peX2+ojox2,peY2-100,tam2-90,tam2-90);//ojo
ellipse(peX2-ojox2,peY2-100,tam2-90,tam2-90);//ojo
}
void personajehombre(){

fill(#99ADB4);
rect(peX-50,peY-60,tam,tam+30,20);//cuerpo
ellipse(peX,peY-100,tam,tam);//cabeza
rect(peX-35,peY-40,tam-30,tam-20,30);//estomago

rect(peX-40,peY+80,tam-65,tam-50,20);//pierna 
rect(peX+10,peY+80,tam-65,tam-50,20);//pierna

rect(peX-40,peY+130,tam-65,tam-50,20);//pantorrilla 
rect(peX+10,peY+130,tam-65,tam-50,20);//pantorrilaa
rect(peX-38,peY+110,tam-70,tam-70,30);//rodilla
rect(peX+12,peY+110,tam-70,tam-70,30);//rodilla
rect(peX,peY+60,tam-50,tam-70,30);//pie
rect(peX-50,peY+60,tam-50,tam-70,30);//pie2

ellipse(peX-50,peY,tam-60,tam);//mano
ellipse(peX+50,peY,tam-60,tam);//mano
ellipse(peX-50,peY-50,tam-70,tam-70);//mano
ellipse(peX+50,peY-50,tam-70,tam-70);//mano
fill(0);
ellipse(peX+ojox,peY-100,tam-80,tam-80);//ojo
ellipse(peX-ojox,peY-100,tam-80,tam-80);//ojo
}
void movimiento(){
  background(255);   
  if (mousePressed==true){
    peY2=peY2-5;
    peX2=mouseX/2;
     
  }
      
   }

void anunciodepersonaje(){
  textSize(32);
  int x2=width-600;
   
    x=x+3;
     x2=x2-x;
    println(x);
   if(x==600){
     x=0-300;
   }
   fill(255);
   textAlign(CENTER);
   text(cadena,x,500,300,200);
    text(cadena2,x2,460,300,200);
  
}
void fondo(){
  
  for(int a=0;a<personnages.length;a++){
    personnages[a].dessine(); 
  }
}
class people{
  color c;
  float x,y,taille,vx,vy,vitesse;
  people(float _x, float _y, float maxi){
    float an=random(TWO_PI);
    c = color(random(100, 255),random(100, 255),random(50));
    x = _x; y=_y;taille=random(2,maxi);
    vitesse = ((maxi-taille)/3)+0.5;
    vx = cos(an)*vitesse; vy = sin(an)*vitesse;
    personnages = (people[]) append (personnages, this);
  }
  void dessine(){
  //  fill(c);
    x+=vx;y+=vy;vy+=0.1;
    if(x<taille/2){x=taille/2;vx=-vx;}
    if(y<taille/2){y=taille/2;vy=-vy;}
    if(x>width-taille/2){x=width-taille/2;vx=-vx;}
    if(y>height-taille/2){y=height-taille/2;vy=-vy*0.75;}
    for(int a=0;a<personnages.length;a++){
      people deux = personnages[a];
      if(deux!=this){
        float d=dist(deux.x, deux.y, x , y);
        if(d<((taille+deux.taille)/2)){
          float ang = atan2(y-deux.y, x-deux.x);
          vx = cos(ang)*vitesse;vy=sin(ang)*vitesse;
        } else {
          if(d<30){
            //stroke(255);
            line(x,y,deux.x, deux.y);
             
          }
         
        }
         
      }
    } 
  } 
}

void keyReleased(){
  saveFrame("img#####.png");
}
void botonInicio(){
  int x= width-80;
  int y=height/2;
  int w=width/10;
  int h=height/10;
  
  if ((mouseX>x)&&(mouseX<x+w)&&(mouseY>y)&&(mouseY<y+h)){
    fill(#D6B549);
     if(mousePressed==true){
        sonido1.pause();
        escenario=1;
          sonido2 = minim.loadFile("pista2.mp3", 1024);
          sonido2 .loop();
     }
{

} 

}
else{
  fill(255);
}
rect(x,y,w,h,20);
textSize(15);
fill(0);
text("inicio",x+10,y+35);
}
void mouseReleased() {
  
}
void titulo(){
    fill(255);
    textSize(32);
    println(y);
   if(y==width/2){
   }
   else {
      y=y+10;
   }
   text(titulo,width/6,y,300,80);
}
