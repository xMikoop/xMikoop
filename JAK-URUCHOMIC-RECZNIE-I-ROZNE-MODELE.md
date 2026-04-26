# Panel kilku agentów w pi — instrukcja ręczna od A do Z

Ten plik jest gotowym szablonem/procesem, który możesz później skopiować do innego tematu. Pokazuje, jak ręcznie uruchomić kilku „ekspertów” w pi, jak zapisywać ich pracę do plików i jak używać różnych modeli/subskrypcji dla różnych ról.

Przykład tematu:

> Wojna na Ukrainie w 2026 roku — perspektywa polityczna, ekonomiczna i zdrowotna.

---

## 0. Ważna idea

W pi możesz zrobić panel ekspertów na dwa sposoby:

### Wariant prosty

Jedna sesja pi udaje kilka ról.

Plusy:

- szybkie,
- tanie,
- proste.

Minusy:

- to nadal jeden model odgrywa kilka głosów.

### Wariant ręczny, bardziej „prawdziwy”

Otwierasz kilka terminali i w każdym działa osobna sesja pi:

- agent polityczny,
- agent ekonomiczny,
- agent zdrowia publicznego,
- moderator.

Plusy:

- każda rola ma osobny kontekst,
- możesz dać każdej roli inny model,
- łatwiej porównać różne style myślenia.

Minusy:

- trzeba ręcznie przenosić wyniki między sesjami,
- większy koszt albo szybsze zużycie limitów,
- trzeba uważać, żeby dwóch agentów nie pisało naraz do tego samego pliku.

---

## 1. Przygotuj folder roboczy

Przykład dla Windows/PowerShell:

```powershell
mkdir C:\Users\mikol\panel-ukraina-2026
cd C:\Users\mikol\panel-ukraina-2026
```

W tym folderze trzymaj pliki:

```text
00-temat.md
01-polityka.md
02-ekonomia.md
03-zdrowie.md
04-pytania-krzyzowe.md
05-synteza.md
prompts/agent-polityka.md
prompts/agent-ekonomia.md
prompts/agent-zdrowie.md
prompts/moderator.md
```

Zasada:

- każdy ekspert pisze do własnego pliku,
- moderator czyta wszystkie pliki,
- dopiero moderator robi finalną syntezę.

---

## 2. Przygotuj temat

Plik `00-temat.md` powinien zawierać temat, zakres i zasady.

Szablon:

```markdown
# Temat panelu

## Pytanie główne

Jakie mogą być konsekwencje [TEMAT] w perspektywie [OKRES]?

## Role ekspertów

1. Analityk polityczny.
2. Ekonomista.
3. Lekarz / ekspert zdrowia publicznego.
4. Moderator.

## Zasady

- Oddziel fakty od prognoz.
- Oznacz niepewności.
- Jeżeli brakuje aktualnych danych, napisz: „do weryfikacji”.
- Nie udawaj źródeł, których nie sprawdziłeś.
- Nie wygładzaj sporów między ekspertami.
```

---

## 3. Zaloguj subskrypcje w pi

W dowolnej sesji pi wpisz:

```text
/login
```

Następnie zaloguj te subskrypcje, których chcesz używać.

Przykładowe providery/subskrypcje w pi:

| Subskrypcja / konto | Provider w pi | Uwaga |
|---|---|---|
| ChatGPT Plus/Pro przez Codex | `openai-codex` | To jest subskrypcja, nie zwykłe API OpenAI. |
| OpenAI API | `openai` | To używa klucza API i nalicza koszty API. |
| Claude Pro/Max | `anthropic` | Ten sam provider może też działać z API key, zależnie od logowania. |
| GitHub Copilot | `github-copilot` | Dostęp zależy od Twojego Copilota i włączonych modeli. |
| Google Gemini CLI | `google-gemini-cli` | Konto Google / Cloud Code Assist. |
| Google Antigravity | `google-antigravity` | Modele udostępniane przez Antigravity. |

Ważne rozróżnienie:

```text
openai        = OpenAI API key
openai-codex  = ChatGPT Plus/Pro / Codex subscription
```

Jeśli chcesz używać subskrypcji ChatGPT/Codex, wybieraj modele z providera `openai-codex`, a nie `openai`.

---

## 4. Sprawdź dostępne modele

W pi interaktywnie:

```text
/model
```

albo skrótem:

```text
Ctrl+L
```

W terminalu możesz też sprawdzić listę:

```powershell
pi --list-models gpt
pi --list-models claude
pi --list-models gemini
```

Jeżeli modelu nie widać:

1. zaktualizuj pi:

```powershell
npm install -g @mariozechner/pi-coding-agent@latest
```

2. zaloguj właściwego providera przez `/login`,
3. sprawdź, czy wybierasz właściwy provider, np. `openai-codex`, a nie `openai`,
4. sprawdź, czy model jest dostępny w Twojej subskrypcji.

---

## 5. Zaplanuj, który ekspert używa którego modelu

Przykładowa macierz:

| Rola | Model | Provider | Dlaczego |
|---|---|---|---|
| Polityka | GPT-5.5 | `openai-codex` | mocny model ogólny, dobre rozumowanie strategiczne |
| Ekonomia | Claude / inny model | `anthropic` albo inny | dobry do długich analiz i ostrożnych wniosków |
| Zdrowie publiczne | Gemini / GPT / Claude | wybrany provider | można użyć modelu ostrożnego, dobrze strukturyzującego ryzyka |
| Moderator | najlepszy dostępny model | dowolny | moderator powinien najlepiej syntetyzować spory |

To jest tylko przykład. Najważniejsze jest to, żeby w każdym terminalu jawnie sprawdzić provider/model w stopce pi albo przez `/model`.

---

## 6. Uruchom osobne terminale

Otwórz 4 terminale PowerShell.

W każdym:

```powershell
cd C:\Users\mikol\panel-ukraina-2026
```

### Terminal 1 — agent polityczny

Przykład z subskrypcją ChatGPT/Codex:

```powershell
cd C:\Users\mikol\panel-ukraina-2026
$role = Get-Content .\prompts\agent-polityka.md -Raw -Encoding UTF8
pi --provider openai-codex --model gpt-5.5 --append-system-prompt "$role" @00-temat.md "Wykonaj analizę polityczną i zapisz wynik do 01-polityka.md. Oddziel fakty, założenia, prognozy i niepewności."
```

Jeżeli nie znasz dokładnego ID modelu, uruchom po prostu:

```powershell
pi
```

potem wpisz:

```text
/model
```

wybierz model ręcznie, a następnie wklej prompt.

---

### Terminal 2 — agent ekonomiczny

Przykład z innym providerem/modelem:

```powershell
cd C:\Users\mikol\panel-ukraina-2026
$role = Get-Content .\prompts\agent-ekonomia.md -Raw -Encoding UTF8
pi --provider anthropic --model "MODEL_Z_LISTY" --append-system-prompt "$role" @00-temat.md "Wykonaj analizę ekonomiczną i zapisz wynik do 02-ekonomia.md. Oddziel fakty, założenia, prognozy i niepewności."
```

Zamień `MODEL_Z_LISTY` na model znaleziony w `/model` albo `pi --list-models claude`.

Jeśli używasz Claude Pro/Max przez subskrypcję, najpierw zaloguj się przez:

```text
/login
```

i wybierz Anthropic/Claude Pro/Max.

---

### Terminal 3 — agent zdrowia publicznego

Przykład z Gemini CLI:

```powershell
cd C:\Users\mikol\panel-ukraina-2026
$role = Get-Content .\prompts\agent-zdrowie.md -Raw -Encoding UTF8
pi --provider google-gemini-cli --model "MODEL_Z_LISTY" --append-system-prompt "$role" @00-temat.md "Wykonaj analizę zdrowia publicznego i zapisz wynik do 03-zdrowie.md. To ma być analiza systemowa, nie indywidualna porada medyczna."
```

Zamień `MODEL_Z_LISTY` na model znaleziony w:

```powershell
pi --list-models gemini
```

albo wybierz go ręcznie przez `/model`.

---

### Terminal 4 — moderator

Moderator powinien ruszyć dopiero po tym, jak trzy pliki są gotowe:

```text
01-polityka.md
02-ekonomia.md
03-zdrowie.md
```

Przykład:

```powershell
cd C:\Users\mikol\panel-ukraina-2026
$role = Get-Content .\prompts\moderator.md -Raw -Encoding UTF8
pi --provider openai-codex --model gpt-5.5 --append-system-prompt "$role" @00-temat.md @01-polityka.md @02-ekonomia.md @03-zdrowie.md "Przeczytaj trzy analizy. Przygotuj rundę pytań krzyżowych i zapisz ją do 04-pytania-krzyzowe.md. Pokaż spory, luki i założenia wymagające sprawdzenia."
```

---

## 7. Runda pytań krzyżowych

Po tym jak moderator stworzy `04-pytania-krzyzowe.md`, wróć do każdego eksperta.

### Do agenta politycznego wklej:

```text
Przeczytaj @04-pytania-krzyzowe.md oraz pozostałe analizy, jeśli są potrzebne.
Zaktualizuj tylko sekcje polityczne w @01-polityka.md.
Nie nadpisuj plików ekonomii ani zdrowia.
```

### Do agenta ekonomicznego:

```text
Przeczytaj @04-pytania-krzyzowe.md oraz pozostałe analizy, jeśli są potrzebne.
Zaktualizuj tylko sekcje ekonomiczne w @02-ekonomia.md.
Nie nadpisuj plików polityki ani zdrowia.
```

### Do agenta zdrowia publicznego:

```text
Przeczytaj @04-pytania-krzyzowe.md oraz pozostałe analizy, jeśli są potrzebne.
Zaktualizuj tylko sekcje zdrowotne w @03-zdrowie.md.
Nie nadpisuj plików polityki ani ekonomii.
```

---

## 8. Synteza końcowa moderatora

Po aktualizacji trzech analiz wróć do moderatora i wpisz:

```text
Przeczytaj @00-temat.md, @01-polityka.md, @02-ekonomia.md, @03-zdrowie.md i @04-pytania-krzyzowe.md.
Przygotuj końcową syntezę do @05-synteza.md.

W syntezie pokaż:
1. streszczenie w 10 punktach,
2. mapę zgody ekspertów,
3. mapę sporów ekspertów,
4. scenariusz optymistyczny, bazowy, pesymistyczny i zaskoczenie,
5. najważniejsze ryzyka,
6. wskaźniki do monitorowania,
7. dane i źródła do sprawdzenia przed poważnym użyciem analizy.
```

---

## 9. Jak zmienić model w działającej sesji

W pi wpisz:

```text
/model
```

albo użyj:

```text
Ctrl+L
```

Wybierz provider i model.

Jeżeli obok modelu widzisz np. `medium`, to jest poziom reasoning/thinking, czyli intensywność rozumowania.

Zmienisz go przez:

```text
/settings
```

albo skrótem:

```text
Shift+Tab
```

Możliwe poziomy zwykle obejmują:

```text
off
minimal
low
medium
high
xhigh
```

Do trudnych analiz możesz używać `high`, ale może być wolniej i drożej albo szybciej zużywać limity.

---

## 10. Jak uruchamiać pi od razu z konkretnym modelem

Ogólny wzór:

```powershell
pi --provider PROVIDER --model MODEL
```

Przykłady:

```powershell
pi --provider openai-codex --model gpt-5.5
```

```powershell
pi --provider openai --model gpt-5.5
```

```powershell
pi --provider anthropic --model "MODEL_Z_LISTY"
```

```powershell
pi --provider github-copilot --model "MODEL_Z_LISTY"
```

```powershell
pi --provider google-gemini-cli --model "MODEL_Z_LISTY"
```

```powershell
pi --provider google-antigravity --model "MODEL_Z_LISTY"
```

Możesz też dopisać poziom thinking:

```powershell
pi --provider openai-codex --model gpt-5.5 --thinking high
```

albo czasem skrótem:

```powershell
pi --model gpt-5.5:high
```

---

## 11. Co jeśli model ma tę samą nazwę u różnych providerów?

Używaj providera jawnie.

Zamiast:

```powershell
pi --model gpt-5.5
```

lepiej:

```powershell
pi --provider openai-codex --model gpt-5.5
```

albo:

```powershell
pi --provider openai --model gpt-5.5
```

Dzięki temu nie pomylisz subskrypcji z API.

---

## 12. Jak sprawdzić, czy używasz subskrypcji czy API

Patrz na provider w stopce pi albo w selektorze `/model`.

Jeśli widzisz:

```text
openai / gpt-...
```

to jest zwykle OpenAI API.

Jeśli widzisz:

```text
openai-codex / gpt-...
```

to jest subskrypcja ChatGPT Plus/Pro przez Codex.

Dla OpenAI najważniejsze jest:

```text
openai        = API
openai-codex  = subskrypcja Codex/ChatGPT
```

Jeżeli masz ustawione `OPENAI_API_KEY`, pi może używać API dla providera `openai`. To nie przeszkadza w używaniu `openai-codex`, ale trzeba świadomie wybrać właściwy provider.

---

## 13. Jak ustawić domyślny model/providera

Ustawienia pi są w:

```text
C:\Users\mikol\.pi\agent\settings.json
```

Przykład dla subskrypcji Codex:

```json
{
  "defaultProvider": "openai-codex",
  "defaultModel": "gpt-5.5"
}
```

Przykład dla OpenAI API:

```json
{
  "defaultProvider": "openai",
  "defaultModel": "gpt-5.5"
}
```

Jeśli często przełączasz modele, możesz nie ustawiać tego ręcznie i wybierać model przez `/model`.

---

## 14. Jak nazwać sesje, żeby się nie pogubić

W każdej sesji pi możesz wpisać:

```text
/name panel-polityka
```

```text
/name panel-ekonomia
```

```text
/name panel-zdrowie
```

```text
/name panel-moderator
```

Dzięki temu łatwiej wrócić do sesji przez:

```text
/resume
```

albo z terminala:

```powershell
pi -r
```

---

## 15. Zasady bezpieczeństwa pracy na plikach

Najważniejsza zasada:

> Nie pozwalaj kilku agentom pisać jednocześnie do tego samego pliku.

Bezpieczny podział:

```text
agent polityczny      -> 01-polityka.md
agent ekonomiczny     -> 02-ekonomia.md
agent zdrowia         -> 03-zdrowie.md
moderator             -> 04-pytania-krzyzowe.md i 05-synteza.md
```

Jeżeli agent ma tylko czytać cudzy plik, napisz mu jasno:

```text
Przeczytaj ten plik, ale go nie edytuj.
```

Jeżeli ma edytować tylko swój plik:

```text
Edytuj wyłącznie 01-polityka.md. Nie zmieniaj innych plików.
```

---

## 16. Gotowy schemat procesu

### Krok 1

Przygotuj temat w `00-temat.md`.

### Krok 2

Przygotuj role w folderze `prompts/`.

### Krok 3

Zaloguj subskrypcje przez `/login`.

### Krok 4

Sprawdź modele przez `/model` albo `pi --list-models`.

### Krok 5

Otwórz terminale dla ekspertów.

### Krok 6

Każdy ekspert pisze własną analizę do własnego pliku.

### Krok 7

Moderator czyta trzy analizy i tworzy pytania krzyżowe.

### Krok 8

Eksperci odpowiadają na pytania i poprawiają swoje pliki.

### Krok 9

Moderator robi `05-synteza.md`.

### Krok 10

Ty czytasz syntezę i prosisz o:

- skrócenie,
- tabelę,
- wersję publicystyczną,
- wersję akademicką,
- listę źródeł do sprawdzenia,
- wariant „co jeśli założenie X jest fałszywe?”.

---

## 17. Minimalny gotowiec do skopiowania

Jeśli chcesz szybko odpalić ręczny panel, użyj tego schematu:

```text
Otwieram panel ekspertów.
Moja rola w tej sesji: [ROLA].
Temat jest w @00-temat.md.
Instrukcja roli jest w @[PLIK_ROLI].

Wykonaj swoją część analizy.
Zapisz wynik wyłącznie do [PLIK_WYJŚCIOWY].
Oddziel:
- fakty,
- założenia,
- prognozy,
- niepewności,
- dane do weryfikacji.

Nie edytuj innych plików.
```

Przykład:

```text
Otwieram panel ekspertów.
Moja rola w tej sesji: ekonomista.
Temat jest w @00-temat.md.
Instrukcja roli jest w @prompts/agent-ekonomia.md.

Wykonaj swoją część analizy.
Zapisz wynik wyłącznie do 02-ekonomia.md.
Oddziel fakty, założenia, prognozy, niepewności i dane do weryfikacji.
Nie edytuj innych plików.
```

---

## 18. Najczęstsze problemy

### Problem: widzę GPT-5.5, ale nie wiem czy to API czy subskrypcja

Sprawdź provider:

```text
openai        = API
openai-codex  = subskrypcja Codex/ChatGPT
```

### Problem: model jest w Codexie, ale nie widzę go w pi

Spróbuj:

```powershell
npm install -g @mariozechner/pi-coding-agent@latest
```

potem:

```text
/login
/model
```

### Problem: dwóch agentów nadpisało sobie pliki

Ustal twardo:

```text
Ten agent może edytować tylko jeden plik: X.md.
```

I najlepiej trzymaj osobne pliki dla każdej roli.

### Problem: chcę szybko przełączać modele

Używaj:

```text
/model
```

albo:

```text
Ctrl+L
```

Dla cyklicznego przełączania wybranych modeli możesz użyć:

```text
/scoped-models
```

---

## 19. Najlepsza praktyka

Dla ważnych analiz używaj układu:

```text
słabszy/szybszy model  -> pierwsze szkice eksperckie
mocniejszy model       -> krytyka i synteza
najlepszy model        -> finalny moderator
```

Czyli np.:

```text
polityka   -> model A
ekonomia   -> model B
zdrowie    -> model C
moderator  -> najlepszy dostępny model
```

Moderator powinien dostać wszystkie pliki i mieć polecenie:

```text
Nie zgadzaj się automatycznie z ekspertami. Wskaż konflikty założeń, słabe punkty i brakujące dane.
```

---

## 20. Krótka wersja całego procesu

1. Tworzysz temat.
2. Tworzysz role ekspertów.
3. Logujesz subskrypcje przez `/login`.
4. Dla każdego terminala wybierasz inny provider/model.
5. Eksperci piszą osobne pliki.
6. Moderator robi pytania krzyżowe.
7. Eksperci poprawiają odpowiedzi.
8. Moderator robi syntezę.
9. Ty decydujesz, co dalej: skrócić, rozwinąć, sprawdzić źródła, zmienić scenariusze.

Gotowe.
