# Design och Implementering av en Säker IoT-lösning (Proof of Concept)

## Scenario
Ni är anställda som IoT-utvecklare på ett tech house och har blivit tilldelade att utveckla ett Proof of Concept (PoC) för en potentiell kund. Kunden är intresserad av att se hur en säker och robust IoT-lösning kan hjälpa dem att övervaka och hantera sina enheter på distans.

Uppgiften är att ta fram en fungerande prototyp som demonstrerar säker kommunikation, en fungerande infrastruktur och som tar hänsyn till de grundläggande kraven från **Cyber Resilience Act (CRA)**.

## Mål
Utveckla och presentera en Proof of Concept (PoC) för en säker IoT-lösning som potentiellt kan gå vidare till produktionsfas. PoC ska inkludera:
- En **ESP32-baserad sensorlösning** som kommunicerar via **WiFi**.
- Anslutning till **ThingSpeak** för datainsamling och visualisering.
- Implementering av **säker kommunikation** (HTTPS).
- Systemspecifikation och analys av hur lösningen uppfyller **CRA-kraven**.

---

## Lösningsbeskrivning

### 1. Arkitektur

- **Sensorenhet (ESP32):** ESP32 är ansluten till sensorer som mäter olika parametrar (t.ex. temperatur, luftfuktighet) och använder **WiFi** för att ansluta till internet.
  
- **Molnplattform (ThingSpeak):** ESP32-enheten kommunicerar med **ThingSpeak** för att skicka sensordata. **ThingSpeak** används för att lagra, analysera och visualisera data. 

- **Kommunikation:** Data skickas från ESP32 till ThingSpeak via **HTTPS** för att säkerställa säker dataöverföring.

- **Användargränssnitt:** ThingSpeak erbjuder inbyggda visualiseringsverktyg som kan användas för att skapa grafer och visa data i realtid.

#### Arkitekturdiagram:

**ESP32 (WiFi) → ThingSpeak (HTTPS) → ThingSpeak Dashboard (Visualisering)**


---

### 2. Kommunikationsflöde

1. **ESP32** samlar in data från sensorer.
2. ESP32-enheten ansluter till **WiFi** och skickar data via **HTTPS** (krypterad HTTP) till ThingSpeak.
3. ThingSpeak lagrar data och visualiserar det i realtid via sin webbaserade dashboard.

---

### 3. Säkerhetsåtgärder

- **Kryptering av dataöverföring:** Kommunikation mellan ESP32 och ThingSpeak sker via **HTTPS**, vilket innebär att datan krypteras med **TLS (Transport Layer Security)**. Detta skyddar datan från obehörig åtkomst under överföring.

- **WiFi-säkerhet:** Använd **WPA2** eller bättre för att säkra WiFi-anslutningen mellan ESP32 och nätverket.

- **API-autentisering:** Varje ESP32-enhet autentiseras mot ThingSpeak med en unik **API-nyckel**. Denna nyckel måste användas vid varje dataöverföring till ThingSpeak, vilket förhindrar obehöriga enheter från att skicka data till plattformen.

---

### 4. Systemspecifikation och CRA-krav

#### Arkitekturkomponenter
- **ESP32 Sensorenhet:** Samlar in data och ansluter via WiFi till ThingSpeak.
- **ThingSpeak (Molnplattform):** Samlar in, lagrar och visualiserar data.
- **WiFi-kommunikation:** ESP32 använder WiFi och HTTPS för att skicka data till ThingSpeak.

#### Säkerhetsfunktioner
- **HTTPS/TLS:** All kommunikation mellan ESP32 och ThingSpeak sker via krypterade HTTPS-förfrågningar, vilket skyddar dataintegriteten.
- **WiFi-säkerhet:** Användning av stark WiFi-säkerhet (WPA2 eller bättre) för att förhindra obehörig åtkomst till det lokala nätverket.
- **API-nyckel-autentisering:** ThingSpeak använder API-nycklar för att autentisera enheterna.

---

## Anpassning till **Cyber Resilience Act (CRA)**

### 1. Security-by-design
- **Inbyggd kryptering:** ESP32 kommunicerar med ThingSpeak via **HTTPS**, vilket skyddar data i transit.
- **Autentisering:** Varje enhet använder en unik API-nyckel för att autentisera sig mot ThingSpeak.
- **WiFi-säkerhet:** Säker WiFi-kommunikation genom WPA2- eller WPA3-kryptering.

### 2. Uppdaterbarhet
- **OTA (Over-the-Air) uppdateringar:** ESP32 kan stödja OTA-uppdateringar, vilket möjliggör fjärruppdateringar av firmware. Detta är avgörande för att kunna åtgärda sårbarheter eller implementera nya säkerhetsfunktioner.
- Uppdateringsmekanismen kan utvecklas vidare genom användning av ESP32:s inbyggda OTA-bibliotek eller integrering med tredjepartstjänster för fjärruppdateringar.

### 3. Sårbarhetshantering
- **Loggning och övervakning:** ThingSpeak kan övervaka och logga data från enheterna, vilket gör det möjligt att upptäcka avvikelser i systemet. Regelbunden granskning av dessa loggar kan hjälpa till att identifiera potentiella sårbarheter.
- **Incidenthantering:** Vid identifierade säkerhetshot kan OTA-uppdateringar snabbt distribueras för att åtgärda sårbarheter i systemet.

---

## Recap

Denna PoC för en IoT-lösning med **ESP32**, **WiFi**, och **ThingSpeak** uppfyller både de tekniska och säkerhetsmässiga kraven för att demonstrera hur kunden kan övervaka och hantera sina enheter på distans. Genom att använda **HTTPS** för säker dataöverföring och **API-nycklar** för autentisering uppfyller du viktiga säkerhetskrav. Lösningen kan vidareutvecklas för att inkludera **OTA-uppdateringar** och sårbarhetshantering, vilket gör den redo för produktionsfas och kompatibel med **Cyber Resilience Act**.

Denna implementation visar på god säkerhet-by-design och möjligheten att hantera framtida säkerhetsuppdateringar och sårbarheter, vilket är avgörande för långsiktig IoT-drift.

