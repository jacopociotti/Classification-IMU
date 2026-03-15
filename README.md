# 🏋️‍♂️ Classificazione di esercizi in palestra con IMUe

## 🎯 Obiettivo del Progetto
Questo progetto ha come obiettivo la realizzazione di un modello di Machine Learning per la classificazione di attività fisiche (esercizi in palestra). La classificazione si basa sull'analisi dei dati acquisiti da un sensore inerziale (IMU), comprensivo di accelerometro e giroscopio, indossato sull'avambraccio.

## 📊 Dataset e Strumenti
* **Dataset di riferimento**: MyoGym.
* **Sensore utilizzato**: I dati sono stati raccolti tramite un bracciale Myo Armband posizionato sull'avambraccio destro. Per la raccolta dati è stata utilizzata una frequenza di 50 Hz.
* **Caratteristiche del segnale**: Sebbene il sensore acquisisca anche segnali EMG, questo progetto si concentra esclusivamente sui dati IMU (accelerometro a 3 assi e giroscopio a 3 assi).
* **Feature utilizzate**: Sono state estratte 57 feature per l'accelerometro e 57 per il giroscopio, per un totale di 114 feature. I segnali sono stati segmentati tramite finestramento temporale con finestra di 4 s e passo di 1 s.

## 🛠️ Metodologia e Workflow
Il progetto è stato sviluppato seguendo questi step principali:
1. **Analisi iniziale**: Esplorazione del dataset MyoGym e delle feature IMU fornite.
2. **Selezione delle classi**: Le classi non nettamente distinguibili solo tramite IMU sono state rimosse in base a un f1-score $\ge0.75$, precision e recall. In particolare, le varianti dell'esercizio "Biceps Curl" (classi 19-24) sono state accorpate in una singola nuova classe, la 31. 
3. **Addestramento Modello (SVC)**: È stato implementato un classificatore Support Vector Classifier (SVC) configurato con `kernel="rbf"`, `C=1.0`, `gamma="scale"` e `class_weight="balanced"` .
4. **Validazione incrociata**: Sono state utilizzate e confrontate due tecniche di validazione: la Leave-One-Subject-Out (LOSO) per valutare la generalizzazione su nuovi soggetti e uno split $80/20$ per l'apprendimento di pattern motori condivisi.
5. **Feature Extraction Personalizzata**: Le feature sono state ricalcolate e confrontate con quelle fornite dal dataset. Il confronto ha mostrato elevata coerenza; sono stati esclusi solo i parametri di Hjorth e l'entropia.
6. **Ottimizzazione e Selezione Feature**: Tramite l'analisi della varianza (ANOVA), le feature sono state ridotte mantenendo quelle con F-score più discriminante, al fine di migliorare l'efficienza computazionale.

## 📈 Risultati Principali
I test effettuati hanno dimostrato l'efficacia del modello, con prestazioni variabili a seconda della tecnica di validazione:
* **Modello Iniziale (LOSO - 30 classi)**: Accuratezza complessiva (Overall) dello 0.681.
* **Modello Ottimizzato (LOSO - Classi selezionate/accorpate)**: Accuratezza complessiva dello 0.908. L'analisi per soggetto ha evidenziato prestazioni non uniformi (es. il soggetto 1 ha un'accuratezza inferiore), confermando che persone diverse generano segnali diversi per i medesimi movimenti.
* **Modello Intra-soggetto (Split 80/20)**: In condizioni di soggetto "noto", l'accuratezza ha raggiunto lo 0.994.
* **Confronto tra classificatori**: Utilizzando il Classification Learner di MATLAB, la SVM ha mostrato il miglior compromesso tra accuratezza e complessità computazionale (es. Linear SVM con il 99.3% di accuratezza).
* **Riduzione delle Feature**: Applicando la riduzione ANOVA, il modello Linear SVM ha mantenuto un'accuratezza del 99.4% utilizzando solo 64 feature su 114.
