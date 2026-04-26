# Panel ekspertów: Ukraina 2026 — proces w pi

Ten folder to gotowy, kopiowalny szablon pracy z kilkoma agentami/rolami w pi.

Cel: uruchomić debatę ekspertów o wojnie na Ukrainie w 2026 roku z perspektyw:

1. polityka/geopolityka,
2. ekonomia,
3. zdrowie publiczne/medycyna,
4. moderator/synteza.

> Uwaga: pi domyślnie nie ma automatycznych sub-agentów. Ten szablon pokazuje dwa tryby: ręczny panel w kilku terminalach albo symulację panelu w jednej sesji.

---

## Struktura folderu

```text
panel-ukraina-2026/
├── README.md
├── JAK-URUCHOMIC-RECZNIE-I-ROZNE-MODELE.md
├── 00-temat.md
├── 01-polityka.md
├── 02-ekonomia.md
├── 03-zdrowie.md
├── 04-pytania-krzyzowe.md
├── 05-synteza.md
├── prompts/
│   ├── agent-polityka.md
│   ├── agent-ekonomia.md
│   ├── agent-zdrowie.md
│   └── moderator.md
└── proces/
    ├── 00-protokol-demo.md
    └── copy-paste-komendy.md
```

---

## Tryb A — kilka prawdziwych sesji pi

Otwórz 4 terminale w tym folderze:

```powershell
cd C:\Users\mikol\panel-ukraina-2026
```

W każdym terminalu uruchamiasz pi z inną rolą. Gotowe komendy są w:

```text
proces/copy-paste-komendy.md
```

Ten tryb daje niezależne sesje, czyli agent polityczny, ekonomiczny i zdrowotny nie są tylko „udawanymi głosami” w jednej odpowiedzi.

---

## Tryb B — jedna sesja pi symuluje panel

Wygodne, szybkie i tańsze, ale technicznie to jeden model odgrywa kilka ról.

Przykładowy prompt:

```text
Przeprowadź panel ekspertów na podstawie @00-temat.md.
Role weź z plików @prompts/agent-polityka.md, @prompts/agent-ekonomia.md, @prompts/agent-zdrowie.md i @prompts/moderator.md.
Zapisz wynik do 05-synteza.md, a widoczny protokół pracy do proces/00-protokol-demo.md.
Nie pokazuj ukrytego chain-of-thought; pokaż tylko jawny proces: tezy, pytania, sprzeczności, wnioski.
```

---

## Zasady jakości

- Oddzielaj **fakty**, **założenia**, **prognozy** i **niepewności**.
- Przy aktualnych wydarzeniach wymagaj źródeł albo oznacz sekcję jako „do weryfikacji”.
- Nie przedstawiaj prognoz jako pewników.
- Lekarz/ekspert zdrowia publicznego nie daje indywidualnej porady medycznej — analizuje skutki systemowe.
- Moderator ma obowiązek wskazać konflikty między ekspertami, a nie tylko sklejać ich opinie.

---

## Czym jest „proces” w tym folderze?

Plik `proces/00-protokol-demo.md` pokazuje **jawny proces roboczy**:

- kto ma jaką rolę,
- jakie są rundy debaty,
- jakie tezy padły,
- jakie pytania zadali sobie eksperci,
- gdzie są spory,
- jaki jest wynik moderatora.

Nie jest to ukryte rozumowanie modelu krok po kroku. To protokół pracy, który możesz potem kopiować i modyfikować.
