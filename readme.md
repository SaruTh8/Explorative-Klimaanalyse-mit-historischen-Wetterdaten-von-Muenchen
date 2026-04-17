# LMU München Wetterdaten — Data Science Projekt (2005–2025)

## Projektübersicht
Dieses Projekt analysiert 20 Jahre Wetterdaten der Wetterstation des 
Meteorologischen Instituts der LMU München. Die Daten wurden eigenständig 
gescrapt, bereinigt und anschließend mit Machine Learning Methoden analysiert.

## Datenquelle
- **Quelle:** Meteorologisches Institut der LMU München  
- **URL:** https://www.en.meteo.physik.uni-muenchen.de
- **Zeitraum:** 01.01.2005 – 31.12.2025  
- **Auflösung:** Täglich  
- **Variablen:** T_max, T_min, Niederschlag, Luftdruck (max/min), Sonnenstunden

## Projektstruktur
├── 01_scraping_and_cleaning.ipynb       # Web-Scraping der LMU-Rohdaten und Bereinigung & Preprocessing
├── 02_eda.ipynb            # Explorative Datenanalyse
├── 03_forecasting.ipynb    # Zeitreihenvorhersage mit XGBoost
├── 04_anomaly.ipynb        # Anomalieerkennung

## Methoden & Ergebnisse

### Datenbereinigung
- 223 fehlende Tage identifiziert und zeitlich interpoliert
- Fehlende Tage mit `ist_interpoliert`-Flag markiert
- Sonne-Spalte von kumulativer Monatssumme auf Tageswerte zurückgerechnet

### Explorative Datenanalyse
- Erwärmungstrend: **+1.2°C pro Dekade** (2005–2025)
- Dominante Jahressaisonalität mit ±10°C Amplitude
- Starke AR(1)-Struktur — gestriger Wert erklärt ~90% der Varianz
- Rückgang der Eistage von 38 (2005) auf 1 (2020)

### Zeitreihenvorhersage (T_max)
| Modell | MAE | Verbesserung |
|--------|-----|-------------|
| Baseline (Persistence) | 2.91°C | — |
| XGBoost v1 | 2.84°C | +2.4% |
| XGBoost v2 | 3.65°C | -25.6% |
| XGBoost v3 | 2.81°C | +3.4% |

### Anomalieerkennung
Drei Methoden verglichen:
- **Regelbasiert** (DWD-Schwellenwerte): Hitzetage, Frosttage, Starkregen
- **Z-Score** (|Z|>2): 245 Anomalien identifiziert
- **Isolation Forest** (multivariat): 231 Anomalien identifiziert

## Technologien
- Python 3.14
- pandas, numpy, matplotlib, seaborn
- scikit-learn, xgboost, statsmodels
- Jupyter Notebook

## Erkenntnisse
1. München zeigt einen Erwärmungstrend von +1.2°C pro Dekade (eigene Berechnung aus den Daten 2005 bis 2025)
   Zum Vergleich: globaler Langzeittrend seit 1850 liegt bei +0.06°C/Dekade 
   (Quelle: NOAA Annual Climate Report 2024)
2. Hitzetage (≥30°C) haben sich seit 2015 deutlich gehäuft
3. Eistage sind seit 2005 dramatisch zurückgegangen
4. XGBoost schlägt die naive Baseline nur marginal — 
   die starke Autokorrelation macht einfache Modelle schwer zu übertreffen
5. Isolation Forest und Z-Score stimmen bei robusten Anomaliejahren 
   (2010, 2012, 2013) überein

## Ausblick
- LSTM für längere Zeitabhängigkeiten
- Multivariate Vorhersage (nicht nur T_max)
- Automatisches tägliches Update der Daten und Modellevaluation

## Autor
Sarujan Thangarajah  
Projekt erstellt: April 2025
