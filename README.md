# Praca_Inzynierska

temat **"Wykorzystanie uczenia maszynowego do wykrywania złośliwego oprogramowania w analizie śledczej"**

#### 1. Wstęp (2-3 strony)
- **Cel pracy**:  
  Zaprojektowanie i przetestowanie systemu opartego na uczeniu maszynowym do wykrywania złośliwego oprogramowania w analizie śledczej, z uwzględnieniem wczytywania i standaryzacji danych, metod ensemble, wizualizacji wyników (wykresy treningu i walidacji) oraz prostych obliczeń. Projekt obejmuje różne zbiory danych (w tym syntetyczne), aplikację na Azure i zarządzanie kodem na GitHub.
- **Uzasadnienie**:  
  - Malware to poważne zagrożenie w cyberbezpieczeństwie.  
  - Uczenie maszynowe (zwłaszcza metody ensemble) może poprawić skuteczność wykrywania.  
  - Automatyzacja analizy śledczej przyspiesza dochodzenia.  
- **Zakres pracy**:  
  - Wczytywanie i standaryzacja danych z różnych zbiorów (w tym syntetycznych).  
  - Budowa modelu ML z użyciem metod ensemble.  
  - Wizualizacja wyników (wykresy treningu i walidacji).  
  - Prosta aplikacja na Azure.  
  - Zarządzanie projektem na GitHub.

#### 2. Przegląd literatury (5-7 stron)
- **Cel**:  
  Walidacja literaturą (zgodnie z wymogiem promotora).  
- **Zakres**:  
  - Złośliwe oprogramowanie: rodzaje i zagrożenia (np. wirusy, ransomware).  
  - Tradycyjne metody wykrywania (sygnatury) vs uczenie maszynowe.  
  - Metody ensemble w ML (np. Random Forest, Voting Classifier).  
  - Informatyka śledcza: analiza plików w dochodzeniach.  
- **Źródła**:  
  - Artykuły z Google Scholar (np. "machine learning malware detection").  
  - Dokumentacje bibliotek (Scikit-learn).  

#### 3. Metodyka (10-12 stron)
- **Cel**:  
  Opisanie, co i jak zrobiłaś – krok po kroku.  
- **Zakres**:  
  - **Wczytywanie danych**:  
    - Kilka różnych zbiorów danych (np. "Malware Classification Dataset" z Kaggle, "VirusShare").  
    - Zbiór syntetyczny: Wygenerujesz własne dane (np. w Pythonie, losowe cechy plików).  
    - Użyjesz Pandas do wczytania danych w formacie CSV.  
  - **Standaryzacja i normalizacja**:  
    - Standaryzacja: Przekształcisz cechy (np. rozmiar pliku, entropia) na wartości o średniej 0 i odchyleniu 1 (Scikit-learn: `StandardScaler`).  
    - Normalizacja: Przeskalujesz dane do zakresu [0,1] (`MinMaxScaler`).  
  - **Model ML (ensemble)**:  
    - Użyjesz metod ensemble (np. Random Forest i Voting Classifier łączący kilka modeli, np. SVM + Naive Bayes).  
    - Podział danych: 80% trening, 20% test.  
  - **Proste obliczenia**:  
    - Obliczysz metryki: accuracy, precision, recall, F1-score (gotowe w Scikit-learn).  
  - **Wykresy**:  
    - Wykres treningu i walidacji (jak w załączonym obrazie) – użyjesz Matplotlib do pokazania accuracy modelu w czasie (po epokach).  
  - **Technologie**:  
    - Python (Pandas, NumPy, Scikit-learn, Matplotlib).  
    - Google Colab (darmowe środowisko).  
    - Azure (prosta aplikacja – np. Azure Web App).  
    - GitHub (repozytorium projektu).  

#### 4. Wyniki i analiza (5-7 stron)
- **Cel**:  
  Przedstawienie wyników i wykresów.  
- **Zakres**:  
  - Tabelka z metrykami (accuracy, precision, recall) dla różnych modeli ensemble.  
  - Wykresy: Trening vs walidacja (accuracy w czasie, jak na załączonym obrazku).  
  - Krótka analiza: "Model ensemble (Voting Classifier) osiągnął 90% accuracy, co jest lepsze niż pojedyncze modele".  
  - Znaczenie dla śledztwa: Automatyzacja wykrywania malware w dochodzeniach.

#### 5. Wnioski i dalsze prace (2-3 strony)
- **Cel**:  
  Podsumowanie i propozycje rozwoju.  
- **Zakres**:  
  - Podsumowanie: "Udało się zbudować model ensemble do wykrywania malware z 90% skutecznością".  
  - Ograniczenia: "Model opiera się na statycznych cechach, brak analizy dynamicznej".  
  - Dalsze prace: "Można dodać głębokie sieci neuronowe lub analizę w czasie rzeczywistym".

#### 6. Bibliografia (1-2 strony)
- Lista źródeł: artykuły, dokumentacje, tutoriale.

---

### Wymagania promotora – jak je zrealizować?

#### 1. Wczytywanie danych
- **Jak**: Użyjesz Pandas w Pythonie (`pd.read_csv()`).  
- **Zbiory danych**:  
  - Kaggle: "Malware Classification Dataset" (gotowe cechy plików).  
  - VirusShare: Darmowe próbki malware (proste cechy, np. rozmiar, entropia).  
  - Zbiór syntetyczny: Wygenerujesz w Pythonie (np. losowe wartości cech: `np.random.uniform()`).  
- **Czas**: 1-2 dni (pobieranie + wczytywanie).

#### 2. Standaryzacja i normalizacja
- **Jak**:  
  - Standaryzacja: `StandardScaler` z Scikit-learn (przekształca dane na średnią 0, odchylenie 1).  
  - Normalizacja: `MinMaxScaler` (przeskalowuje do [0,1]).  
- **Kod**:  
  ```python
  from sklearn.preprocessing import StandardScaler, MinMaxScaler
  scaler = StandardScaler()
  normalized_data = scaler.fit_transform(data)
  ```
- **Czas**: 1 dzień (nauka + kod).

#### 3. Walidacja literaturą
- **Jak**: W rozdziale 2 opiszesz badania nad ML w wykrywaniu malware (Google Scholar: "machine learning malware detection").  
- **Przykład**: "Smith i in. (2020) pokazali, że Random Forest osiąga 92% skuteczności w wykrywaniu malware na podstawie cech statycznych".  
- **Czas**: 2-3 dni (szukanie + pisanie).

#### 4. Wykresy
- **Jak**: Wzorując się na załączonym wykresie (trening i walidacja), użyjesz Matplotlib do pokazania accuracy modelu w czasie.  
- **Kod**:  
  ```python
  import matplotlib.pyplot as plt
  plt.plot(history.history['accuracy'], label='Train Accuracy')
  plt.plot(history.history['val_accuracy'], label='Validation Accuracy')
  plt.xlabel('Epochs')
  plt.ylabel('Accuracy')
  plt.legend()
  plt.savefig('plot_accuracy.png')
  plt.show()
  ```
- **Uwaga**: Jeśli używasz Random Forest, możesz ręcznie zapisać accuracy po każdej iteracji (np. zmieniając wielkość danych treningowych).  
- **Czas**: 1-2 dni (nauka + kod).

#### 5. Zacząć robić proces z pliku
- **Jak**: Przygotujesz skrypt Pythona, który wczytuje dane, standaryzuje je, trenuje model i generuje wykresy.  
- **Plik**: `malware_detection.py` – zapiszesz cały proces (wczytywanie, standaryzacja, model, wykresy).  
- **Czas**: 2-3 dni (pisanie kodu).

#### 6. Kilka różnych zbiorów danych
- **Jak**:  
  - Zbiór 1: Kaggle (np. "Malware Classification Dataset").  
  - Zbiór 2: VirusShare (proste cechy).  
  - Zbiór 3: Syntetyczny (wygenerujesz w Pythonie).  
- **Czas**: 2 dni (pobieranie + generowanie).

#### 7. Zbiór syntetyczny
- **Jak**: Wygenerujesz dane w Pythonie (np. losowe cechy plików: rozmiar, entropia).  
- **Kod**:  
  ```python
  import pandas as pd
  import numpy as np
  synthetic_data = pd.DataFrame({
      'size': np.random.uniform(1000, 1000000, 1000),
      'entropy': np.random.uniform(0, 8, 1000),
      'label': np.random.choice([0, 1], 1000)  # 0: benign, 1: malware
  })
  synthetic_data.to_csv('synthetic_malware.csv', index=False)
  ```
- **Czas**: 1 dzień.

#### 8. Apka na Azure
- **Jak**: Stworzysz prostą aplikację w Azure Web App, która wyświetla wyniki modelu (np. "Plik jest złośliwy: 90%").  
- **Kroki**:  
  1. Użyj Flask (prosty framework Pythona) do stworzenia apki.  
  2. Wdróż na Azure Web App (darmowe konto studenckie).  
- **Kod (Flask)**:  
  ```python
  from flask import Flask, request
  app = Flask(__name__)
  @app.route('/')
  def home():
      return "Wynik: Plik jest złośliwy z prawdopodobieństwem 90%"
  if __name__ == "__main__":
      app.run()
  ```
- **Czas**: 3-4 dni (nauka Flask + wdrożenie).

#### 9. Ensemble method
- **Jak**: Użyjesz metod ensemble w Scikit-learn:  
  - Random Forest (już jest ensemble).  
  - Voting Classifier (łączy kilka modeli).  
- **Kod**:  
  ```python
  from sklearn.ensemble import RandomForestClassifier, VotingClassifier
  from sklearn.svm import SVC
  from sklearn.naive_bayes import GaussianNB
  rf = RandomForestClassifier()
  svm = SVC(probability=True)
  nb = GaussianNB()
  ensemble = VotingClassifier(estimators=[('rf', rf), ('svm', svm), ('nb', nb)], voting='soft')
  ensemble.fit(X_train, y_train)
  ```
- **Czas**: 2 dni (nauka + kod).

#### 10. Proste obliczenia
- **Jak**: Obliczysz metryki modelu: accuracy, precision, recall, F1-score.  
- **Kod**:  
  ```python
  from sklearn.metrics import accuracy_score, precision_score, recall_score, f1_score
  print("Accuracy:", accuracy_score(y_test, y_pred))
  print("Precision:", precision_score(y_test, y_pred))
  print("Recall:", recall_score(y_test, y_pred))
  print("F1-score:", f1_score(y_test, y_pred))
  ```
- **Czas**: 1 dzień.

#### 11. GitHub i język Git
- **Jak**:  
  1. Załóż konto na GitHub.  
  2. Stwórz repozytorium (np. "malware-detection").  
  3. Użyj Git do zarządzania kodem:  
     - `git init` (inicjalizacja).  
     - `git add .` (dodanie plików).  
     - `git commit -m "Initial commit"` (zapis zmian).  
     - `git push` (wysłanie na GitHub).  
  4. Wyślij promotorowi link do repozytorium.  
- **Czas**: 1-2 dni (nauka Git + konfiguracja).

#### 12. Pokazać i wysłać pracę na GitHub
- **Jak**:  
  - Dodaj do repozytorium kod (`malware_detection.py`), dane (CSV), wykresy (`plot_accuracy.png`) i plik pracy (PDF).  
  - Udostępnij promotorowi link (np. `github.com/twoje-konto/malware-detection`).  
- **Czas**: 1 dzień.

---

### Czas realizacji (4 tygodnie)
Zakładam 1-2 godziny dziennie:
- **Tydzień 1**: Nauka Pythona, Scikit-learn, Git (5 dni).  
- **Tydzień 2**: Wczytywanie danych, standaryzacja, zbiór syntetyczny (5 dni).  
- **Tydzień 3**: Model ensemble, wykresy, proste obliczenia (5 dni).  
- **Tydzień 4**: Apka na Azure, GitHub, pisanie pracy (5-7 dni).  

---

### Jak to uprościć?
- Skorzystaj z gotowych tutoriali (np. "Malware Detection with Python" na Kaggle).  
- Użyj Google Colab – zero instalacji.  
- Ogranicz apkę na Azure do minimum (tylko wynik).  
