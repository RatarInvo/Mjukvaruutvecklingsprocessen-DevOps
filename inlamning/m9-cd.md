# M9 - cd

## Vad vi gjorde utanför repot
### Vi gjorde en ny ssh deploy nyckel och en depoly användare sedan lade vi till ssh-nyckel, adressen och host key på github.

### Nästa steg tittade vi att deploy.yaml fungerade med actionlint. Vi gjorde också en liten ändring i själv frontenden för att trigga en ny deploy av VM

### Lät Publish images bygga/pusha Docker-images till GHCR.

## Skärmdumpar

### Steg 3 – Deploy-användare på VM:en
![Terminalen på VM:en där `id deploy` visar att deploy-användaren finns och tillhör docker-gruppen](./m9-screenshots/screenshot1.png)

### Steg 6 – GitHub Actions-variabler
![GitHub-fliken Variables med DEPLOY_HOST, DEPLOY_KNOWN_HOSTS och DEPLOY_USER](./m9-screenshots/screenshot2.png)

### Steg 6 – GitHub Actions-secret
![GitHub-fliken Secrets med repository-secreten DEPLOY_SSH_KEY](./m9-screenshots/screenshot3.png)

### Steg 9 – Actions-kedjan
![GitHub Actions med gröna körningar för Publish images och Deploy to VM i rätt ordning](./m9-screenshots/screenshot4.png)

### Steg 10 – Verifiering utifrån
![Notes-appen på nip.io-adressen med adressfältet synligt efter deployen](./m9-screenshots/screenshot5.png)
