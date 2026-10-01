# M8 - iac

## Vad vi gjorde utanför repot
### Vi började med första att ge application credential till terrafrom via https://pouta.csc.fi. Vi satt namnet som devops-m8 och gav den user som role. Sedan laddade vi ner clouds.yaml filen och satt den under ~/.config/openstack/clouds.yaml. satt också miljövariabeln till namnet på nyckeln under clouds:. 

### I terraform.tfvars ändrade vi templaten variablerna till våra egna och sedan efter validera utan molnaccess.

### Vid steg 4 så började vi svetta. Här gjorde vi init, plan och sedan apply. Vi fick problem vid plan för att jag gjorde export OS_CLOUD=openstack i en annan codespace som sedan vi fixade med hjälpa av den smarta Tobias. 

### Vi testade också att floating ip addresen med curl och vi fick fina svar. 

### Vi rev ner gamla M7 som vi gjorde manuellt först genom pouta.csc och sedan lokalt med terrafrom destroy

## Skärmdumpar

### Steg 4 – [CLOUD] init, plan, apply mot cPouta
![slutet av apply-utskriften med Apply complete! Resources: 8 added och outputs-blocket](./m8-screenshots/screenshot1.png)

### Steg 5 – Verifiera utifrån
![Kom ihåg skärmdump till inlämningen: webbläsaren med notes-appen på M8:s nip.io-URL (adressfältet synligt) — jämför adressen med app_url_nip_io i outputs](./m8-screenshots/screenshot2.png)

### Steg 6 – [KONSOL] Riv M7-VM:en och släpp dess floating IP
![Instances-listan i Horizon med bara M8-instansen m8-Albin kvar, medan M7-instansen är borttagen](./m8-screenshots/screenshot3.png)

### Steg 6 – [KONSOL] Riv M7-VM:en och släpp dess floating IP
![Floating IPs-listan i Horizon med bara M8:s floating IP kvar efter att M7:s adress har släppts](./m8-screenshots/screenshot4.png)

### Steg 7 – Läs state, rör den aldrig för hand
![Terminalen med terraform state list som visar Terraform-resurserna och data-raden för nätverket](./m8-screenshots/screenshot5.png)

### Steg 8 – [CLOUD] Rebuild-demot: riv VM:en, behåll IP:n
![Terminalen med floating_ip-outputen före rebuild-demot](./m8-screenshots/screenshot6.png)

### Steg 8 – [CLOUD] Rebuild-demot: riv VM:en, behåll IP:n
![Terminalen med floating_ip-outputen efter rebuild-demot, med samma adress som före](./m8-screenshots/screenshot7.png)
