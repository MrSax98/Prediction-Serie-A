# Predittore Risultati Serie A  Serie A Match Predictor 🇮🇹⚽️

Questo progetto utilizza dati storici delle partite di Serie A per addestrare un modello di Machine Learning (Random Forest) capace di predire l'esito (Vittoria Casa 'H', Pareggio 'D', Vittoria Trasferta 'A') delle partite future.

**Progetto Realizzato con l'aiuto del "Comitato di Esperti AI"**

## Come Funziona

Il processo si sviluppa attraverso i seguenti passi principali all'interno del notebook `serie_a_predictor.ipynb`:

1.  **Caricamento Dati:** Lettura dei file CSV contenenti i risultati storici per stagione da Google Drive (o percorso locale).
2.  **Pulizia Dati:** Gestione dei valori mancanti, correzione dei tipi di dato (es. conversione date).
3.  **Feature Engineering:** Creazione di nuove features informative, come:
    * Giorno della settimana (`DayOfWeek`)
    * Forma Punti Ultime 5 Partite (`HomeFormPts_L5`, `AwayFormPts_L5`)
    * Forma Gol Segnati/Concessi Ultime 5 Partite (`HomeFormGS_L5`, `HomeFormGC_L5`, etc.)
4.  **Analisi Esplorativa (EDA):** Visualizzazione dei dati per capirne le distribuzioni e i trend (es. vantaggio casalingo, media gol).
5.  **Preparazione Modello:**
    * Selezione delle features pre-partita valide (evitando data leakage!).
    * Codifica One-Hot delle variabili categoriche (squadre).
    * Divisione **cronologica** del dataset in training set (85%) e test set (15%).
6.  **Addestramento Modello:** Addestramento di un classificatore Random Forest sui dati di training.
7.  **Valutazione Modello:** Valutazione delle performance del modello sul test set (Accuracy, Confusion Matrix, Classification Report). Il modello attuale raggiunge circa il **48% di Accuracy**.
8.  **Funzione di Previsione:** Una funzione Python per calcolare le features necessarie e predire i risultati (con probabilità H/D/A) per un nuovo set di partite.

## Requisiti Dati

⚠️ **I dati storici della Serie A NON sono inclusi in questo repository.**

* È necessario fornire i propri file CSV, uno per stagione (es. `season-2324.csv`, `season-2223.csv`, ecc.).
* I file devono contenere almeno le seguenti colonne utilizzate dal codice:
    * `Date`: Data della partita (formato leggibile da `pd.to_datetime`, es. `GG/MM/AA` o `YYYY-MM-DD`)
    * `HomeTeam`: Nome squadra casa (stringa)
    * `AwayTeam`: Nome squadra trasferta (stringa)
    * `FTHG`: Gol finali squadra casa (numero intero)
    * `FTAG`: Gol finali squadra trasferta (numero intero)
    * `FTR`: Risultato finale ('H', 'D', 'A')
    * *(Opzionali per pulizia iniziale, ma presenti nei dati usati)* `HTHG`, `HTAG`, `HTR` (Gol/Risultato primo tempo)
* Il codice nel notebook è impostato per leggere questi file da una cartella chiamata `Stagioni Serie A` su Google Drive, ma può essere adattato per leggere da un percorso locale.

## Setup

1.  **Clona il Repository:**
    ```bash
    git clone [https://github.com/TUO_USERNAME/TUO_REPONAME.git](https://github.com/TUO_USERNAME/TUO_REPONAME.git)
    cd TUO_REPONAME
    ```
2.  **(Opzionale) Crea un Ambiente Virtuale:**
    ```bash
    python -m venv venv
    source venv/bin/activate  # Su Windows: venv\Scripts\activate
    ```
3.  **Installa le Dipendenze:**
    ```bash
    pip install -r requirements.txt
    ```

## Come Eseguire

1.  **Prepara i Dati:** Assicurati che i tuoi file CSV delle stagioni siano accessibili nel percorso specificato nel notebook (Google Drive o locale). Modifica il percorso nel notebook se necessario.
2.  **Apri il Notebook:** Apri `serie_a_predictor.ipynb` in un ambiente compatibile (Jupyter Notebook, Jupyter Lab, Google Colab, VS Code, ecc.).
3.  **Esegui le Celle:** Esegui le celle del notebook in sequenza dall'inizio alla fine.
    * Se usi Google Colab, la prima cella monterà il tuo Google Drive e richiederà autorizzazione.
    * Le celle che calcolano la forma (`.apply`) potrebbero richiedere alcuni minuti per essere eseguite.
4.  **Visualizza le Previsioni:** L'ultima parte del notebook definisce e utilizza la funzione `predict_next_matchday`. Modifica la lista `giornata_esempio` con le partite reali che vuoi predire ed esegui la cella per vedere i risultati formattati.

## Librerie Utilizzate

Vedi il file `requirements.txt`. Le principali sono:
* pandas
* numpy
* scikit-learn
* matplotlib
* seaborn

## Performance e Limitazioni

* Il modello Random Forest attuale, con le features di forma incluse, raggiunge un'**accuracy di circa il 48%** sul test set cronologico.
* Il modello è più bravo a predire le vittorie casalinghe ('H') che quelle esterne ('A') e **molto debole nel predire i pareggi ('D')**.
* Le previsioni si basano **esclusivamente** sulle features incluse (identità squadre, giorno settimana, forma punti/gol L5) e non tengono conto di fattori cruciali come infortuni, squalifiche, motivazioni, condizioni meteo, H2H, ecc.
* **Le previsioni sono indicative e non costituiscono una garanzia di risultato.**

## Possibili Miglioramenti Futuri

* Aggiungere più features (classifica, punti/goal difference tra squadre, ELO ratings, H2H).
* Provare diversi modelli (XGBoost, LightGBM, Reti Neurali).
* Ottimizzare gli iperparametri del modello attuale.
* Implementare tecniche per migliorare la predizione dei pareggi.
* Salvare/caricare il modello addestrato per evitare il riaddestramento.
