# M7 - Cloud VM

## Vad vi gjorde utanför repot
### I den här milstolpen byggde vi manuellt upp en Ubuntu 24.04 VM i CSC cPouta via Horizon-konsolen. Vi skapade egna SSH-nyckelpar och konfigurerade en security group som tillåter SSH på port 22 och applikationstrafik på port 8080.

### Vi startade VM:en med standard.small och kopplade en floating IP till den så att den kunde nås utifrån. Vi konfigurerade SSH så att båda i paret kunde logga in med sina egna nycklar.

### På VM:en installerade vi Docker och Docker Compose. Därefter skapade vi en docker-compose.yml som kör backend- och frontend-images från GHCR. Frontend-containern exponerades på port 8080.

### Vi verifierade först att applikationen fungerade från VM:en genom att anropa /api/health, vilket returnerade {"status":"ok"}. Därefter testade vi samma endpoint från vår codespace via floating IP:n för att bekräfta att applikationen var åtkomlig utifrån.

### Till sist använde vi en nip.io-adress för floating IP:n och öppnade den i webbläsaren. Notes-applikationen visades och allt funkade.

## Skärmdumpar

### Steg 2 – Publika nycklar uppladdade i cPouta
![Key Pairs-listan i Horizon med Albin-key och Alexander-key, typ ssh och fingerprint](./m7-screenshots/screenshot3.png)

### Steg 3 – Security group med SSH och port 8080 öppna
![Regellistan i security groupen med Ingress TCP 22 och Ingress TCP 8080 från 0.0.0.0/0](./m7-screenshots/screenshot2.png)

### Steg 4–5 – VM:en kör med floating IP
![Instansen m7-Albin i Compute → Instances: Ubuntu-24.04, standard.small, Active, Running, intern IP 192.168.1.205 och floating IP 86.50.20.81](./m7-screenshots/screenshot1.png)

### Steg 6 och 8 – SSH in och installera Docker
![SSH-session mot 86.50.20.81 där get.docker.com-skriptet installerar Docker](./m7-screenshots/screenshot4.png)

### Steg 8 – Docker och Docker Compose verifierade
![Utskrift av sudo docker --version och sudo docker compose version på VM:en](./m7-screenshots/screenshot5.png)

### Steg 9 – docker-compose.yml skapad på VM:en
![docker-compose.yml skrivs till /opt/app med backend- och frontend-images från GHCR](./m7-screenshots/screenshot6.png)

### Steg 9 – Images pullade och stacken startad
![docker compose pull och up -d: båda images pullade, nätverk och båda containrarna startade](./m7-screenshots/screenshot7.png)

### Steg 10 – Verifiering på VM:en
![docker compose ps visar backend och frontend som Up, och curl mot localhost:8080/api/health svarar status ok](./m7-screenshots/screenshot8.png)

### Steg 10 – Appen svarar utifrån via floating IP och nip.io
![Från codespacen, efter exit ur SSH: curl mot 86.50.20.81:8080 och 86-50-20-81.nip.io:8080 svarar båda status ok](./m7-screenshots/screenshot9.png)

### Steg 10 – Notes-appen i webbläsaren
![Webbläsaren på 86-50-20-81.nip.io:8080 visar notes-appen "Albins och Alexanders anteckningar"](./m7-screenshots/screenshot10.png)
