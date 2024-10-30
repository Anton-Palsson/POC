# **Design och Implementering av en Säker IoT-lösning (Proof of Concept)**

## **Projektöversikt**
Som IoT-utvecklare på tech house har jag utvecklat en Proof of Concept (PoC) för en säker IoT-lösning åt en potentiell kund. Lösningen demonstrerar hur säker kommunikation kan användas för att fjärrövervaka enheter och visar upp en fungerande infrastruktur som uppfyller grundläggande säkerhetskrav enligt Cyber Resilience Act (CRA).

## **Mål**
Utveckla och demonstrera en säker IoT-lösning som innehåller:
- En fungerande sensorprototyp
- Säker dataöverföring med hjälp av TLS-kryptering och HTTPS
- Grundläggande infrastruktur som kan utvidgas för att möta CRA:s säkerhetskrav i en produktionsmiljö

## **Lösningens Komponenter**

### **1. IoT-Prototyp**
Denna PoC använder en fysisk ESP32 för att mäta temperatur och luftfuktighet. Sensorvärdena skickas via MQTT-protokollet till en broker (HiveMQ), som sedan hanteras i Node-RED. Data bearbetas för att skapa en struktur som skickas vidare till InfluxDB för lagring och visualisering i Grafana.

### **2. Kommunikationsflöde**
Dataflödet i systemet fungerar enligt följande:
1. **Sensorenhet**: En fysisk ESP32 med dht sensor samlar in temperatur- och luftfuktighetsdata.
![IMG_6618](https://github.com/user-attachments/assets/3c5ff286-71de-48d4-a967-012e8267a04c)

2. **MQTT Broker**: Sensorvärden publiceras på MQTT, och brokern säkerställer dataöverföring till Node-RED.
 ![HIVEMQ CLOUD](https://github.com/user-attachments/assets/9305adfe-6501-4a2e-8962-9dc65017631e)

3. **Node-RED**: Bearbetar och strukturerar data innan den skickas vidare till InfluxDB.
   ![NODERED WORKFLOW](https://github.com/user-attachments/assets/f5dc8171-2522-4605-9d9a-3316bece9003)


4. **InfluxDB**: Tar emot och lagrar data i en tidsserie, vilket möjliggör visualisering och analys.
   ![DATA IN INFLUXDB](https://github.com/user-attachments/assets/a1e96885-1ab9-40d8-8008-bafd0e5f95ff)

5. **Grafana**: Används för att skapa dashboards som visualiserar data i realtid.
![GRAFANA GRAPH](https://github.com/user-attachments/assets/6b94b749-d355-4dfb-a5eb-b1ab47716897)

### **3. Säker Kommunikationsväg**
För att skydda datan används TLS för att säkra kommunikationen mellan MQTT-broker och klienter. HTTPS används också som ett säkerhetslager i all kommunikation mellan Node-RED, InfluxDB och Grafana. Genom användningen av HTTPS, förväntas en säker anslutning mellan de komponenterna, vilket garanterar datans integritet och sekretess. Den fysiska devicens design bör ses över när det gäller fysisk manipulering. 

### **Användning av Tokens**
För att hantera autentisering och accesskontroll på ett säkert sätt används tokens för både InfluxDB och Grafana. Detta gör det möjligt att begränsa och säkra tillgången till olika tjänster samt logga och övervaka användningen. Med tokens säkerställs att bara auktoriserade enheter och användare får åtkomst, vilket är centralt för att följa CRA:s krav på säkerhet och åtkomstkontroll.

## **Framtida Utveckling: Direkta Integrationsmöjligheter**

### **Direktkoppling mellan HiveMQ och InfluxDB**
I framtiden skulle en direkt integration mellan HiveMQ och InfluxDB kunna övervägas för att minska beroendet av Node-RED, vilket kan minska antalet systemkomponenter och därmed komplexiteten. Denna lösning kan dock innebära extra kostnader, då direkta integrationslösningar kräver specifik licensiering eller teknisk support, beroende på valda tjänster.

## **Systemspecifikation och Anpassning till CRA**

### **Arkitektur**
- **Sensorenhet**: En fysisk ESP32 med inkopplad sensor skickar data via MQTT, där temperatur och luftfuktighet mäts och skickas periodiskt.
- **Gateway och Broker**: En MQTT-broker genom cloud fungerar som mellanstation och säkerställer tillförlitlig dataöverföring.
- **Datahantering**: Node-RED bearbetar och strukturerar datan innan lagring.
- **Lagring och Visualisering**: InfluxDB lagrar data som en tidsserie, medan Grafana möjliggör visualisering av denna data.

### **Säkerhetsåtgärder**
1. **Datakryptering**: TLS används för att kryptera all dataöverföring, vilket skyddar data i transit. HTTPS ger dessutom ett extra skyddslager i kommunikationerna.
2. **Autentisering**: Vid användning av mTLS skulle både klient och broker verifiera varandra för ökad säkerhet, även om detta är en framtida förbättring i PoC.
3. **Accesskontroll**: Rollbaserad access kan läggas till för att begränsa åtkomst på servernivå i framtida versioner.

### **CRA-krav och Hur Lösningen Uppfyller Dem**
1. **Security-by-Design**: Säkerhet har byggts in i designen genom att använda TLS för säker kommunikation och förbereda för rollbaserad accesskontroll.
2. **Uppdaterbarhet**: Framtida versioner kan stödja OTA-uppdateringar för att säkerställa att sårbarheter kan patchas.
3. **Sårbarhetshantering**: Genom loggning och övervakning i Grafana kan framtida intrång eller avvikande aktiviteter upptäckas och hanteras.

## **Skalbarhet och Utmaningar för Storskaliga Implementationer**

Om projektet skalas upp till att omfatta hundratals eller tusentals enheter kan systemet behöva optimeras för att hantera de stora datamängderna och säkerställa att kommunikationen förblir säker och snabb. Här är några överväganden vid storskalig implementering:

### **Fördelar**
- **Centraliserad kontroll och insamling**: Med en central MQTT-broker och InfluxDB kan all data enkelt samlas och analyseras, vilket ger en tydlig överblick över hela systemet.
- **Anpassningsbarhet genom Node-RED**: Node-RED möjliggör flexibel uppbyggnad av dataflöden och snabb integration med andra system, vilket gör att nya funktioner enkelt kan läggas till vid behov.
- **Skalbar visualisering**: Grafana kan enkelt anpassas för att visualisera stora mängder data i realtid, vilket gör den väl lämpad för övervakning i större system.
- **Modulär arkitektur**: De tydligt separerade komponenterna (HiveMQ, Node-RED, InfluxDB och Grafana) gör det enkelt att förbättra eller byta ut enskilda delar av systemet.
- **Säker kommunikation med HTTPS och TLS**: Användning av HTTPS och TLS skyddar all dataöverföring, vilket säkerställer att systemet möter aktuella säkerhetskrav.

### **Nackdelar**
- **Ökade infrastrukturkostnader**: Vid storskalig användning kan kostnaderna för molntjänster och lagring öka snabbt. En direktintegration mellan HiveMQ och InfluxDB skulle kunna minska kostnaderna på längre sikt, men kräver licensierade tjänster eller specifik teknisk kompetens.
  
- **Komplex accesskontroll**: Vid många anslutna enheter krävs skalbara mekanismer för autentisering och åtkomstkontroll för att upprätthålla säkerheten. Detta innebär ökad komplexitet och underhållsbehov för att säkerställa att endast behöriga enheter kan ansluta till systemet.

- **Utmaningar med säkerhetsuppdateringar**: Med många enheter är det avgörande att kunna rulla ut säkerhetsuppdateringar effektivt och säkert. En robust lösning för OTA-uppdateringar och rollback krävs för att undvika driftsavbrott och säkerhetsproblem, särskilt vid storskalig drift.

- **Node-RED som potentiell flaskhals**: Node-RED fungerar som ett mellanlager för bearbetning av data mellan MQTT och InfluxDB. Vid hög belastning kan Node-RED bli en flaskhals, vilket kräver optimering och eventuellt fler resursallokeringar för att hantera stora datamängder.

- **Prestanda vid stor skala**: Att hantera tusentals enheter som skickar data kontinuerligt innebär högre krav på systemets prestanda och stabilitet. Detta kan kräva dedikerade servrar eller extra resurser för att bibehålla driftkvaliteten.

- **Direktkoppling till InfluxDB**: Att direkt ansluta HiveMQ till InfluxDB är ett alternativ som kan minska beroendet av Node-RED och förenkla arkitekturen. Dock krävs en stark säkerhetsstrategi för att säkerställa datakvalitet och skydda datan, vilket kan innebära extra kostnader och tekniska utmaningar.


---

## **Sammanfattning**
Denna PoC demonstrerar en säker IoT-lösning som möter kundens behov. Med denna grund kan systemet enkelt utvidgas till en produktionsfärdig lösning som efterlever CRA och inkluderar ytterligare säkerhetsfunktioner och skalbara integrationslösningar. HTTPS används i hela systemet för säker kommunikation mellan tjänster, och autentisering med tokens tillämpas för både Grafana och InfluxDB för att förstärka säkerheten ytterligare. I projekt mappen hittar man Screenshot på fungerande prototyp samt koden till esp32:an 

