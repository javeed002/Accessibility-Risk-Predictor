# AI-Based Website UI Accessibility Risk Predictor

| Layer | Tool |
|---|---|
| Frontend | HTML, CSS, JavaScript (`frontend/index.html`, `style.css`) |
| Dashboard | React.js (`frontend/dashboard.html`) |
| Backend | Python + FastAPI (`backend/main.py`) |
| ML | Scikit-learn, Pandas, NumPy (`backend/ml/`) |
| Accessibility testing | axe-core (injected by `backend/scanner.py`) |
| Browser automation | Playwright (Selenium is in requirements as an alternative) |
| Database | SQLite (`backend/db.py`); swap to MySQL if desired |
| Charts | Chart.js |

## Login
- Username: `admin`
- Password: `Admin@123`  (change it after first run; you can also register new users)

## Run (VS Code terminal)
```bash
python -m venv .venv
.venv\Scripts\activate          # Windows   (Mac/Linux: source .venv/bin/activate)
pip install -r requirements.txt
playwright install chromium
python backend/ml/train.py      # trains the model (also auto-trains on first scan)
uvicorn backend.main:app --reload
```
Open http://127.0.0.1:8000

Offline axe-core: put `axe.min.js` in `backend/` (from `npm i axe-core`, file `node_modules/axe-core/axe.min.js`) and it is used instead of the CDN.

## Git / GitHub
```bash
git init && git add . && git commit -m "Initial commit"
git branch -M main
git remote add origin https://github.com/<you>/accessibility-risk-predictor.git
git push -u origin main
```

## How it works
1. User logs in, enters a URL.
2. Playwright opens the page; axe-core finds violations.
3. Violation counts become features for a RandomForest model, which outputs a Low/Medium/High risk and a 0–100 score.
4. Results are saved in SQLite and shown with Chart.js.

Note: the model starts on synthetic data; retrain on real rows from the `scans` table as you collect them.
