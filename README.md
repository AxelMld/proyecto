

# PROYECTO DE DIPLOMADO AUTOMATIZACION LLENADO DE UN TINACO 


## CODIGO 

```
//librerias a internet
#include <ArduinoJson.h>
#include <WiFi.h>
#include <PubSubClient.h>


// Se declaran las variables a utilizar
const int Trig_sisterna = 12; //sisterna
const int Echo_sisterna = 13; //sisterna
const int Trig_tinaco = 2; //tinaco
const int Echo_sisterna_tinaco = 15; //tinaco
int ledPin = 14; // Pin del LED

//conexion a nuestro wifi

const char* ssid = "Wokwi-GUEST";
const char* password = "";
const char* mqtt_server = "44.195.202.69";
String username_mqtt = "Axel_Michelle_jav";
String password_mqtt = "1234";

WiFiClient espClient;
PubSubClient client(espClient);
unsigned long lastMsg = 0;
#define MSG_BUFFER_SIZE  (50)
char msg[MSG_BUFFER_SIZE];
int value = 0;

void setup_wifi() {

  delay(10);
  // We start by connecting to a WiFi network
  Serial.println();
  Serial.print("Conectando  ");
  Serial.println(ssid);

  WiFi.mode(WIFI_STA);
  WiFi.begin(ssid, password);

  while (WiFi.status() != WL_CONNECTED) {
    delay(500);
    Serial.print(".");
  }

  randomSeed(micros());

  Serial.println("");
  Serial.println("WiFi conectado");
  Serial.println("IP address: ");
  Serial.println(WiFi.localIP());
}

void callback(char* topic, byte* payload, unsigned int length) {
  Serial.print("Message arrived [");
  Serial.print(topic);
  Serial.print("] ");
  for (int i = 0; i < length; i++) {
    Serial.print((char)payload[i]);
  }
  Serial.println();

  // Switch on the LED if an 1 was received as first character
  if ((char)payload[0] == '1') {
    digitalWrite(BUILTIN_LED, LOW);
    // Turn the LED on (Note that LOW is the voltage level
    // but actually the LED is on; this is because
    // it is active low on the ESP-01)
  } else {
    digitalWrite(BUILTIN_LED, HIGH);
    // Turn the LED off by making the voltage HIGH
  }

}

void reconnect() {
  // Loop until we're reconnected
  while (!client.connected()) {
    Serial.print("Attempting MQTT connection...");
    // Create a random client ID
    String clientId = "ESP8266Client-";
    clientId += String(random(0xffff), HEX);
    // Attempt to connect
    if (client.connect(clientId.c_str(), username_mqtt.c_str() , password_mqtt.c_str())) {
      Serial.println("conectado");
      // Once connected, publish an announcement...
      client.publish("outTopic", "Hola mundo");
      // ... and resubscribe
      client.subscribe("inTopic");
    } else {
      Serial.print("failed, rc=");
      Serial.print(client.state());
      Serial.println(" try again in 5 seconds");
      // Wait 5 seconds before retrying
      delay(5000);
    }
  }
}


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
  d_sisterna = t_sisterna / 59; //escalamos el tiempo a una distancia en cm
  t_tinaco = pulseIn(Echo_sisterna_tinaco, HIGH); //obtenemos el ancho del pulso
  d_tinaco = t_tinaco / 59; //escalamos el tiempo a una distancia en cm


  // si la sisterna esta vacia y el tinaco vacio no se prende la bomba
  // 0| 0 =0
  if (d_sisterna <= 200 && d_tinaco <= 200) {
    digitalWrite (ledPin , 0);
  }

  // si la sisterna esta vacia y el tinaco esta lleno no se prende la bomba
  // 0|1 = 0
  else if (d_sisterna <= 200 && d_tinaco >= 200) {
    digitalWrite(ledPin, 0);
  }

  // si la sisterna esta llena y el tinaco vacio se prende la bomba
  // 1|0 = 1
  else if (d_sisterna >= 200 && d_tinaco <= 200) {
    digitalWrite(ledPin, 1);
  }

  // si la sisterna esta llena y el tinaco lleno se prende la bomba
  // 1|1 = 0
  else if (d_sisterna >= 200 && d_tinaco >= 200) {
    digitalWrite(ledPin, 1);
  }


}

```

## CONEXION 
![](https://github.com/AxelMld/proyecto/blob/main/conex%202.png?raw=true)