# Komendy do skopiowania — panel ekspertów w pi

Najpierw przejdź do folderu:

```powershell
cd C:\Users\mikol\panel-ukraina-2026
```

---

## Wariant 1 — prawdziwe osobne sesje, 4 terminale

Otwórz cztery okna/zakładki PowerShell.

### Terminal 1 — polityka

```powershell
cd C:\Users\mikol\panel-ukraina-2026
$role = Get-Content .\prompts\agent-polityka.md -Raw -Encoding UTF8
pi --append-system-prompt "$role" @00-temat.md "Przyjmij rolę analityka politycznego. Wykonaj rundę 1 i zapisz wynik do 01-polityka.md. Oddziel fakty, założenia, prognozy i niepewności."
```

### Terminal 2 — ekonomia

```powershell
cd C:\Users\mikol\panel-ukraina-2026
$role = Get-Content .\prompts\agent-ekonomia.md -Raw -Encoding UTF8
pi --append-system-prompt "$role" @00-temat.md "Przyjmij rolę ekonomisty. Wykonaj rundę 1 i zapisz wynik do 02-ekonomia.md. Oddziel fakty, założenia, prognozy i niepewności."
```

### Terminal 3 — zdrowie publiczne

```powershell
cd C:\Users\mikol\panel-ukraina-2026
$role = Get-Content .\prompts\agent-zdrowie.md -Raw -Encoding UTF8
pi --append-system-prompt "$role" @00-temat.md "Przyjmij rolę lekarza / eksperta zdrowia publicznego. Wykonaj rundę 1 i zapisz wynik do 03-zdrowie.md. To ma być analiza systemowa, nie indywidualna porada medyczna."
```

### Terminal 4 — moderator

Po zakończeniu pracy trzech agentów:

```powershell
cd C:\Users\mikol\panel-ukraina-2026
$role = Get-Content .\prompts\moderator.md -Raw -Encoding UTF8
pi --append-system-prompt "$role" @00-temat.md @01-polityka.md @02-ekonomia.md @03-zdrowie.md "Przeczytaj trzy analizy. Przygotuj rundę pytań krzyżowych i zapisz ją do 04-pytania-krzyzowe.md. Pokaż spory i luki, nie wygładzaj różnic."
```

Potem możesz wkleić `04-pytania-krzyzowe.md` do każdego z trzech agentów i poprosić o korektę swojej analizy.

Na końcu w terminalu moderatora:

```powershell
pi @00-temat.md @01-polityka.md @02-ekonomia.md @03-zdrowie.md @04-pytania-krzyzowe.md "Przygotuj końcową syntezę moderatora i zapisz ją do 05-synteza.md."
```

---

## Wariant 2 — jedna sesja symuluje cały panel

To jest tańsze i prostsze, ale mniej „prawdziwie wieloagentowe”.

```powershell
cd C:\Users\mikol\panel-ukraina-2026
pi @00-temat.md @prompts\agent-polityka.md @prompts\agent-ekonomia.md @prompts\agent-zdrowie.md @prompts\moderator.md "Przeprowadź panel ekspertów. Najpierw wygeneruj trzy analizy do 01-polityka.md, 02-ekonomia.md i 03-zdrowie.md. Następnie wygeneruj rundę pytań krzyżowych do 04-pytania-krzyzowe.md. Na końcu przygotuj syntezę do 05-synteza.md. Do proces/00-protokol-demo.md zapisz jawny protokół pracy: role, rundy, tezy, spory, decyzje moderatora. Nie pokazuj ukrytego chain-of-thought."
```

---

## Wariant 3 — szybki prompt bez zapisu do wielu plików

```text
Zrób panel ekspertów o wojnie na Ukrainie w 2026 roku.
Role:
1. Analityk polityczny.
2. Ekonomista.
3. Lekarz / ekspert zdrowia publicznego.
4. Moderator.

Zasady:
- oddziel fakty od prognoz,
- oznacz niepewności,
- pokaż spory,
- nie dawaj instrukcji wojskowych,
- lekarz analizuje zdrowie publiczne, nie daje indywidualnych porad,
- na końcu moderator robi syntezę.
```
