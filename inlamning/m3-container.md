## Vilken rad avgör Python-versionen imagen bygger på?
### första raden "FROM python:3.12-slim" på grund av att installer den där.
## COPY requirements.txt . och RUN pip install ... kommer FÖRE COPY app ./app. Varför i den ordningen, och inte tvärtom? (Ledtråd: föreläsningens cache-slide.)
### På grund av att RUN pip install skulle runna varje gång du gör kod skillnader
## EXPOSE 8000 — tror ni den raden gör porten nåbar utanför containern, så att curl i er terminal når den? Testa gissningen i nästa deluppgift.
### Tror så