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
