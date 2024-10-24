# Design och Implementering av en Säker IoT-lösning (Proof of Concept)

## Scenario
Ni är anställda som IoT-utvecklare på ett tech house och har blivit tilldelade att utveckla ett Proof of Concept (PoC) för en potentiell kund. Kunden är intresserad av att se hur en säker och robust IoT-lösning kan hjälpa dem att övervaka och hantera sina enheter på distans.

Uppgiften är att ta fram en fungerande prototyp som demonstrerar säker kommunikation, en fungerande infrastruktur och som tar hänsyn till de grundläggande kraven från **Cyber Resilience Act (CRA)**.

## Mål
Utveckla och presentera en Proof of Concept (PoC) för en säker IoT-lösning som potentiellt kan gå vidare till produktionsfas. PoC ska inkludera:

- En **ESP32-baserad sensorlösning** som kommunicerar via **WiFi**.
- Anslutning till **ThingSpeak** för datainsamling och visualisering.
- Implementering av **säker kommunikation** (via **MQTT med TLS**).
- Systemspecifikation och analys av hur lösningen uppfyller **CRA-kraven**.

---

## Lösningsbeskrivning

### 1. Arkitektur
- **Sensorenhet (ESP32):** ESP32 är ansluten till sensor som mäter olika parametrar (Temperatur, luftfuktighet) och använder **WiFi** för att ansluta till internet.
  
- **Molnplattform (ThingSpeak):** ESP32-enheten kommunicerar med **ThingSpeak** via **MQTT** för att skicka sensordata. **ThingSpeak** används för att lagra, analysera och visualisera data.

- **Kommunikation:** Data skickas från ESP32 till ThingSpeak via **MQTT över TLS (Transport Layer Security)** för att säkerställa krypterad och säker dataöverföring.

- **Användargränssnitt:** ThingSpeak erbjuder inbyggda visualiseringsverktyg som kan användas för att skapa grafer och visa data i realtid.

#### Arkitekturdiagram:
**ESP32 (WiFi) → MQTT Broker (ThingSpeak MQTT över TLS) → ThingSpeak Dashboard (Visualisering)**

---

### 2. Kommunikationsflöde
1. **ESP32** samlar in data från sensorer.
2. ESP32-enheten ansluter till **WiFi** och kommunicerar via **MQTT** med **ThingSpeak** över en **TLS-krypterad anslutning**.
3. **ThingSpeak** tar emot, lagrar och visualiserar data i realtid via sin webbaserade dashboard.

---

### 3. Säkerhetsåtgärder
- **Kryptering av dataöverföring:** Kommunikation mellan ESP32 och ThingSpeak sker via **MQTT över TLS**, vilket innebär att data krypteras under överföring för att skydda mot avlyssning och man-in-the-middle-attacker.
  
- **WiFi-säkerhet:** Använd **WPA2** eller bättre för att säkra WiFi-anslutningen mellan ESP32 och nätverket.

- **MQTT-autentisering:** Varje ESP32-enhet autentiseras mot ThingSpeak via **MQTT** med en specifik **API-nyckel** och autentisering sker med **MQTT-användarnamn och lösenord**. Detta förhindrar obehöriga enheter från att skicka data till plattformen.

---

### 4. Systemspecifikation och CRA-krav

#### Arkitekturkomponenter
- **ESP32 Sensorenhet:** Samlar in data och ansluter via WiFi till ThingSpeak via MQTT.
- **ThingSpeak (Molnplattform):** Samlar in, lagrar och visualiserar data.
- **WiFi-kommunikation:** ESP32 använder WiFi och MQTT över TLS för att skicka data till ThingSpeak.

#### Säkerhetsfunktioner
- **MQTT över TLS:** All kommunikation mellan ESP32 och ThingSpeak sker via krypterade MQTT-förfrågningar, vilket skyddar dataintegriteten.
- **WiFi-säkerhet:** Användning av stark WiFi-säkerhet (WPA2 eller bättre) för att förhindra obehörig åtkomst till det lokala nätverket.
- **API-nyckel-autentisering:** ThingSpeak använder API-nycklar och MQTT-lösenord för att autentisera enheterna.

