
### Krok 1: Przygotowanie środowiska (Dzień 1-2, 2-4 godziny)
Musisz skonfigurować narzędzia, których użyjesz. Wszystko będzie online, żebyś nie musiała nic instalować.

#### 1.1. Google Colab (środowisko do Pythona)
- Wejdź na [Google Colab](https://colab.research.google.com/).
- Zaloguj się na konto Google.
- Utwórz nowy notebook: Kliknij "File" → "New Notebook".
- To Twoje środowisko – tu będziesz pisać kod.

#### 1.2. GitHub (zarządzanie projektem)
- Załóż konto na [GitHub](https://github.com/).
- Utwórz nowe repozytorium: Kliknij "+" → "New Repository".
  - Nazwa: `malware-detection`.
  - Ustaw jako publiczne.
  - Zainicjalizuj z README.
- Zainstaluj Git (na komputerze):
  - Pobierz z [git-scm.com](https://git-scm.com/downloads).
  - Zainstaluj (proste "Next, Next").
- Połącz Git z GitHub:
  - Otwórz terminal (w Windows: "Git Bash").
  - Skonfiguruj:  
    ```
    git config --global user.name "TwojeImie"
    git config --global user.email "twoj.email@example.com"
    ```
  - Sklonuj repozytorium:  
    ```
    git clone https://github.com/twoje-konto/malware-detection.git
    ```
  - Przejdź do folderu:  
    ```
    cd malware-detection
    ```

#### 1.3. Azure (prosta aplikacja)
- Załóż darmowe konto na [Azure](https://azure.microsoft.com/en-us/free/).
  - Użyj konta studenckiego (daje 100 USD kredytu).
- Na razie tylko konto – aplikację zrobimy później.

**Czas**: 2-4 godziny (1-2 dni przy 1-2h dziennie).

---

### Krok 2: Wczytywanie danych (Dzień 3-4, 2-4 godziny)
Promotor wymaga kilku różnych zbiorów danych, w tym syntetycznego.

#### 2.1. Pobranie danych
- **Zbiór 1 (Kaggle)**:  
  - Wejdź na [Kaggle](https://www.kaggle.com/datasets).
  - Wyszukaj "Malware Classification Dataset" (np. "Malware Analysis Dataset").
  - Pobierz plik CSV (np. `malware_data.csv`) – zawiera cechy plików (rozmiar, entropia, etykiety: 0=benign, 1=malware).
  - Zapisz na Dysku Google.
- **Zbiór 2 (VirusShare)**:  
  - Wejdź na [VirusShare](https://virusshare.com/) (potrzebujesz konta – darmowe).
  - Pobierz mały zbiór próbek (np. 100 plików).
  - Wyodrębnij proste cechy (rozmiar, entropia) – możesz użyć gotowego skryptu z internetu (np. [ten tutorial](https://www.kaggle.com/code/naumanmohd/malware-detection-using-machine-learning)).  
  - Zapisz jako `virusshare_data.csv`.
- **Zbiór 3 (Syntetyczny)**:  
  - Wygenerujesz w Pythonie.

#### 2.2. Wczytywanie danych w Google Colab
- W Colab kliknij ikonę folderu (po lewej).
- Przeciągnij `malware_data.csv` i `virusshare_data.csv` z Dysku Google.
- Wygeneruj syntetyczny zbiór:  
  ```python
  import pandas as pd
  import numpy as np

  # Generowanie syntetycznych danych
  synthetic_data = pd.DataFrame({
      'size': np.random.uniform(1000, 1000000, 1000),  # Rozmiar pliku
      'entropy': np.random.uniform(0, 8, 1000),        # Entropia
      'label': np.random.choice([0, 1], 1000)          # 0=benign, 1=malware
  })
  synthetic_data.to_csv('synthetic_malware.csv', index=False)

  # Wczytywanie danych
  data1 = pd.read_csv('malware_data.csv')
  data2 = pd.read_csv('virusshare_data.csv')
  data3 = pd.read_csv('synthetic_malware.csv')

  # Połączenie zbiorów (jeśli mają te same kolumny)
  data = pd.concat([data1, data2, data3], ignore_index=True)
  ```

**Czas**: 2-4 godziny (pobieranie + kod).

---

### Krok 3: Standaryzacja i normalizacja (Dzień 5, 1-2 godziny)
Promotor wymaga standaryzacji i normalizacji danych.

- **Kod w Colab**:
  ```python
  from sklearn.preprocessing import StandardScaler, MinMaxScaler

  # Oddziel cechy (X) i etykiety (y)
  X = data[['size', 'entropy']]  # Zakładam, że masz te kolumny
  y = data['label']

  # Standaryzacja (średnia=0, odchylenie=1)
  scaler = StandardScaler()
  X_standardized = scaler.fit_transform(X)

  # Normalizacja (zakres [0,1])
  normalizer = MinMaxScaler()
  X_normalized = normalizer.fit_transform(X_standardized)

  # Zamiana z powrotem na DataFrame
  X_normalized = pd.DataFrame(X_normalized, columns=['size', 'entropy'])
  ```

**Czas**: 1-2 godziny (nauka + kod).

---

### Krok 4: Budowa modelu ensemble (Dzień 6-8, 3-6 godzin)
Promotor wymaga metod ensemble i prostych obliczeń (metryk).

#### 4.1. Przygotowanie danych
- **Kod**:
  ```python
  from sklearn.model_selection import train_test_split

  # Podział na zbiór treningowy i testowy
  X_train, X_test, y_train, y_test = train_test_split(X_normalized, y, test_size=0.2, random_state=42)
  ```

#### 4.2. Model ensemble
- Użyjemy Random Forest i Voting Classifier (łączy kilka modeli).
- **Kod**:
  ```python
  from sklearn.ensemble import RandomForestClassifier, VotingClassifier
  from sklearn.svm import SVC
  from sklearn.naive_bayes import GaussianNB
  from sklearn.metrics import accuracy_score, precision_score, recall_score, f1_score

  # Definicja modeli
  rf = RandomForestClassifier(n_estimators=100, random_state=42)
  svm = SVC(probability=True, random_state=42)
  nb = GaussianNB()

  # Ensemble: Voting Classifier
  ensemble = VotingClassifier(estimators=[('rf', rf), ('svm', svm), ('nb', nb)], voting='soft')

  # Trening
  ensemble.fit(X_train, y_train)

  # Predykcja
  y_pred = ensemble.predict(X_test)

  # Proste obliczenia (metryki)
  print("Accuracy:", accuracy_score(y_test, y_pred))
  print("Precision:", precision_score(y_test, y_pred))
  print("Recall:", recall_score(y_test, y_pred))
  print("F1-score:", f1_score(y_test, y_pred))
  ```

**Czas**: 3-6 godzin (nauka + kod).

---

### Krok 5: Wykresy treningu i walidacji (Dzień 9-10, 2-4 godziny)
Promotor chce wykresy jak w załączonym obrazie (trening vs walidacja). Random Forest nie ma epok jak sieci neuronowe, więc pokażemy accuracy w zależności od liczby drzew (`n_estimators`).

- **Kod**:
  ```python
  import matplotlib.pyplot as plt

  # Listy do przechowywania wyników
  train_accuracies = []
  test_accuracies = []
  n_estimators_range = range(10, 110, 10)

  # Obliczanie accuracy dla różnej liczby drzew
  for n in n_estimators_range:
      rf = RandomForestClassifier(n_estimators=n, random_state=42)
      rf.fit(X_train, y_train)
      train_accuracies.append(accuracy_score(y_train, rf.predict(X_train)))
      test_accuracies.append(accuracy_score(y_test, rf.predict(X_test)))

  # Wykres
  plt.plot(n_estimators_range, train_accuracies, label='Train Accuracy')
  plt.plot(n_estimators_range, test_accuracies, label='Validation Accuracy')
  plt.xlabel('Number of Trees')
  plt.ylabel('Accuracy')
  plt.legend()
  plt.savefig('plot_accuracy.png')
  ```

- Pobierz plik `plot_accuracy.png` z Colab (kliknij prawym przyciskiem → "Download").

**Czas**: 2-4 godziny.

---

### Krok 6: Prosta aplikacja na Azure (Dzień 11-14, 4-8 godzin)
Promotor wymaga aplikacji na Azure. Zrobimy prostą apkę we Flask, która wyświetla wynik modelu.

#### 6.1. Kod aplikacji (Flask)
- W Colab utwórz nowy plik `app.py`:
  ```python
  from flask import Flask
  import pickle
  import pandas as pd
  from sklearn.ensemble import RandomForestClassifier

  app = Flask(__name__)

  # Zapisz model do pliku
  rf = RandomForestClassifier(n_estimators=100, random_state=42)
  rf.fit(X_train, y_train)
  with open('model.pkl', 'wb') as f:
      pickle.dump(rf, f)

  # Wczytaj model
  with open('model.pkl', 'rb') as f:
      model = pickle.load(f)

  @app.route('/')
  def predict():
      # Przykładowe dane do predykcji
      sample_data = pd.DataFrame([[50000, 6.5]], columns=['size', 'entropy'])
      prediction = model.predict(sample_data)[0]
      prob = model.predict_proba(sample_data)[0][1]
      result = "Malware" if prediction == 1 else "Benign"
      return f"Result: {result} (Probability: {prob:.2f})"

  if __name__ == "__main__":
      app.run(host='0.0.0.0', port=5000)
  ```

#### 6.2. Wdrożenie na Azure
- Pobierz `app.py` i `model.pkl` z Colab.
- Zainstaluj Azure CLI ([instrukcja](https://docs.microsoft.com/en-us/cli/azure/install-azure-cli)).
- W terminalu:
  ```
  az login  # Zaloguj się
  az webapp up --name malware-detection-app --resource-group myResourceGroup --sku FREE --runtime "PYTHON|3.9"
  ```
- Wgraj pliki:
  - Skopiuj `app.py` i `model.pkl` do folderu `malware-detection`.
  - W terminalu:
    ```
    git add app.py model.pkl
    git commit -m "Add Flask app"
    git push
    ```
- Sprawdź apkę: Wejdź na `https://malware-detection-app.azurewebsites.net/`.

**Czas**: 4-8 godzin.

---

### Krok 7: GitHub i zarządzanie projektem (Dzień 15-16, 2-4 godziny)
Promotor chce, aby praca była na GitHub.

- Dodaj pliki do repozytorium:
  - `malware_detection.py` (cały kod: wczytywanie, standaryzacja, model, wykresy).
  - `plot_accuracy.png`.
  - Dane: `malware_data.csv`, `virusshare_data.csv`, `synthetic_malware.csv`.
- W terminalu:
  ```
  git add .
  git commit -m "Add project files"
  git push
  ```
- Wyślij promotorowi link do repozytorium (np. `github.com/twoje-konto/malware-detection`).

**Czas**: 2-4 godziny.

---

### Krok 8: Pisanie pracy (Dzień 17-28, 12-24 godziny)
Teraz napiszesz pracę (30-40 stron).

#### 8.1. Struktura pracy
- **Wstęp (2-3 strony)**:  
  Cel: Wykrywanie malware za pomocą ML (ensemble). Zakres: Wczytywanie danych, standaryzacja, model, wykresy, aplikacja na Azure.
- **Przegląd literatury (5-7 stron)**:  
  - Malware i jego zagrożenia.  
  - ML w cyberbezpieczeństwie (artykuły z Google Scholar).  
  - Metody ensemble (np. Random Forest).  
- **Metodyka (10-12 stron)**:  
  - Wczytywanie danych (opis zbiorów).  
  - Standaryzacja i normalizacja (opis kodu).  
  - Model ensemble (Random Forest + Voting Classifier).  
  - Wykresy (trening vs walidacja).  
  - Apka na Azure.  
- **Wyniki i analiza (5-7 stron)**:  
  - Metryki (accuracy, precision itd.).  
  - Wykres (`plot_accuracy.png`).  
- **Wnioski (2-3 strony)**:  
  - Podsumowanie: "Model osiągnął 90% accuracy".  
  - Ograniczenia: "Brak analizy dynamicznej".  
- **Bibliografia (1-2 strony)**:  
  - Artykuły, dokumentacje, tutoriale.

#### 8.2. Jak pisać?
- Używaj prostych zdań.  
- Kopiuj fragmenty z tutoriali (ale podawaj źródła).  
- Wklej wykresy i fragmenty kodu jako zrzuty ekranu.

**Czas**: 12-24 godziny (2 tygodnie przy 1-2h dziennie).

---

### Krok 9: Finalizacja (Dzień 29-30, 2-4 godziny)
- Zapisz pracę jako PDF (`malware_detection.pdf`).
- Dodaj do GitHub:
  ```
  git add malware_detection.pdf
  git commit -m "Add final thesis"
  git push
  ```
- Wyślij promotorowi link do repozytorium.

**Czas**: 2-4 godziny.

---

### Pełny kod (malware_detection.py)
Oto kompletny kod, który możesz zapisać jako `malware_detection.py`:

```python
import pandas as pd
import numpy as np
from sklearn.preprocessing import StandardScaler, MinMaxScaler
from sklearn.model_selection import train_test_split
from sklearn.ensemble import RandomForestClassifier, VotingClassifier
from sklearn.svm import SVC
from sklearn.naive_bayes import GaussianNB
from sklearn.metrics import accuracy_score, precision_score, recall_score, f1_score
import matplotlib.pyplot as plt

# 1. Generowanie syntetycznego zbioru danych
synthetic_data = pd.DataFrame({
    'size': np.random.uniform(1000, 1000000, 1000),
    'entropy': np.random.uniform(0, 8, 1000),
    'label': np.random.choice([0, 1], 1000)
})
synthetic_data.to_csv('synthetic_malware.csv', index=False)

# 2. Wczytywanie danych (załóż, że masz malware_data.csv i virusshare_data.csv)
data1 = pd.read_csv('malware_data.csv')
data2 = pd.read_csv('virusshare_data.csv')
data3 = pd.read_csv('synthetic_malware.csv')
data = pd.concat([data1, data2, data3], ignore_index=True)

# 3. Standaryzacja i normalizacja
X = data[['size', 'entropy']]
y = data['label']
scaler = StandardScaler()
X_standardized = scaler.fit_transform(X)
normalizer = MinMaxScaler()
X_normalized = normalizer.fit_transform(X_standardized)
X_normalized = pd.DataFrame(X_normalized, columns=['size', 'entropy'])

# 4. Podział na zbiór treningowy i testowy
X_train, X_test, y_train, y_test = train_test_split(X_normalized, y, test_size=0.2, random_state=42)

# 5. Model ensemble
rf = RandomForestClassifier(n_estimators=100, random_state=42)
svm = SVC(probability=True, random_state=42)
nb = GaussianNB()
ensemble = VotingClassifier(estimators=[('rf', rf), ('svm', svm), ('nb', nb)], voting='soft')
ensemble.fit(X_train, y_train)
y_pred = ensemble.predict(X_test)

# 6. Proste obliczenia (metryki)
print("Accuracy:", accuracy_score(y_test, y_pred))
print("Precision:", precision_score(y_test, y_pred))
print("Recall:", recall_score(y_test, y_pred))
print("F1-score:", f1_score(y_test, y_pred))

# 7. Wykresy treningu i walidacji
train_accuracies = []
test_accuracies = []
n_estimators_range = range(10, 110, 10)
for n in n_estimators_range:
    rf = RandomForestClassifier(n_estimators=n, random_state=42)
    rf.fit(X_train, y_train)
    train_accuracies.append(accuracy_score(y_train, rf.predict(X_train)))
    test_accuracies.append(accuracy_score(y_test, rf.predict(X_test)))
plt.plot(n_estimators_range, train_accuracies, label='Train Accuracy')
plt.plot(n_estimators_range, test_accuracies, label='Validation Accuracy')
plt.xlabel('Number of Trees')
plt.ylabel('Accuracy')
plt.legend()
plt.savefig('plot_accuracy.png')
```

---

### Podsumowanie czasu
- **Dni 1-16**: Kod, wykresy, aplikacja, GitHub (16-32 godziny).  
- **Dni 17-28**: Pisanie pracy (12-24 godziny).  
- **Dni 29-30**: Finalizacja (2-4 godziny).  
**Razem**: 30-60 godzin (miesiąc przy 1-2h dziennie).
