# Soluslab

**Environmental Balance Calculator** — a Streamlit web app that computes the
net CO₂ footprint and biocapacity impact of waste collection and transport
operations.

## What it does

For each waste movement, Soluslab calculates a three-part carbon balance:

- **P1** — Emission savings from recycling vs. linear alternatives (landfill /
  virgin production)
- **P2** — Emissions from the return (empty) trip from operator to client
- **P3** — Emissions from the client-to-plant leg, with fixed and variable
  components

Vehicle emissions follow a **logarithmic curve** calibrated on empty/full load
values, with a linear EEA fallback for uncalibrated vehicles.

Biocapacity is derived from GFN forest absorption factors and equivalence
factors for biological materials.

## Features

- Per-movement CO₂ and global hectares (gha) calculation
- Vehicle and material management with JSON persistence
- Dark-theme Plotly charts
- Special handling for mixed/undifferentiated waste streams
- Deployable on Streamlit Community Cloud

## Stack

- Python 3
- Streamlit
- Plotly
- JSON (vehicles and materials configuration)

## Run locally

```bash
pip install -r requirements.txt
streamlit run app.py
```

## Data files

| File | Description |
|---|---|
| `veicoli.json` | Vehicle fleet with emission curve parameters |
| `materiali.json` | Waste materials with emission factors and recycling rates |

## License

Private — internal use only.
