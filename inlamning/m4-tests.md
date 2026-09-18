# M4 – Bugjakt

## 1. Test som bevisar buggen:

Vid Testet skapar vi 'milk' och 'bread', tar bort 'bread' och hämtar sedan
summeringen.

Efter borttagningen borde det finnas 1 anteckning och 4 tecken, men det stämmer inte och på det här sättet hittade vi vår bug.

### Röd testkörning:

![test_bugjakt output](m4-screenshots/screenshot8.png)

## 2. En förklaring:

Code review tittade bara på det nya funktionen och tester som kom, men missade att kolla vad som händer när just en anteckning skapas och sedan raderas.

Den gröna testviten testade att räkningen fungerade när vi lade till anteckningar, men de kollade inte vad som hände när vi tog bort en anteckning.

Pull requesten kunde mergas eftersom alla befintliga tester var gröna. Det saknades ett test som skulle ha kontrollerat just att "characters" antalet minskar när man raderar en anteckning.

## 3. Vår fix:

Vi fixade buggen genom att uppdatera _total_characters när en anteckning tas bort. Vi minskar räknaren med längden på den borttagna anteckningen och använder global _total_characters eftersom variabeln ligger utanför funktionen.

![test_bugjakt fixed](m4-screenshots/screenshot9-fixed.png)

## 4. Hur kunde man ha undvikit hela buggen:

Det hade behövts ett test som skapade två anteckningar som tog bort en av dom och sedan kontrollerade vad slutliga summan av "characters" skulle ha blivit. Om det testet hade funnits från början hade buggen upptäckts innan Pull requesten blev merged.
###

# Vad vi gjorde utanför repot + skärmdumpar
### 1. Komma igång:
![Konsolen](m4-screenshots/screenshot1.png)
### 2. Cherry-pick:
![Konsolen](m4-screenshots/screenshot3.png)
### 3. Testade git logs:
![Konsolen](m4-screenshots/screenshot2.png)
### 4. Acceptade båda git commits och pullade båda:
![koden av båda commits](m4-screenshots/screenshot4-accept-both-changes.png)
### 5. Bygger backend pytest:
![Konsolen med building](m4-screenshots/screenshot5-part1.png)
### 6. Proof att vi passade alla tests:
![Konsolen med test passed](m4-screenshots/screenshot5-part2.png)
### 7. Vi bygger website hostingen:
![Konsolen med att vi lyckades bygga](m4-screenshots/screenshot6-part1.png)
### 8. Sidan up & running:
![Website running](m4-screenshots/screenshot6-part2.png)
### 9. Bug testing nu:
![Website bug testing](m4-screenshots/screenshot7-part1.png)
### 10. Bugged är att när man raderar notes så blir inte "characters" också deleted:
![Website bug testing](m4-screenshots/screenshot7-part2.png)
### 11. Efter att vi gjorde ett simpel test på test_bugjakt:
![Konsolen med att testet inte passade](m4-screenshots/screenshot8.png)
### 12. Fixade själva bugget:
![Print av vår fix funkade](m4-screenshots/screenshot9-fixed.png)
### 13. Nu bygger vi hela backend pytest again:
![Konsolen av building med att vi har inga errors](m4-screenshots/screenshot10.png)
### 14. Nu kör vi upp hela webbsidan igen med docker:
![Konsolen efter ändringen](m4-screenshots/screenshot10-p2.png)
### 15. Testar samma bug igen part 1:
![Konsolen efter ändringen](m4-screenshots/screenshot11.png)
### 16. Testar samma bug igen part 2:
![Konsolen efter ändringen](m4-screenshots/screenshot12.png)
## Allt fungerar nu och vi lyckades fixa problemet!