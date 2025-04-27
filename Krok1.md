Krok 1: Przygotowanie środowiska 
Konfiguracja narzędzi, których używam. Wszystko będzie online, żebym nie musiała nic instalować.

1.1. Google Colab (środowisko do Pythona)
Wejdź na Google Colab.
Zaloguj się na konto Google.
Utwórz nowy notebook: Kliknij "File" → "New Notebook".
To Twoje środowisko – tu będziesz pisać kod.
1.2. GitHub (zarządzanie projektem)
Załóż konto na GitHub.
Utwórz nowe repozytorium: Kliknij "+" → "New Repository".
Nazwa: malware-detection.
Ustaw jako publiczne.
Zainicjalizuj z README.
Zainstaluj Git (na komputerze):
Pobierz z git-scm.com.
Zainstaluj (proste "Next, Next").
Połącz Git z GitHub:
Otwórz terminal (w Windows: "Git Bash").

Skonfiguruj
git config --global user.name "TwojeImie"
git config --global user.email "twoj.email@example.com"

Sklonuj repozytorium:
git clone https://github.com/twoje-konto/malware-detection.git

Przejdź do folderu:
cd malware-detection

1.3. Azure (prosta aplikacja)
Załóż darmowe konto na Azure.
Użyj konta studenckiego (daje 100 USD kredytu).
Na razie tylko konto – aplikację zrobimy później.



Dokładne

### Krok 1: Otwórz terminal
- Jeśli używasz Windows, otwórz **Git Bash**:
  - Wyszukaj "Git Bash" w menu Start i uruchom.
- Jeśli używasz MacOS/Linux, otwórz zwykły terminal.

---

### Krok 2: Skonfiguruj Git
Musisz ustawić swoje dane (imię i email), które będą powiązane z commitami na GitHub.

W terminalu wpisz:
```
git config --global user.name "DarynaOv"
git config --global user.email "twój.email@example.com"
```
- Zamiast `twój.email@example.com` wpisz email, którego użyłaś do założenia konta na GitHub (np. `daryna.ov@example.com`).
- `DarynaOv` to Twoja nazwa użytkownika GitHub, ale możesz wpisać dowolne imię, np. "Daryna".

Sprawdź, czy konfiguracja się zapisała:
```
git config --global user.name
git config --global user.email
```
Powinno wyświetlić Twoje dane.

---

### Krok 3: Sklonuj repozytorium
Teraz pobierzesz swoje repozytorium `Praca_Inzynierska` na komputer.

W terminalu wpisz:
```
git clone https://github.com/DarynaOv/Praca_Inzynierska.git
```
- To pobierze repozytorium na Twój komputer do folderu `Praca_Inzynierska`.
- Jeśli GitHub poprosi o zalogowanie, wpisz nazwę użytkownika (`DarynaOv`) i hasło/token:
  - Jeśli używasz hasła, wpisz swoje hasło do GitHub.
  - Jeśli GitHub wymaga tokenu (nowsze wersje), musisz go wygenerować:
    - Wejdź na GitHub → Ustawienia → Developer Settings → Personal Access Tokens → Generate New Token.
    - Wygeneruj token z uprawnieniami `repo` i zapisz go.
    - Użyj tego tokenu zamiast hasła.

---

### Krok 4: Przejdź do folderu
Po sklonowaniu przejdź do folderu repozytorium:
```
cd Praca_Inzynierska
```
- Sprawdź, czy jesteś w dobrym folderze, wpisując:
  ```
  pwd
  ```
  Powinno wyświetlić ścieżkę, np. `/ścieżka/do/Praca_Inzynierska`.

---

### Krok 5: (Dodatkowe) Sprawdź zawartość i połączenie
- Zobacz, co jest w folderze:
  ```
  dir  # W Windows
  ls   # W MacOS/Linux
  ```
  Powinieneś zobaczyć plik `README.md` (jeśli go utworzyłaś na GitHub).

- Sprawdź, czy repozytorium jest poprawnie podłączone:
  ```
  git remote -v
  ```
  Powinno wyświetlić:
  ```
  origin  https://github.com/DarynaOv/Praca_Inzynierska.git (fetch)
  origin  https://github.com/DarynaOv/Praca_Inzynierska.git (push)
  ```

---

### Krok 6: (Praktyka) Dodaj plik i wyślij na GitHub
Żeby upewnić się, że wszystko działa, dodamy testowy plik i wyślemy zmiany na GitHub.

1. Utwórz plik testowy (możesz to zrobić ręcznie lub w terminalu):
   ```
   echo "Testowy plik" > test.txt
   ```
2. Dodaj plik do Gita:
   ```
   git add test.txt
   ```
3. Zrób commit (zapis zmian):
   ```
   git commit -m "Dodano testowy plik"
   ```
4. Wyślij zmiany na GitHub:
   ```
   git push
   ```
   Jeśli poprosi o dane logowania, wpisz nazwę użytkownika (`DarynaOv`) i hasło/token.

5. Sprawdź na GitHub:
   - Wejdź na `https://github.com/DarynaOv/Praca_Inzynierska`.
   - Powinieneś zobaczyć nowy plik `test.txt`.

---

### Krok 7: (Dla pracy) Jak dodawać pliki do repozytorium?
W trakcie pracy będziesz dodawać kod, wykresy i samą pracę. Robisz to tak:
- Skopiuj pliki do folderu `Praca_Inzynierska` (np. `malware_detection.py`, `plot_accuracy.png`, `malware_detection.pdf`).
- W terminalu:
  ```
  git add .
  git commit -m "Dodano pliki pracy inżynierskiej"
  git push
  ```
- Wyślij promotorowi link do repozytorium: `https://github.com/DarynaOv/Praca_Inzynierska`.



### Jeśli coś pójdzie nie tak:
- **Błąd autoryzacji**: Upewnij się, że używasz tokena (nie hasła) – wygeneruj go na GitHub.
- **Błąd "not a git repository"**: Upewnij się, że jesteś w folderze `Praca_Inzynierska` (użyj `cd Praca_Inzynierska`).
- **Błąd push**: Sprawdź, czy masz uprawnienia (jesteś zalogowana na swoje konto).



Napotkany problem
Rozumiem, że jesteś w trakcie klonowania repozytorium GitHub (`Praca_Inzynierska`) za pomocą komendy `git clone https://github.com/DarynaOv/Praca_Inzynierska.git`, ale terminal (Git Bash) zatrzymał się na etapie uwierzytelniania i nie możesz wpisać nazwy użytkownika ani hasła/tokena. Komunikat "Please complete the authentication in your browser" wskazuje, że Git próbuje otworzyć przeglądarkę, aby Cię uwierzytelnić, ale coś poszło nie tak. Pomogę Ci to rozwiązać i dokończyć klonowanie repozytorium.

---

### Problem: Dlaczego nie możesz wpisać danych logowania?
GitHub od 2021 roku nie pozwala na uwierzytelnianie przez hasło w terminalu – zamiast tego używa tokenów (Personal Access Token) lub uwierzytelniania przez przeglądarkę. Twój Git próbuje otworzyć przeglądarkę, aby zalogować się do GitHub, ale albo przeglądarka się nie otworzyła, albo proces się zawiesił. Możemy to obejść, ręcznie podając token w terminalu.

---

### Rozwiązanie: Użyj tokena Personal Access Token (PAT)

#### Krok 1: Anuluj bieżącą operację
- W terminalu (Git Bash) naciśnij **Ctrl+C**.  
  To przerwie bieżącą operację `git clone` i pozwoli Ci wpisać nowe komendy.

#### Krok 2: Wygeneruj token na GitHub
1. Wejdź na [GitHub](https://github.com) i zaloguj się na swoje konto (`DarynaOv`).
2. Przejdź do ustawień:
   - Kliknij na swoje zdjęcie profilowe (prawy górny róg) → **Settings**.
3. Przejdź do **Developer Settings**:
   - W menu po lewej wybierz **Developer settings** → **Personal access tokens** → **Tokens (classic)** → **Generate new token**.
4. Wygeneruj token:
   - Nazwa tokena: np. `Praca_Inzynierska`.
   - Ustaw uprawnienia: Zaznacz `repo` (daje dostęp do repozytoriów).
   - Wygeneruj token i **zapisz go** (np. skopiuj do notatnika) – nie zobaczysz go ponownie.

#### Krok 3: Ponownie sklonuj repozytorium z tokenem
1. W terminalu (Git Bash) wpisz zmodyfikowaną komendę `git clone`, dodając token do adresu URL:
   ```
   git clone https://DarynaOv:TWÓJ_TOKEN@github.com/DarynaOv/Praca_Inzynierska.git
   ```
   - Zamiast `TWÓJ_TOKEN` wstaw token, który wygenerowałaś (np. `ghp_abc123...`).
   - Przykład:
     ```
     git clone https://DarynaOv:ghp_abc123xyz789@github.com/DarynaOv/Praca_Inzynierska.git
     ```
2. Terminal powinien teraz pobrać repozytorium bez problemu:
   - Zobaczysz coś w stylu:
     ```
     Cloning into 'Praca_Inzynierska'...
     remote: Enumerating objects: 14, done.
     remote: Counting objects: 100% (14/14), done.
     remote: Compressing objects: 100% (10/10), done.
     remote: Total 14 (delta 1), reused 0 (delta 0), pack-reused 0
     Receiving objects: 100% (14/14), 22.71 KiB | 289.00 KiB/s, done.
     Resolving deltas: 100% (1/1), done.
     ```

#### Krok 4: Przejdź do folderu
Po sklonowaniu repozytorium przejdź do folderu:
```
cd Praca_Inzynierska
```
Sprawdź, czy jesteś w dobrym folderze:
```
pwd
```
Powinno wyświetlić ścieżkę, np. `/ścieżka/do/Praca_Inzynierska`.

#### Krok 5: Sprawdź zawartość
Sprawdź, jakie pliki są w folderze:
```
dir  # W Windows (Git Bash)
```
Powinieneś zobaczyć np. `README.md`, jeśli go utworzyłaś na GitHub.

---

### Krok 6: Dodaj plik testowy i wyślij na GitHub (kontynuacja)
Teraz, gdy repozytorium jest sklonowane, możesz dokończyć krok z dodaniem pliku testowego i wysłaniem go na GitHub.

1. Utwórz plik testowy:
   ```
   echo "Testowy plik" > test.txt
   ```
2. Dodaj plik do Gita:
   ```
   git add test.txt
   ```
3. Zrób commit:
   ```
   git commit -m "Dodano testowy plik"
   ```
4. Wyślij zmiany na GitHub:
   ```
   git push
   ```
   - Jeśli poprosi o dane logowania, użyj znowu swojego tokena:
     - Nazwa użytkownika: `DarynaOv`.
     - Hasło: Twój token (np. `ghp_abc123...`).
5. Sprawdź na GitHub:
   - Wejdź na [https://github.com/DarynaOv/Praca_Inzynierska](https://github.com/DarynaOv/Praca_Inzynierska).
   - Powinieneś zobaczyć nowy plik `test.txt`.

---

### Dodatkowe wskazówki
- **Zapisz token**: Zapisz swój token w bezpiecznym miejscu (np. w notatniku), bo będzie potrzebny przy każdym `git push` lub `git clone`.
- **Użyj menedżera poświadczeń (opcjonalne)**:
  - Możesz skonfigurować Git, żeby zapamiętał token:
    ```
    git config --global credential.helper store
    ```
    Przy pierwszym `git push` wpisz token, a Git zapamięta go na przyszłość.
