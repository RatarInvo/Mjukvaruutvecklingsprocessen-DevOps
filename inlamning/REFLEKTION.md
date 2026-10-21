# Albin Berglund:

## Produktens livscykel

Under hela kursen hade vår tjänst över tid ändrat till en applikation som kan svara i produktion med en automatiserad kedja. I början, vid M1, handlade arbetet mest om att få igång repot och lägga in den första koden. Vi jobbade mycket manuellt och behövde själva hålla koll på commits med partnern, branches och vad som skulle ändras. Det var ibland ganska irriterande att alltid approva alla requests men det är där för säkerhet som vi implementerade i M2, där vi lade till branch protection och pull requests med review. Det gjorde att ändringar till `main` inte längre kunde göras hur som helst, utan behövde gå igenom en tydligare process.

I M3 paketerade vi applikationen med Docker. Backend och frontend fick egna images och vi använde Docker Compose för att köra båda delarna tillsammans. Där lärde vi oss bland annat hur ordningen på `COPY` och `RUN` påverkar cachen och varför frontendens nginx behöver prata med tjänsten som heter `backend`. Det gjorde utvecklingsmiljön mer lik en riktig miljö och det blev lättare att köra samma applikation på olika ställen.

M4 blev viktig eftersom vi hittade en riktig bugg. När en anteckning togs bort minskade inte antalet characters. De gamla testerna var gröna eftersom de bara testade att lägga till anteckningar. När vi skrev ett nytt test som skapade två anteckningar, tog bort en och sedan kontrollerade summan, blev testet rött. Efter fixen körde vi testerna igen och verifierade även applikationen i Docker.

I M5 kopplade vi tester och linting till GitHub Actions. Ruff och backend-tester kördes efter push och en medvetet felaktig ändring stoppades av CI. I M6 började GitHub bygga och publicera Docker-images till GHCR. I M7 gjorde vi först mycket manuellt: vi skapade en VM i cPouta, satte upp security groups, SSH, Docker och startade containrarna. Denna milstopen var ganska svår men grund av att instruktionerna var tydliga så kom vi genom denna del utan mycket problem. I M8 ersatte vi detta med Terraform. Med `init`, `plan` och `apply` kunde infrastrukturen beskrivas i kod och återskapas på ett mer kontrollerat sätt. Till sist, i M9, kopplade vi ihop allt med CD. GitHub Actions publicerar images och deployar sedan till VM:en via SSH. En ändring i repot kan alltså nu leda hela vägen till en uppdaterad applikation i produktion.

## Vad går sönder utan automation?

Vart vi märkte mest använding av automation var när vi kom till testsviten och CI-pipelinen. Då vi hittade buggen i M4 hade vi redan gröna tester, men det betydde inte att all funktion fungerade. Testerna saknade just situationen där en anteckning tas bort. När vi skrev det testet fick vi direkt ett konkret bevis på problemet. Utan en automatiserad testkörning hade vi behövt komma ihåg att köra alla tester manuellt efter varje ändring. Det är lätt att glömma, särskilt när man samtidigt ändrar backend, bygger Docker-images och startar om applikationen.

CI skyddar också repot från att en dålig ändring mergas. Vi testade detta genom att lägga in en import som inte användes och då reagerade Ruff. Om lint och tester hade körts manuellt hade någon behövt komma ihåg stegen varje gång. Det hade kunnat göra att en felaktig pull request blev godkänd bara för att man råkade missa en kontroll.

Samma sak gäller infrastrukturen. I M7 tog det många manuella steg att skapa VM, nätverk, floating IP och installera Docker. I M8 kunde Terraform göra samma typ av arbete från kod. När vi dessutom byggde om VM:en och behöll floating IP såg vi att infrastrukturen blev mer förutsägbar. Utan IaC hade det varit större risk att vi glömde någon inställning eller skapade en miljö som inte var exakt likadan som den förra.

CD-steget i M9 minskar också risken för fel. I stället för att logga in på VM:en, pulla images och starta om containrar för hand sker det i rätt ordning efter att images har byggts. Automation gör därför inte bara arbetet snabbare. Den gör också att vi arbetar mer konsekvent och att samma kontroller körs varje gång.

# Alexander Dammert:

## 1. Produktens livscykel

Själva appen var nästan likadan hela kursen. Det som växte var allt runt omkring den.

Första veckorna gjorde vi allt med händerna. Vi skapade repot, skrev våra namn i README och pushade den första committen (M1). Sen låste vi main så att ingen fick skriva direkt där (M2). Allt måste gå via en pull request som den andra av oss godkände. Vi gjorde också en krock med flit. Albin och jag ändrade samma rubrik på var sin branch och sen lagade vi den tillsammans till "Albins och Alexanders anteckningar".

Sen lärde vi oss att packa appen med Docker. Docker bygger en image för backend och en för frontend, och docker compose startar båda på en gång (M3). Vi delade upp jobbet så att jag gjorde `.dockerignore` för frontend och Albin för backend. I M4 hittade vi en bugg: teckenräknaren visade fel när man tog bort en anteckning. Vi skrev ett test som först blev rött, lagade koden och såg samma test bli grönt.

Sen började maskinen hjälpa oss. GitHub Actions kollade varje pull request och stoppade en som var trasig (M5). Den byggde också nya images och lade dem i GHCR varje gång vi mergade (M6).

I M7 satte vi upp en dator i molnet hos cPouta och byggde ihop allt för hand: nyckel, brandvägg, IP-adress, Docker och appen. I M8 byggde Terraform samma dator utifrån en fil. Vi kunde riva datorn och bygga en ny och den fick ändå samma adress. I M9 kopplade vi ihop allt. Vi lade till texten "Try adding a note below:" på sidan och efter mergen syntes den på vår nip.io-adress utan att någon av oss loggade in på servern.

Ibland kändes det jobbigt att se all text i vissa milstolpar, men när man förstod helheten var instruktionerna lätta att följa. Det var bra att vi fick jobba självständigt. Genom att prova och misslyckas förstår man varför något funkar.

## 2. Vad går sönder utan automation?

Jag väljer det sista steget i kedjan när appen ska ut på servern.

I M7 gjorde vi det för hand. Vi loggade in med SSH, skrev `docker compose pull` och sen `up -d`. Det låter som två korta rader, men mycket kunde gå fel. När jag skulle starta Docker glömde jag `sudo` och fick ett fel om att jag inte hade rättigheter. Och var paketet i GHCR fortfarande privat sade servern bara "denied".

Om vi skulle göra så efter varje ändring idag skulle tre saker gå snett.

För det första kunde vi köra för tidigt. Om vi kör `pull` innan de nya images är färdigbyggda får servern den gamla versionen och vi tror att allt är nytt fast det inte är det. Vår `deploy.yml` väntar tills "Publish images" har blivit grön innan den börjar. En människa gissar, maskinen väntar.

För det andra kunde bara en av oss göra det. På Terraform-datorn fanns bara nyckeln från den codespace där vi körde `apply`. Nu har GitHub en egen deploy-nyckel så det spelar ingen roll vem av oss som mergar.

För det tredje skulle ingen veta vad som hände. När man gör något för hand finns inget spår kvar. I Actions ser vi varje körning, vilken commit den kom från och om den blev grön eller röd.

Det jag tar med mig är att automation inte bara sparar tid. Den gör samma sak i samma ordning varje gång och glömmer aldrig ett steg, som en trött människa lätt kan göra. Efter lektionen om GitHub Actions gjorde jag ett eget Python-skript. Var 15:e minut kollar det om en produkt på Verkkokauppa.com som står som "tulee pian" går att köpa. När den gör det skickar skriptet ett meddelande till mig på Discord. Då vet jag direkt utan att behöva kolla själv hela tiden.