# AQUADROP AI — prototype

Three-zone smart-irrigation prototype: ML irrigation prediction → priority-based water allocation → irrigation **simulation** → monitoring → rule-based fault flags → dashboard.
**Everything physical (pump, valves, moisture, flow) is simulated. No hardware or real field data is involved.**

## Run on Windows
1. Install Python 3.10+ from python.org (tick **Add Python to PATH**).
2. Open Command Prompt in this folder:
```
cd backend
python -m venv venv
venv\Scripts\activate
pip install -r requirements.txt
uvicorn app.main:app --reload
```
3. Open http://127.0.0.1:8000 (the backend serves the dashboard; no separate frontend server).
4. Run tests (from the project folder): `python -m pytest tests -q`
5. Optional weather: copy `.env.example` to `.env`, add your OpenWeather key. Without it, labelled demo weather is used (editable under Settings).

## Using it
- **AI Prediction**: "Train on SYNTHETIC demo data", or upload a CSV, map columns, choose a 2-class target (e.g. pump ON/OFF) and train.
  Before any training, a labelled rule-based fallback is used.
- **Allocation**: press *Allocate water*; see demand, score, rank, allocation, unmet demand, reason.
- **Irrigation**: Start (uses allocation) or start a zone manually; *Auto* mode runs the whole pipeline when a zone drops below its threshold.
  Inject a clog / leak / stuck valve to see alerts.
- Database: `aquadrop.db` (SQLite, created automatically). Models: `models/`.

## Files
`backend/app/`: `main.py` (API) · `ml.py` (training/prediction) · `allocation.py` · `simulation.py` · `faults.py` · `weather.py` · `db.py`. `frontend/index.html` (dashboard). `tests/test_core.py` and `tests/test_persistence.py`.

## Troubleshooting
- `python` not found → reinstall Python with PATH ticked. `uvicorn` not found → activate the venv first.
- Charts blank → Chart.js loads from the internet (cdnjs); check your connection.
- Port busy → `uvicorn app.main:app --port 8001`.

## Status
See the chat summary for what was and wasn't tested. Not done: MQTT/ESP32 code, PostgreSQL, Kaggle S14 dataset inspection, forecast-based weather logic.
