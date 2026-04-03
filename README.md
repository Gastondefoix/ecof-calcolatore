Ecof Calcolatore — Saldo Ambientale
Strumento interno di Ecof Italia per il calcolo del saldo ambientale associato ai movimenti di rifiuti gestiti per conto dei clienti.

Cosa fa:
Per ogni ritiro inserito, calcola la CO₂ netta prodotta e la converte in ettari globali (gha), distinguendo tra:

Biocapacità liberata — quando il riciclo restituisce più risorse biologiche di quante ne consuma
Impronta ecologica — quando il bilancio netto è negativo per l'ambiente

Come funziona:
Il calcolo si divide in tre componenti:

P1 — bilancio emissivo del riciclo rispetto alla produzione vergine e allo smaltimento
P2 — emissioni del tragitto Ecof → cliente, ripartite equamente tra i clienti dello stesso giro
P3 — emissioni del tragitto cliente → impianto, con quota fissa ripartita per giro e quota variabile proporzionale al peso conferito (Conf = Q / C)

Per i materiali biologici (carta, legno, organico) il saldo in gha include due contributi distinti:

Biocapacità da CO₂ evitata — quando la CO₂ netta è negativa, convertita in gha tramite la capacità di assorbimento forestale del Global Footprint Network (1.197 tCO₂/gha/anno)
Biocapacità da terreno — superficie forestale o agricola risparmiata grazie al mancato ricorso a materia prima vergine, calcolata usando i fattori di equivalenza GFN

Per i materiali abiotici (plastica, metalli, vetro, toner) viene calcolata solo la biocapacità da CO₂ evitata, quando applicabile.
Curva emissioni veicoli
Le emissioni al km in funzione del carico seguono un modello logaritmico calibrato su due punti (CO₂/km a vuoto e a pieno carico):
CO₂perKm(C) = co2_vuoto + b × ln(1 + C)
b = (co2_pieno - co2_vuoto) / ln(1 + C_max)
Finché CO₂/km a pieno carico non è disponibile per un veicolo, viene usato un modello lineare EEA come fallback.

Stack:

Python 3
Streamlit
Pandas
Plotly

Avvio:

pip install streamlit pandas plotly
streamlit run ecof_calcolatore.py
