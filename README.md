# Soluslab

**Calcolatore Saldo Ambientale** — applicazione *Streamlit* per il calcolo
dell'impatto netto in CO₂ e biocapacità delle operazioni di raccolta
e trasporto rifiuti.

🌐 **Demo:** [soluslab.streamlit.app](https://soluslab.streamlit.app)

---

## Descrizione

Soluslab quantifica il bilancio ambientale di ogni movimento di rifiuti,
combinando l'impatto del processo di riciclo con quello del trasporto su
un itinerario circolare. Il risultato è espresso sia in kg di CO₂ netta
che in ettari globali (*gha*), consentendo un confronto diretto con la
biocapacità liberata dal riciclo.

---

## Modello di calcolo

Per ogni movimento vengono calcolate quattro componenti:

| Componente | Significato | Segno atteso |
|---|---|---|
| **S1** | Emissioni evitate: risparmio da smaltimento e produzione vergine evitati | negativo |
| **S2** | Emissioni generate dal processo di riciclo e trattamento | positivo |
| **T1** | Emissioni del tragitto itinerario, ripartite sul numero di clienti | positivo |
| **T2** | Incremento emissivo dovuto al carico, proporzionale al peso conferito | positivo |

Per i materiali **indifferenziati** (senza percorso di riciclo) le componenti S1 e S2
sono sostituite da un'unica voce **S** = costo di trattamento + smaltimento.

### Formule

```
S1 = -Q × (T_smalt + T_verg)
S2 =  Q × (T_tratt + T_ric)
T1 = (D_itinerario × CO2perKm(0)) / N_itinerario
T2 = (CO2perKm(C) − CO2perKm(0)) × D_baricentro_impianto × Conf

CO2_netta = S1 + S2 + T1 + T2
```

dove `Conf = Q / C` è la quota del carico totale attribuita al cliente.

### Curva emissioni veicolo

Le emissioni al km seguono una **curva logaritmica** calibrata su misurazioni
a vuoto e a pieno carico:

```
CO2perKm(C) = co2pkm_vuoto + b × ln(1 + C)
b = (co2pkm_pieno − co2pkm_vuoto) / ln(1 + C_max)
```

In assenza del dato a pieno carico viene usata una funzione lineare *EEA*
come fallback.

### Biocapacità

La biocapacità liberata è calcolata su due canali:

- **Da terreno** (solo materiali biologici): `(Q / 1000) / resa × f_equiv`
- **Da CO₂ evitata** (solo se CO₂ netta < 0): `|gha_impronta|`

Il **saldo netto** in *gha* = −impronta ecologica + biocapacità totale.

---

## Parametri di input

### Logistica — itinerario circolare

| Campo | Descrizione |
|---|---|
| `D_itinerario` | Distanza totale del giro circolare: Ecof → clienti → impianto → Ecof (km) |
| `D_baricentro_impianto` | Distanza dal baricentro dei clienti all'impianto di riciclo (km) |
| `N_itinerario` | Numero di clienti nel giro |
| `C` — Carico totale | Peso totale nel furgone al momento del ritiro (kg) |
| `Q` | Kg di rifiuto conferiti dal singolo cliente (kg) |

### Veicoli

| Campo | Descrizione |
|---|---|
| `co2pkm_vuoto` | Emissioni CO₂ al km a furgone vuoto (kg/km) |
| `co2pkm_pieno` | Emissioni CO₂ al km a pieno carico — calibra la curva logaritmica (kg/km) |
| `C_max` | Portata massima del veicolo (kg) |

### Materiali

| Campo | Descrizione |
|---|---|
| `T_verg` | CO₂ emessa per produrre 1 kg di materia vergine (kgCO₂/kg) |
| `T_smalt` | CO₂ emessa per smaltire 1 kg senza riciclo (kgCO₂/kg) |
| `T_tratt` | CO₂ emessa per il trattamento pre-riciclo di 1 kg (kgCO₂/kg) |
| `T_ric` | CO₂ emessa dal processo di riciclo di 1 kg (kgCO₂/kg) |
| `resa` | Resa coltura (t/ha/anno) — solo materiali biologici, fonte GFN |
| `f_equiv` | Fattore di equivalenza territoriale (gha/ha) — fonte GFN |

---

## Funzionalità

- Calcolo CO₂ netta e saldo in *gha* per singolo movimento
- Scomposizione grafica delle quattro componenti (S1, S2, T1, T2)
- Curva logaritmica CO₂/km interattiva con punto del giro corrente
- Gestione veicoli e materiali con persistenza JSON
- Dettaglio calcolo passo per passo espandibile
- Tabelle riassuntive dei fattori emissivi e del parco veicoli

---

## Stack

- Python 3
- Streamlit
- Plotly
- Pandas
- JSON (persistenza veicoli e materiali)

---

## Avvio locale

```bash
pip install -r requirements.txt
streamlit run app.py
```

---

## File di configurazione

| File | Descrizione |
|---|---|
| `veicoli.json` | Parco veicoli con parametri curva emissioni |
| `materiali.json` | Materiali rifiuto con fattori emissivi e tassi di riciclo |

I file vengono creati automaticamente al primo avvio con i valori predefiniti.

---

## Licenza

Pubblica
