#include <WiFi.h>

const char* ssid = "YOUR_WIFI_NAME";
const char* password = "YOUR_WIFI_PASSWORD";

WiFiServer server(80);

const int bulbRelay = 18;   // D18
const int fanRelay  = 4;    // D4

void setup() {
  Serial.begin(115200);

  pinMode(bulbRelay, OUTPUT);
  pinMode(fanRelay, OUTPUT);

  digitalWrite(bulbRelay, HIGH);
  digitalWrite(fanRelay, HIGH);

  WiFi.begin(ssid, password);

  Serial.print("Connecting to WiFi");

  while (WiFi.status() != WL_CONNECTED) {
    delay(500);
    Serial.print(".");
  }

  Serial.println("\nWiFi Connected");
  Serial.print("IP Address: ");
  Serial.println(WiFi.localIP());

  server.begin();
}

void loop() {

  WiFiClient client = server.available();

  if (client) {

    String request = client.readStringUntil('\r');
    client.flush();

    if (request.indexOf("/BULB_ON") != -1)
      digitalWrite(bulbRelay, LOW);

    if (request.indexOf("/BULB_OFF") != -1)
      digitalWrite(bulbRelay, HIGH);

    if (request.indexOf("/FAN_ON") != -1)
      digitalWrite(fanRelay, LOW);

    if (request.indexOf("/FAN_OFF") != -1)
      digitalWrite(fanRelay, HIGH);

    client.println("HTTP/1.1 200 OK");
    client.println("Content-Type: text/html");
    client.println();

    client.println("<html><head><title>Smart Home</title></head>");
    client.println("<body style='font-family:Arial;text-align:center;'>");

    client.println("<h1>IoT Smart Home Automation</h1>");

    client.println("<h2>Bulb Control</h2>");
    client.println("<a href='/BULB_ON'><button style='padding:15px;'>ON</button></a>");
    client.println("<a href='/BULB_OFF'><button style='padding:15px;'>OFF</button></a>");

    client.println("<h2>Fan Control</h2>");
    client.println("<a href='/FAN_ON'><button style='padding:15px;'>ON</button></a>");
    client.println("<a href='/FAN_OFF'><button style='padding:15px;'>OFF</button></a>");

    client.println("</body></html>");

    delay(1);
    client.stop();
  }
}
