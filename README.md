

# PROYECTO DE DIPLOMADO AUTOMATIZACION LLENADO DE UN TINACO 


## CODIGO 

```
// Se declaran las variables a utilizar  
const int Trig_sisterna = 12; //sisterna 
const int Echo_sisterna = 13; //sisterna 
const int Trig_tinaco = 2; //tinaco 
const int Echo_sisterna_tinaco = 15; //tinaco
int ledPin = 14; // Pin del LED
int btn = 23;

void setup() {

// Se define el modo del pin a utilizar 

Serial.begin(9600); //iniciailzamos la comunicación 
pinMode(ledPin, OUTPUT);
pinMode (Trig_sisterna, OUTPUT); //pin como salida
pinMode(Echo_sisterna, INPUT); //pin como entrada 
pinMode(Trig_tinaco, OUTPUT); //pin como salida
pinMode(Echo_sisterna_tinaco, INPUT); //pin como entrada
digitalWrite (Trig_sisterna, LOW);//Inicializamos el pin con 0 
digitalWrite(Trig_tinaco, LOW);//Inicializamos el pin con 0
pinMode(btn, INPUT_PULLUP);

}

void loop() {

long t_sisterna; //timepo que demora en llegar el hecho 
long d_sisterna; //terna/distancia en centimetros

long t_tinaco; //timepo que demora en llegar el hecho
long d_tinaco; //distancia en centimetros

// Tiempo se envio de señal 

//Para el primer sensor 

digitalWrite (Trig_sisterna, HIGH); 
delayMicroseconds(10); //Enviamos un pulso de 10us
digitalWrite (Trig_sisterna, LOW);

//para segundo sensor
digitalWrite(Trig_tinaco, HIGH); 
delayMicroseconds(10); //Enviamos un pulso de 10us
digitalWrite(Trig_tinaco, LOW);

t_sisterna = pulseIn(Echo_sisterna, HIGH); //obtenemos el ancho del pulso 
d_sisterna = t_sisterna/59; //escalamos el tiempo a una distancia en cm
t_tinaco = pulseIn(Echo_sisterna_tinaco, HIGH); //obtenemos el ancho del pulso 
d_tinaco = t_tinaco/59; //escalamos el tiempo a una distancia en cm


// si la sisterna esta llena y el tinaco vacio se prende bomba 
// 0| 0
if (d_sisterna<=200 && d_tinaco<=200){
  digitalWrite (ledPin , 0); 
}

// si la sisterna esta vacia y el tinaco vacio no se prende la bomba
// 0|1
else if (d_sisterna<=200 && d_tinaco>=200){
  digitalWrite(ledPin, 0);  

}

// si la sisterna esta vacio y el tinaco lleno no se prende la bomba
// 1|0
else if (d_sisterna>=200 && d_tinaco<=200){
  digitalWrite(ledPin, 1);  
}

  // si la sisterna esta llena y la tinaco esta lleno no se prende la bomba 
  //1|1
else if (d_sisterna>=200 && d_tinaco>=200){
  digitalWrite(ledPin, 0);

}

  // si se apreta el boton se apaga 
else if (digitalRead(btn) == 0){
  digitalWrite(ledPin, 1);

}

else{
  digitalWrite(ledPin,0);
}


}

```

## CONEXION 
![](https://github.com/AxelMld/proyecto/blob/main/conexion.png?raw=true)