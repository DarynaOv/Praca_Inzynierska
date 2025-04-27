### Krok 2: Wczytywanie danych (Dzień 3-4, 2-4 godziny)
Musimy pobrać i wczytać dane, które wykorzystasz do trenowania modelu ML. Promotor wymaga:
- Kilka różnych zbiorów danych.
- Zbiór syntetyczny (wygenerowany przez Ciebie).

Zrobimy to w Google Colab, bo masz tam już skonfigurowane środowisko.

---

#### Krok 2.1: Pobranie danych
Będziemy używać trzech zbiorów danych: dwa prawdziwe (z Kaggle i VirusShare) oraz jeden syntetyczny (wygenerowany w Pythonie).

##### 2.1.1. Zbiór 1: Kaggle (Malware Classification Dataset)
1. Wejdź na [Kaggle](https://www.kaggle.com/datasets).
2. W wyszukiwarce wpisz "Malware Classification Dataset" lub "Malware Analysis Dataset".
   - Przykładowy zbiór: [Malware Analysis Dataset](https://www.kaggle.com/datasets/shashwatwork/malware-analysis-dataset).
3. Pobierz plik CSV (np. `malware_data.csv`):
   - Kliknij "Download" (może wymagać zalogowania na Kaggle – załóż konto, jeśli nie masz).
   - Plik zapisze się na Twoim komputerze (np. w folderze "Pobrane").
4. Prześlij plik do Google Colab:
   - Otwórz swój notebook w Google Colab (ten, który utworzyłaś w Kroku 1).
   - Po lewej stronie kliknij ikonę folderu 📁.
   - Przeciągnij plik `malware_data.csv` z komputera do panelu plików w Colab (lub kliknij "Upload" i wybierz plik).

##### 2.1.2. Zbiór 2: VirusShare
1. Wejdź na [VirusShare](https://virusshare.com/).
   - Potrzebujesz konta (darmowe) – zarejestruj się, jeśli jeszcze nie masz.
2. Pobierz mały zbiór próbek malware (np. 100 plików):
   - VirusShare udostępnia pliki w formacie ZIP – pobierz jeden z nowszych (np. "VirusShare_00400.zip").
   - Rozpakuj ZIP na komputerze (kliknij prawym przyciskiem → "Wypakuj").
3. Wyodrębnij cechy (rozmiar, entropia) z plików:
   - To wymaga analizy plików, co może być skomplikowane dla początkującego. Aby uprościć, możesz użyć gotowego zbioru danych z cechami (np. z Kaggle) lub stworzyć prosty plik CSV ręcznie.
   - Alternatywnie: Wyszukaj na Kaggle zbiór "VirusShare Features" – niektórzy użytkownicy udostępniają gotowe CSV z cechami (np. `virusshare_data.csv`).
   - Jeśli znajdziesz taki zbiór, pobierz go i prześlij do Colab (tak jak w punkcie 2.1.1).
   - **Uproszczenie**: Jeśli nie znajdziesz gotowego zbioru, pomiń ten krok na razie – użyjemy tylko zbioru z Kaggle i syntetycznego. Wrócimy do VirusShare, jeśli będzie potrzebne.

##### 2.1.3. Zbiór 3: Syntetyczny
- Wygenerujemy syntetyczne dane w Google Colab – nie musisz nic pobierać, zrobimy to w kodzie.

---

#### Krok 2.2: Wczytywanie danych w Google Colab
Teraz wczytamy dane do Colab i połączymy je w jeden zbiór.

1. **Otwórz notebook w Google Colab**:
   - Upewnij się, że masz otwarty notebook z Kroku 1.
2. **Utwórz nową komórkę kodu**:
   - Kliknij "+" (dodaj komórkę) lub naciśnij Shift+Enter, aby przejść do nowej komórki.
3. **Wpisz i uruchom kod**:
   ```python
   import pandas as pd
   import numpy as np

   # Generowanie syntetycznego zbioru danych
   synthetic_data = pd.DataFrame({
       'size': np.random.uniform(1000, 1000000, 1000),  # Rozmiar pliku (losowe wartości od 1000 do 1M)
       'entropy': np.random.uniform(0, 8, 1000),        # Entropia (losowe wartości od 0 do 8)
       'label': np.random.choice([0, 1], 1000)          # Etykieta: 0=benign, 1=malware
   })
   synthetic_data.to_csv('synthetic_malware.csv', index=False)

   # Wczytywanie danych z Kaggle
   data1 = pd.read_csv('malware_data.csv')  # Upewnij się, że plik jest w Colab

   # Wczytywanie syntetycznego zbioru
   data3 = pd.read_csv('synthetic_malware.csv')

   # Połączenie zbiorów
   data = pd.concat([data1, data3], ignore_index=True)

   # Wyświetlenie pierwszych wierszy (sprawdzenie)
   print(data.head())
   ```
   - **Uwagi**:
     - Jeśli masz też `virusshare_data.csv`, dodaj linię:
       ```python
       data2 = pd.read_csv('virusshare_data.csv')
       ```
       I zmień `pd.concat` na:
       ```python
       data = pd.concat([data1, data2, data3], ignore_index=True)
       ```
     - Jeśli nazwy kolumn w zbiorach się różnią (np. `size` w jednym, a `file_size` w drugim), musisz je ujednolicić przed połączeniem. Napisz mi, jeśli zobaczysz błąd typu "columns don't match", a pomogę Ci to naprawić.

4. **Sprawdź, czy dane się wczytały**:
   - Po uruchomieniu kodu zobaczysz pierwsze wiersze danych (dzięki `print(data.head())`).
   - Powinieneś zobaczyć tabelkę z kolumnami `size`, `entropy`, `label`.

---

#### Krok 2.3: Zapisz notebook i pliki na GitHub
1. Zapisz notebook w Colab:
   - Kliknij "File" → "Save a copy to GitHub".
   - Wybierz repozytorium `DarynaOv/Praca_Inzynierska`.
   - Nazwij plik, np. `wczytywanie_danych.ipynb`.
   - Zapisz (może poprosić o token – użyj tego samego, co wcześniej).
2. Pobierz pliki danych i dodaj je do GitHub:
   - W Colab kliknij prawym przyciskiem na `malware_data.csv`, `synthetic_malware.csv` → "Download".
   - Skopiuj pliki do folderu `Praca_Inzynierska` na swoim komputerze.
   - W terminalu (Git Bash):
     ```
     cd Praca_Inzynierska
     git add malware_data.csv synthetic_malware.csv
     git commit -m "Dodano zbiory danych"
     git push
     ```
   - Sprawdź na GitHub (`https://github.com/DarynaOv/Praca_Inzynierska`), czy pliki się pojawiły.

---

### Czas
- **Pobieranie danych**: 1-2 godziny (Kaggle + ewentualnie VirusShare).
- **Generowanie i wczytywanie danych w Colab**: 1-2 godziny.
- **Zapis na GitHub**: 0.5-1 godzina.
**Razem**: 2-4 godziny (1-2 dni przy 1-2h dziennie).

---

### Co może pójść nie tak?
- **Błąd wczytywania pliku**: Upewnij się, że plik `malware_data.csv` jest w Colab (przeciągnij go do panelu plików).
- **Różne nazwy kolumn**: Jeśli `pd.concat` wyrzuci błąd, sprawdź nazwy kolumn w każdym zbiorze (`print(data1.columns)` itd.) i ujednolić je (np. `data1.rename(columns={'file_size': 'size'}, inplace=True)`).
- **Problemy z GitHub**: Jeśli `git push` nie działa, upewnij się, że używasz tokena (tego samego, co przy `git clone`).



*** pobrałam Android Malware Dataset for Machine Learning ***

Rozumiem, wybrałaś **Android Malware Dataset for Machine Learning** z Kaggle, a po pobraniu pliku ZIP znalazłaś w nim dwa pliki CSV, które mi wysłałaś:

1. **`dataset-features-categories.csv`**: Zawiera listę cech (215 cech) i ich kategorie (np. `SEND_SMS` – Manifest Permission, `TelephonyManager.getDeviceId` – API call signature).
2. **`drebin-215-dataset-5560malware-9476-benign.csv`**: Zawiera dane – 15 036 próbek (5560 malware, 9476 benign), każda próbka opisana 215 cechami binarnymi (0/1) i etykietą `class` (`S` = Malware, `B` = Benign).

Twoim celem w ramach pracy inżynierskiej **"Wykorzystanie uczenia maszynowego do wykrywania złośliwego oprogramowania w analizie śledczej"** jest wczytanie tych danych w Google Colab, połączenie ich z syntetycznym zbiorem (zgodnie z wymaganiami promotora) i przygotowanie do dalszej analizy. Pokażę Ci, jak wykorzystać oba pliki, a potem przejdziemy do następnego kroku.

---

### Jak wykorzystać oba pliki CSV?

#### 1. Zrozumienie roli każdego pliku
- **`dataset-features-categories.csv`**:
  - **Rola**: To plik pomocniczy, który opisuje, co oznaczają cechy w drugim pliku. Każda cecha (kolumna w `drebin-215-dataset-5560malware-9476-benign.csv`) ma swoją kategorię, np. `Manifest Permission`, `API call signature`, `Intent`, `Commands signature`.
  - **Czy wczytujemy?**: Nie musisz wczytywać tego pliku do modelu ML. Służy on do zrozumienia danych (np. podczas pisania części teoretycznej pracy) lub do ewentualnego grupowania cech (np. analiza tylko uprawnień). Na razie go odłóż – nie jest potrzebny do wczytania danych.
  
- **`drebin-215-dataset-5560malware-9476-benign.csv`**:
  - **Rola**: To główny plik z danymi, który wykorzystasz do trenowania modelu ML. Zawiera 215 cech binarnych (0/1) i etykietę `class` dla każdej próbki.
  - **Czy wczytujemy?**: Tak, to ten plik wczytasz w Google Colab i połączysz z syntetycznym zbiorem.

#### 2. Wczytywanie danych w Google Colab
Skupimy się na pliku `drebin-215-dataset-5560malware-9476-benign.csv`, bo zawiera dane do analizy. Plik `dataset-features-categories.csv` możesz wykorzystać później, np. do opisu cech w pracy.

##### Krok 2: Wczytywanie danych (aktualizacja)
1. **Rozpakuj ZIP i prześlij plik do Google Colab**:
   - Rozpakuj pobrany plik ZIP na komputerze (kliknij prawym przyciskiem → "Wypakuj").
   - W Google Colab otwórz swój notebook (z Kroku 1).
   - Kliknij ikonę folderu 📁 po lewej stronie.
   - Przeciągnij plik `drebin-215-dataset-5560malware-9476-benign.csv` do panelu plików (lub kliknij "Upload" i wybierz plik).

2. **Wczytaj dane i wygeneruj syntetyczny zbiór**:
   - Utwórz nową komórkę kodu w Google Colab i wpisz poniższy kod. Kod wczyta dane, wygeneruje syntetyczny zbiór i połączy je.

```python
import pandas as pd
import numpy as np

# Wczytywanie danych z Android Malware Dataset
data1 = pd.read_csv('drebin-215-dataset-5560malware-9476-benign.csv')

# Konwersja etykiet: S (Malware) -> 1, B (Benign) -> 0
data1['class'] = data1['class'].map({'S': 1, 'B': 0})

# Generowanie syntetycznego zbioru danych (1000 próbek)
# Tworzymy syntetyczne dane z takimi samymi kolumnami jak w oryginalnym zbiorze
synthetic_data = pd.DataFrame(
    {col: np.random.choice([0, 1], 1000) for col in data1.columns[:-1]}  # Losowe 0/1 dla wszystkich cech poza 'class'
)
synthetic_data['class'] = np.random.choice([0, 1], 1000)  # Losowe etykiety: 0=benign, 1=malware
synthetic_data.to_csv('synthetic_malware.csv', index=False)

# Wczytywanie syntetycznego zbioru
data3 = pd.read_csv('synthetic_malware.csv')

# Połączenie zbiorów
data = pd.concat([data1, data3], ignore_index=True)

# Wyświetlenie pierwszych wierszy i informacji o zbiorze
print("Pierwsze 5 wierszy danych:")
print(data.head())
print("\nInformacje o zbiorze:")
print(data.info())
print("\nRozkład etykiet (0=benign, 1=malware):")
print(data['class'].value_counts())
```

3. **Uruchom kod**:
   - Kliknij "Run" (trójkąt) w komórce kodu.
   - Wynik pokaże:
     - Pierwsze 5 wierszy połączonego zbioru (215 cech + `class`).
     - Informacje o zbiorze (kolumny, typy danych, brakujące wartości).
     - Rozkład etykiet (ile próbek benign i malware – powinno być 9476 benign + 5560 malware z oryginalnego zbioru + 1000 syntetycznych).

4. **Zapisz notebook i pliki na GitHub**:
   - Zapisz notebook: "File" → "Save a copy to GitHub" → wybierz `DarynaOv/Praca_Inzynierska` → nazwij `wczytywanie_danych.ipynb`.
   - Pobierz pliki (`drebin-215-dataset-5560malware-9476-benign.csv`, `synthetic_malware.csv`) i dodaj do GitHub:
     ```
     cd Praca_Inzynierska
     git add drebin-215-dataset-5560malware-9476-benign.csv synthetic_malware.csv
     git commit -m "Dodano zbiory danych Android Malware i syntetyczny"
     git push
     ```

---

### Jak możesz wykorzystać `dataset-features-categories.csv`?
Ten plik nie jest potrzebny do trenowania modelu, ale może Ci pomóc w pracy:

1. **Opis danych w pracy inżynierskiej**:
   - W części teoretycznej (np. rozdział "Zbiory danych") możesz opisać, jakie cechy analizujesz. Plik `dataset-features-categories.csv` pokazuje, że masz:
     - Uprawnienia (Manifest Permissions), np. `SEND_SMS`, `READ_PHONE_STATE`.
     - Wywołania API (API call signatures), np. `TelephonyManager.getDeviceId`.
     - Intencje (Intents), np. `android.intent.action.BOOT_COMPLETED`.
     - Komendy systemowe (Commands signatures), np. `chmod`.
   - Możesz napisać: "Zbiór danych zawiera 215 cech, w tym uprawnienia (np. SEND_SMS), wywołania API (np. TelephonyManager.getDeviceId) oraz intencje (np. android.intent.action.BOOT_COMPLETED), co pozwala na analizę zachowań aplikacji Android w kontekście wykrywania złośliwego oprogramowania."

2. **Analiza cech (opcjonalnie, później)**:
   - Jeśli promotor poprosi o analizę, możesz pogrupować cechy według kategorii (np. tylko uprawnienia) i sprawdzić, które cechy najlepiej przewidują malware. To wymaga dodatkowej pracy (np. analizy korelacji), więc zrobimy to później, jeśli będzie potrzeba.

Na razie nie wczytuj tego pliku do Colab – skupmy się na danych z `drebin-215-dataset-5560malware-9476-benign.csv`.

---

### Co może pójść nie tak?
- **Błąd wczytywania pliku**: Upewnij się, że plik `drebin-215-dataset-5560malware-9476-benign.csv` jest w panelu plików Colab. Jeśli nazwa pliku się różni (np. zawiera spacje), zmień ją w kodzie (np. `pd.read_csv('drebin 215 dataset.csv')`).
- **Brak pamięci w Colab**: Zbiór ma 15 036 próbek i 215 cech – to niedużo, ale jeśli Colab zgłosi problem z pamięcią, podziel dane na mniejsze części (np. `data1 = data1.sample(frac=0.5)` – weź 50% próbek).
- **Problemy z GitHub**: Jeśli `git push` nie działa, upewnij się, że używasz tokena (tego samego, co przy `git clone`).



NAPOTKANY BŁĄD
Widzę, że napotkałaś błąd `FileNotFoundError` podczas próby wczytania pliku `drebin-215-dataset-5560malware-9476-benign.csv` w Google Colab. Błąd oznacza, że Python (a konkretnie biblioteka Pandas) nie może znaleźć pliku o podanej nazwie w katalogu roboczym Colaba. Rozwiążemy ten problem krok po kroku, a potem upewnimy się, że dane są poprawnie wczytane i gotowe do dalszej pracy.

---
![image](https://github.com/user-attachments/assets/3b6b8749-b31f-4eb7-bdaf-c4103e9c0567)


### Dlaczego pojawił się błąd `FileNotFoundError`?
Błąd `FileNotFoundError` występuje, gdy:
1. Plik nie znajduje się w katalogu roboczym Google Colab.
2. Nazwa pliku w kodzie różni się od rzeczywistej nazwy pliku (np. literówka, dodatkowe spacje, różnice w wielkości liter).
3. Plik nie został poprawnie przesłany do Colaba.

W Twoim przypadku kod próbuje wczytać plik:
```python
data1 = pd.read_csv('drebin-215-dataset-5560malware-9476-benign.csv')
```
Ale Colab nie może go znaleźć.

---

### Rozwiązanie problemu

#### Krok 1: Sprawdź, czy plik jest w Google Colab
1. **Otwórz panel plików w Colab**:
   - Po lewej stronie ekranu kliknij ikonę folderu 📁.
   - Sprawdź, czy widzisz plik `drebin-215-dataset-5560malware-9476-benign.csv`.

2. **Jeśli pliku nie ma**:
   - Upewnij się, że poprawnie przesłałaś plik:
     - Rozpakowałaś ZIP pobrany z Kaggle.
     - Przeciągnęłaś plik `drebin-215-dataset-5560malware-9476-benign.csv` do panelu plików w Colab (lub kliknęłaś "Upload" i wybrałaś plik).
   - Spróbuj przesłać plik ponownie:
     - Kliknij prawym przyciskiem w panelu plików → "Upload".
     - Wybierz plik `drebin-215-dataset-5560malware-9476-benign.csv` z komputera.

3. **Jeśli plik jest, ale nazwa się różni**:
   - Sprawdź dokładną nazwę pliku w panelu plików. Czasem nazwy mogą się różnić, np.:
     - W nazwie może być dodatkowa spacja, np. `drebin 215 dataset-5560malware-9476-benign.csv`.
     - Rozszerzenie może być inne, np. `.CSV` zamiast `.csv` (Colab rozróżnia wielkość liter).
   - Zaktualizuj nazwę pliku w kodzie, aby pasowała do tej w panelu plików. Na przykład, jeśli plik nazywa się `drebin 215 dataset-5560malware-9476-benign.csv`, zmień kod na:
     ```python
     data1 = pd.read_csv('drebin 215 dataset-5560malware-9476-benign.csv')
     ```

#### Krok 2: Sprawdź ścieżkę pliku
Google Colab domyślnie szuka plików w katalogu roboczym (`/content`). Jeśli plik jest w panelu plików, ale w podfolderze (np. `/content/data/`), musisz podać pełną ścieżkę. Na przykład:
```python
data1 = pd.read_csv('/content/data/drebin-215-dataset-5560malware-9476-benign.csv')
```
Jednak jeśli plik jest bezpośrednio w `/content` (czyli w głównym katalogu widocznym w panelu plików), sama nazwa pliku wystarczy.

#### Krok 3: Zaktualizowany kod
Zakładam, że plik jest już w panelu plików i nazywa się dokładnie `drebin-215-dataset-5560malware-9476-benign.csv`. Oto poprawiony kod, który wczyta dane, wygeneruje syntetyczny zbiór i połączy je:

```python
import pandas as pd
import numpy as np

# Wczytywanie danych z Android Malware Dataset
# Upewnij się, że nazwa pliku jest dokładnie taka sama jak w panelu plików
data1 = pd.read_csv('drebin-215-dataset-5560malware-9476-benign.csv')

# Konwersja etykiet: S (Malware) -> 1, B (Benign) -> 0
data1['class'] = data1['class'].map({'S': 1, 'B': 0})

# Generowanie syntetycznego zbioru danych (1000 próbek)
synthetic_data = pd.DataFrame(
    {col: np.random.choice([0, 1], 1000) for col in data1.columns[:-1]}  # Losowe 0/1 dla wszystkich cech poza 'class'
)
synthetic_data['class'] = np.random.choice([0, 1], 1000)  # Losowe etykiety: 0=benign, 1=malware
synthetic_data.to_csv('synthetic_malware.csv', index=False)

# Wczytywanie syntetycznego zbioru
data3 = pd.read_csv('synthetic_malware.csv')

# Połączenie zbiorów
data = pd.concat([data1, data3], ignore_index=True)

# Wyświetlenie pierwszych wierszy i informacji o zbiorze
print("Pierwsze 5 wierszy danych:")
print(data.head())
print("\nInformacje o zbiorze:")
print(data.info())
print("\nRozkład etykiet (0=benign, 1=malware):")
print(data['class'].value_counts())
```

#### Krok 4: Uruchom kod
- Kliknij "Run" (trójkąt) w komórce kodu.
- Jeśli wszystko jest poprawnie, zobaczysz:
  - Pierwsze 5 wierszy danych.
  - Informacje o zbiorze (215 cech + kolumna `class`).
  - Rozkład etykiet (powinno być 9476 benign + 5560 malware z oryginalnego zbioru + ~500 benign i ~500 malware z syntetycznego).

#### Krok 5: Zapisz notebook i pliki na GitHub
- Zapisz notebook: "File" → "Save a copy to GitHub" → wybierz `DarynaOv/Praca_Inzynierska` → nazwij `wczytywanie_danych.ipynb`.
- Pobierz pliki (`drebin-215-dataset-5560malware-9476-benign.csv`, `synthetic_malware.csv`) i dodaj do GitHub:
  ```
  cd Praca_Inzynierska
  git add drebin-215-dataset-5560malware-9476-benign.csv synthetic_malware.csv
  git commit -m "Dodano zbiory danych Android Malware i syntetyczny"
  git push
  ```

---

### Co może pójść nie tak?
- **Nazwa pliku nadal się nie zgadza**: Jeśli błąd się powtarza, kliknij prawym przyciskiem na plik w panelu plików i wybierz "Copy path" (Skopiuj ścieżkę), a potem wklej tę ścieżkę do kodu, np.:
  ```python
  data1 = pd.read_csv('/content/drebin-215-dataset-5560malware-9476-benign.csv')
  ```
- **Plik jest uszkodzony**: Jeśli plik CSV jest uszkodzony (np. zawiera błędne znaki), Pandas może zgłosić inny błąd. W takim przypadku otwórz plik na komputerze (np. w Excelu) i sprawdź, czy wygląda poprawnie.
- **Za mało pamięci**: Zbiór ma 15 036 próbek i 215 cech – to niedużo, ale jeśli Colab zgłosi problem z pamięcią, weź tylko część danych (np. `data1 = data1.sample(frac=0.5)` – 50% próbek).















Ponownie napotkałaś błąd `FileNotFoundError` podczas próby wczytania pliku `drebin-215-dataset-5560malware-9476-benign.csv` w Google Colab. Widzę jednak, że plik jest widoczny w panelu plików po lewej stronie (`drebin-215-dataset-5560malware-9476-benign.csv`), więc problem nie leży w braku pliku, ale w sposobie, w jaki próbujesz go wczytać. Rozwiążemy to krok po kroku.

---

![image](https://github.com/user-attachments/assets/bc5c2d85-f644-419f-8da9-4345b1324743)

### Dlaczego pojawił się błąd `FileNotFoundError`?
Błąd `FileNotFoundError: [Errno 2] No such file or directory: 'drebin-215-dataset-5560malware-9476-benign.csv'` oznacza, że Python nie może znaleźć pliku w katalogu roboczym Colaba, mimo że plik jest widoczny w panelu plików. Możliwe przyczyny:
1. **Ścieżka pliku jest nieprawidłowa**: Colab domyślnie szuka plików w katalogu `/content`, ale jeśli plik jest w podfolderze lub ścieżka nie jest poprawnie podana, pojawi się błąd.
2. **Plik nie został poprawnie przesłany**: Colab mógł nie zakończyć przesyłania pliku, co zdarza się przy większych plikach.
3. **Różnice w nazwie pliku**: Nazwa pliku w kodzie musi być identyczna z nazwą w panelu plików (uwzględniając wielkość liter i spacje).

W Twoim przypadku plik jest w panelu plików, więc problem leży prawdopodobnie w ścieżce lub w tym, że Colab nie widzi pliku w katalogu roboczym.

---

### Rozwiązanie problemu

#### Krok 1: Sprawdź ścieżkę pliku
Plik `drebin-215-dataset-5560malware-9476-benign.csv` jest widoczny w panelu plików, co oznacza, że znajduje się w katalogu `/content`. Twój kod próbuje wczytać plik za pomocą:
```python
data1 = pd.read_csv('drebin-215-dataset-5560malware-9476-benign.csv')
```
To zakłada, że plik jest bezpośrednio w `/content`. Jednak czasami Colab może mieć problem z dostępem do pliku, jeśli nie określisz pełnej ścieżki lub jeśli plik nie został poprawnie załadowany do pamięci.

Aby to naprawić, użyjemy pełnej ścieżki do pliku:
1. Kliknij prawym przyciskiem na plik `drebin-215-dataset-5560malware-9476-benign.csv` w panelu plików.
2. Wybierz "Copy path" (Skopiuj ścieżkę).
3. Wklej tę ścieżkę do kodu. Ścieżka powinna wyglądać tak:
   ```
   /content/drebin-215-dataset-5560malware-9476-benign.csv
   ```

#### Krok 2: Zaktualizuj kod
Zmodyfikuj kod, aby używał pełnej ścieżki do pliku. Oto poprawiona wersja:

```python
import pandas as pd
import numpy as np

# Wczytywanie danych z Android Malware Dataset z pełną ścieżką
data1 = pd.read_csv('/content/drebin-215-dataset-5560malware-9476-benign.csv')

# Konwersja etykiet: S (Malware) -> 1, B (Benign) -> 0
data1['class'] = data1['class'].map({'S': 1, 'B': 0})

# Generowanie syntetycznego zbioru danych (1000 próbek)
synthetic_data = pd.DataFrame(
    {col: np.random.choice([0, 1], 1000) for col in data1.columns[:-1]}  # Losowe 0/1 dla wszystkich cech poza 'class'
)
synthetic_data['class'] = np.random.choice([0, 1], 1000)  # Losowe etykiety: 0=benign, 1=malware
synthetic_data.to_csv('synthetic_malware.csv', index=False)

# Wczytywanie syntetycznego zbioru
data3 = pd.read_csv('synthetic_malware.csv')

# Połączenie zbiorów
data = pd.concat([data1, data3], ignore_index=True)

# Wyświetlenie pierwszych wierszy i informacji o zbiorze
print("Pierwsze 5 wierszy danych:")
print(data.head())
print("\nInformacje o zbiorze:")
print(data.info())
print("\nRozkład etykiet (0=benign, 1=malware):")
print(data['class'].value_counts())
```

#### Krok 3: Uruchom kod
- Kliknij "Run" (trójkąt) w komórce kodu.
- Jeśli wszystko działa poprawnie, zobaczysz:
  - Pierwsze 5 wierszy danych (215 cech + kolumna `class`).
  - Informacje o zbiorze (kolumny, typy danych, brakujące wartości).
  - Rozkład etykiet (powinno być 9476 benign + 5560 malware z oryginalnego zbioru + ~500 benign i ~500 malware z syntetycznego).

#### Krok 4: Zapisz notebook i pliki na GitHub
- Zapisz notebook: "File" → "Save a copy to GitHub" → wybierz `DarynaOv/Praca_Inzynierska` → nazwij `wczytywanie_danych.ipynb`.
- Pobierz pliki (`drebin-215-dataset-5560malware-9476-benign.csv`, `synthetic_malware.csv`) i dodaj do GitHub:
  ```
  cd Praca_Inzynierska
  git add drebin-215-dataset-5560malware-9476-benign.csv synthetic_malware.csv
  git commit -m "Dodano zbiory danych Android Malware i syntetyczny"
  git push
  ```

---

### Co może pójść nie tak?
1. **Plik nie został poprawnie przesłany**:
   - Usuń plik z panelu plików (kliknij prawym przyciskiem → "Delete").
   - Prześlij go ponownie: przeciągnij plik `drebin-215-dataset-5560malware-9476-benign.csv` do panelu plików.
   - Upewnij się, że przesyłanie się zakończyło (nie przerywaj, dopóki pasek postępu nie zniknie).

2. **Plik jest uszkodzony**:
   - Jeśli po poprawieniu ścieżki pojawi się inny błąd (np. `ParserError`), plik może być uszkodzony.
   - Otwórz plik na komputerze (np. w Excelu lub Notatniku) i sprawdź, czy wygląda poprawnie (czy ma 215 kolumn i 15 036 wierszy).
   - Jeśli plik jest uszkodzony, pobierz go ponownie z Kaggle.

3. **Za mało pamięci**:
   - Zbiór ma 15 036 próbek i 215 cech – to niedużo, ale jeśli Colab zgłosi problem z pamięcią, weź tylko część danych:
     ```python
     data1 = data1.sample(frac=0.5)  # Weź 50% próbek
     ```



     ![image](https://github.com/user-attachments/assets/7026e5aa-adcb-43fe-8f77-a2214155d047)

     Ponownie napotkałaś błąd `FileNotFoundError` podczas próby wczytania pliku `drebin-215-dataset-5560malware-9476-benign.csv` w Google Colab. Jednak zauważyłam, że plik nie znajduje się w panelu plików w katalogu `/content`, gdzie Colab domyślnie szuka plików. Zamiast tego, plik jest w podfolderze `/content/sample_data/`. To właśnie powoduje błąd – kod szuka pliku w złym miejscu. Rozwiążemy to krok po kroku.

---

### Dlaczego pojawił się błąd `FileNotFoundError`?
Błąd `FileNotFoundError: [Errno 2] No such file or directory: 'drebin-215-dataset-5560malware-9476-benign.csv'` oznacza, że Python (Pandas) nie może znaleźć pliku w katalogu roboczym `/content`. Twój kod:
```python
data1 = pd.read_csv('drebin-215-dataset-5560malware-9476-benign.csv')
```
zakłada, że plik znajduje się bezpośrednio w `/content`, ale w panelu plików widzę, że plik `drebin-215-dataset-5560malware-9476-benign.csv` jest w podfolderze `/content/sample_data/`.

---

### Rozwiązanie problemu

#### Krok 1: Popraw ścieżkę pliku w kodzie
Ponieważ plik znajduje się w podfolderze `/content/sample_data/`, musisz zaktualizować ścieżkę w kodzie, aby wskazywała na właściwe miejsce. Zamiast:
```python
data1 = pd.read_csv('drebin-215-dataset-5560malware-9476-benign.csv')
```
użyj pełnej ścieżki:
```python
data1 = pd.read_csv('/content/sample_data/drebin-215-dataset-5560malware-9476-benign.csv')
```

#### Krok 2: Zaktualizuj kod
Oto poprawiona wersja kodu, która uwzględnia poprawną ścieżkę do pliku:

```python
import pandas as pd
import numpy as np

# Wczytywanie danych z Android Malware Dataset z poprawioną ścieżką
data1 = pd.read_csv('/content/sample_data/drebin-215-dataset-5560malware-9476-benign.csv')

# Konwersja etykiet: S (Malware) -> 1, B (Benign) -> 0
data1['class'] = data1['class'].map({'S': 1, 'B': 0})

# Generowanie syntetycznego zbioru danych (1000 próbek)
synthetic_data = pd.DataFrame(
    {col: np.random.choice([0, 1], 1000) for col in data1.columns[:-1]}  # Losowe 0/1 dla wszystkich cech poza 'class'
)
synthetic_data['class'] = np.random.choice([0, 1], 1000)  # Losowe etykiety: 0=benign, 1=malware
synthetic_data.to_csv('synthetic_malware.csv', index=False)

# Wczytywanie syntetycznego zbioru
data3 = pd.read_csv('synthetic_malware.csv')

# Połączenie zbiorów
data = pd.concat([data1, data3], ignore_index=True)

# Wyświetlenie pierwszych wierszy i informacji o zbiorze
print("Pierwsze 5 wierszy danych:")
print(data.head())
print("\nInformacje o zbiorze:")
print(data.info())
print("\nRozkład etykiet (0=benign, 1=malware):")
print(data['class'].value_counts())
```

#### Krok 3: Uruchom kod
- Kliknij "Run" (trójkąt) w komórce kodu.
- Jeśli wszystko działa poprawnie, zobaczysz:
  - Pierwsze 5 wierszy danych (215 cech + kolumna `class`).
  - Informacje o zbiorze (kolumny, typy danych, brakujące wartości).
  - Rozkład etykiet (powinno być 9476 benign + 5560 malware z oryginalnego zbioru + ~500 benign i ~500 malware z syntetycznego).

#### Krok 4: Zapisz notebook i pliki na GitHub
- Zapisz notebook: "File" → "Save a copy to GitHub" → wybierz `DarynaOv/Praca_Inzynierska` → nazwij `wczytywanie_danych.ipynb`.
- Pobierz pliki (`drebin-215-dataset-5560malware-9476-benign.csv`, `synthetic_malware.csv`) i dodaj do GitHub:
  ```
  cd Praca_Inzynierska
  git add drebin-215-dataset-5560malware-9476-benign.csv synthetic_malware.csv
  git commit -m "Dodano zbiory danych Android Malware i syntetyczny"
  git push
  ```
**Uwaga**: Plik `drebin-215-dataset-5560malware-9476-benign.csv` jest w `/content/sample_data/`, więc musisz go najpierw przenieść lub pobrać z Colaba:
- Kliknij prawym przyciskiem na plik w panelu plików → "Download".
- Prześlij go do swojego repozytorium GitHub.

---

### Alternatywa: Przenieś plik do katalogu `/content`
Jeśli nie chcesz używać pełnej ścieżki w kodzie, możesz przenieść plik z `/content/sample_data/` do `/content`:
1. Użyj polecenia w Colab, aby przenieść plik:
   ```python
   !mv /content/sample_data/drebin-215-dataset-5560malware-9476-benign.csv /content/
   ```
2. Sprawdź, czy plik jest teraz w `/content` (powinien zniknąć z `sample_data` i pojawić się w głównym katalogu w panelu plików).
3. Użyj oryginalnego kodu bez zmiany ścieżki:
   ```python
   data1 = pd.read_csv('drebin-215-dataset-5560malware-9476-benign.csv')
   ```

---

### Co może pójść nie tak?
1. **Plik jest uszkodzony**:
   - Jeśli po poprawieniu ścieżki pojawi się inny błąd (np. `ParserError`), plik może być uszkodzony.
   - Otwórz plik na komputerze (np. w Excelu lub Notatniku) i sprawdź, czy wygląda poprawnie (czy ma 215 kolumn i 15 036 wierszy).
   - Jeśli plik jest uszkodzony, pobierz go ponownie z Kaggle.

2. **Za mało pamięci**:
   - Zbiór ma 15 036 próbek i 215 cech – to niedużo, ale jeśli Colab zgłosi problem z pamięcią, weź tylko część danych:
     ```python
     data1 = data1.sample(frac=0.5)  # Weź 50% próbek
     ```

![image](https://github.com/user-attachments/assets/33876e08-dbf9-41b0-ac97-0a13d4a95a4c)
Widzę, że kod w Google Colab działa poprawnie – dane z pliku `drebin-215-dataset-5560malware-9476-benign.csv` zostały wczytane, syntetyczny zbiór został wygenerowany, a oba zbiory zostały połączone. Wynik pokazuje:
- **Pierwsze 5 wierszy danych**: Dane mają 215 cech binarnych (0/1) oraz kolumnę `class` (0 = benign, 1 = malware).
- **Informacje o zbiorze**: Zbiór ma 16 036 wierszy (15 036 z oryginalnego zbioru + 1000 syntetycznych) i 216 kolumn (215 cech + `class`).
- **Rozkład etykiet**: 9951 próbek benign (`0`) i 6085 próbek malware (`1`), co jest zgodne z oczekiwaniami (oryginalny zbiór miał 9476 benign i 5560 malware, a syntetyczny dodaje ~500 benign i ~500 malware).

Teraz możemy przejść do **Kroku 3: Standaryzacja i normalizacja**, aby przygotować dane do trenowania modelu ML (np. Random Forest, który planujesz użyć w pracy inżynierskiej). Pokażę Ci, jak to zrobić w Google Colab.

---

### Krok 3: Standaryzacja i normalizacja danych

#### Dlaczego potrzebujemy standaryzacji/normalizacji?
- **Standaryzacja** (przeskalowanie danych do średniej 0 i odchylenia standardowego 1) oraz **normalizacja** (przeskalowanie danych do zakresu [0,1]) są ważne w uczeniu maszynowym, ponieważ:
  - Wiele algorytmów ML (np. SVM, sieci neuronowe) jest wrażliwych na różnice w skali cech.
  - W Twoim przypadku cechy są binarne (0/1), więc normalizacja nie jest konieczna, ale standaryzacja może pomóc w stabilności modelu, szczególnie jeśli później dodasz inne zbiory danych (np. API Call Sequences).
- Random Forest, który planujesz użyć, nie wymaga standaryzacji, bo opiera się na drzewach decyzyjnych, które są niewrażliwe na skalę cech. Jednak dla porządku (i na wypadek, gdyby promotor wymagał standaryzacji) pokażę Ci, jak to zrobić.

#### Jak przeprowadzić standaryzację?
Użyjemy `StandardScaler` z biblioteki `scikit-learn`, aby przeskalować cechy (bez kolumny `class`, bo to etykiety).

1. **Dodaj nową komórkę kodu w Google Colab**:
   - W swoim notebooku (`wczytywanie_danych.ipynb`) kliknij "+ Code" poniżej ostatniego wyniku.
   - Wpisz poniższy kod, który oddzieli cechy od etykiet, przeskaluje cechy i połączy je z powrotem.

```python
from sklearn.preprocessing import StandardScaler

# Oddziel cechy (X) od etykiet (y)
X = data.drop('class', axis=1)  # Wszystkie kolumny poza 'class'
y = data['class']  # Kolumna 'class'

# Standaryzacja cech
scaler = StandardScaler()
X_scaled = scaler.fit_transform(X)

# Konwersja przeskalowanych danych z powrotem na DataFrame
X_scaled_df = pd.DataFrame(X_scaled, columns=X.columns)

# Połączenie przeskalowanych cech z etykietami
data_scaled = pd.concat([X_scaled_df, y.reset_index(drop=True)], axis=1)

# Wyświetlenie pierwszych wierszy przeskalowanych danych
print("Pierwsze 5 wierszy przeskalowanych danych:")
print(data_scaled.head())

# Sprawdzenie, czy rozkład etykiet się nie zmienił
print("\nRozkład etykiet po standaryzacji (0=benign, 1=malware):")
print(data_scaled['class'].value_counts())
```

2. **Uruchom kod**:
   - Kliknij "Run" (trójkąt) w komórce kodu.
   - Wynik pokaże:
     - Pierwsze 5 wierszy przeskalowanych danych (cechy będą miały wartości w skali z średnią 0 i odchyleniem standardowym 1).
     - Rozkład etykiet (powinien być taki sam jak wcześniej: 9951 benign, 6085 malware).

3. **Zapisz notebook na GitHub**:
   - Zaktualizuj notebook: "File" → "Save a copy to GitHub" → wybierz `DarynaOv/Praca_Inzynierska` → nadpisz `wczytywanie_danych.ipynb`.

---
