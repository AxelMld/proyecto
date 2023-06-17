
#define I2C_ADDR    0x27

const int Trigger = 12;  //sisterna
const int Echo = 13;   //sisterna 
const int Trigger2 = 2;   //tinaco
const int Echo2 = 15;    //tinaco 
int ledPin = 14; // Pin del LED



void setup() {

  Serial.begin(9600);//iniciailzamos la comunicación
  pinMode(ledPin, OUTPUT);
  pinMode(Trigger, OUTPUT); //pin como salida
  pinMode(Echo, INPUT);  //pin como entrada
  pinMode(Trigger2, OUTPUT); //pin como salida
  pinMode(Echo2, INPUT);  //pin como entrada
  digitalWrite(Trigger, LOW);//Inicializamos el pin con 0
  digitalWrite(Trigger2, LOW);//Inicializamos el pin con 0

}

void loop()
{

  long tsis; //timepo que demora en llegar el heco
  long dsis; //distancia en centimetros

  long t2; //timepo que demora en llegar el heco
  long d2; //distancia en centimetros



  digitalWrite(Trigger, HIGH);
  delayMicroseconds(10);          //Enviamos un pulso de 10us
  digitalWrite(Trigger, LOW);

  //para segundo sensor 

  digitalWrite(Trigger2, HIGH);
  delayMicroseconds(10);          //Enviamos un pulso de 10us
  digitalWrite(Trigger2, LOW);
  
  tsis = pulseIn(Echo, HIGH); //obtenemos el ancho del pulso
  dsis = tsis/59;             //escalamos el tiempo a una distancia en cm

  t2 = pulseIn(Echo2, HIGH); //obtenemos el ancho del pulso
  d2 = t2/59;             //escalamos el tiempo a una distancia en cm
  
 if (dsis > 200 && d2 <100){
        digitalWrite (ledPin , HIGH);     //Si el sensor detecta una distancia menor a 20 cm enciende el LED Rojo
      
    }

  else if ()


}


