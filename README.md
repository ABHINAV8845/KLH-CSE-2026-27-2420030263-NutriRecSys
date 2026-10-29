# SafeNutri-RAG: ML-Based Personalized Nutrition Recommendation System

Personalized nutrition recommendation system that predicts a person's
calorie/macronutrient needs from real health data, enforces medical safety
via biomarker-based constraints, and (in its full design) grounds meal
recommendations in verified nutrition data instead of free-form AI
generation.

## Repository Structure
```
data/       Sample/synthetic dataset (stand-in for real NHANES data)
docs/       Methodology, dataset sources, and architecture diagram
reports/    Formal Word reports (Review-1, Review-2, Mini-Hackathon-1)
results/    Generated plots, metrics, and the trained model file
src/        Python source code for the core pipeline
```

## Problem Statement
Generic diet plans ignore individual biology — age, BMI, biomarkers like
glucose and HbA1c, and medical conditions. Existing systems predict
calorie/macro needs but rarely turn these into real, verified meal plans,
and none provide an explanation for their recommendations.

## Objectives
- Predict personalized calorie/macronutrient needs using NHANES health data.
- Build a stronger ensemble model (LightGBM + XGBoost + Random Forest).
- Enforce medical safety using biomarker-based hard constraints.
- Ground meal generation in verified nutrition data (RAG).
- Recommend meals via hybrid content + collaborative filtering.
- Explain every recommendation using SHAP.

## Datasets
| Dataset | Role | Link |
|---|---|---|
| NHANES 2017–2018 | Health/biomarker data | wwwn.cdc.gov/nchs/nhanes/continuousnhanes |
| USDA FoodData Central | Verified nutrient values | fdc.nal.usda.gov/download-datasets.html |
| Food.com Recipes & Interactions | Recipes & ratings | kaggle.com/datasets/shuyangli94/food-com-recipes-and-user-interactions |

Full details: [`docs/dataset_sources.md`](docs/dataset_sources.md)

## System Architecture
![System Architecture](docs/system_architecture.png)

Full methodology: [`docs/methodology.md`](docs/methodology.md)

## What's Implemented vs. Planned

| Component | Status |
|---|---|
| Data pipeline (synthetic stand-in) | ✅ Implemented, executed |
| Feature engineering | ✅ Implemented, executed |
| Stacked ensemble | ✅ Implemented, executed |
| Biomarker constraint layer | ✅ Implemented, tested |
| Feature importance / explainability | ✅ Implemented (`src/shap_explainer.py` with SHAP) |
| RAG-grounded recommendation | ✅ Implemented (`src/rag_pipeline.py`) |
| Food.com recipe catalog & retrieval | ✅ Implemented (`src/prepare_foodcom_catalog.py`) |
| Full-Stack Interactive App & API | ✅ Implemented (`src/server.py` + `website/`) |
| Automated Test Suite | ✅ Implemented (`tests/`) |
| Hybrid content + collaborative recommendation | ⏳ Planned |
| Real NHANES / LightGBM / XGBoost scale-up | ⏳ Future enhancement |

## Interactive Web Dashboard & API
- **Live Full-Stack Mode**: Run `python src/server.py` to launch the FastAPI server at `http://127.0.0.1:8000`, connected directly to the stacked ensemble, SHAP explainability, and Food.com RAG retriever.
- **Standalone Static Mode**: Open `website/index.html` directly in any web browser without a backend server (runs in-browser simulated inference and JavaScript constraints).

## How to Run (VS Code / Terminal)

### 1. Open Terminal & Activate Environment
In VS Code, open the integrated terminal (`Ctrl + ` `) and activate the pre-configured virtual environment:

**PowerShell (VS Code Default):**
```powershell
.\.venv\Scripts\Activate.ps1
```
*(If PowerShell restricts script execution, run: `Set-ExecutionPolicy -Scope Process -ExecutionPolicy Bypass` first)*

**Command Prompt (cmd):**
```cmd
.venv\Scripts\activate.bat
```

*(Optional - if creating a fresh environment):*
```bash
pip install -r requirements.txt
```

---

### 2. Launch the Full Web Application & API (Recommended)
Starts the FastAPI server serving the interactive web dashboard connected to the stacked ensemble, SHAP explainability engine, and Food.com RAG retrieval:
```bash
python src/server.py
```
- **Web Dashboard**: [http://127.0.0.1:8000](http://127.0.0.1:8000)
- **Interactive API Docs (Swagger)**: [http://127.0.0.1:8000/docs](http://127.0.0.1:8000/docs)

---

### 3. Run the Automated Test Suite
Run the test suite verifying API health, recommendation pipeline, and clinical biomarker constraints:
```bash
pytest tests/ -v
```

---

### 4. Run CLI Meal Recommendation Demo
Generate a sample 1-day personalized meal plan with clinical constraint checks directly in the terminal:
```bash
python src/recommend_meals.py
```

---

### 5. Re-run ML Pipeline & SHAP Evaluation (Optional)
Re-generate synthetic health data, build the Food.com catalog, and re-train the ensemble:
```bash
python src/generate_synthetic_data.py
python src/prepare_foodcom_catalog.py
python src/explain_and_evaluate.py
```
Outputs and plots will be saved to `results/`.

## Results (on synthetic data — see docs/methodology.md for caveats)
| Model | RMSE | MAE | R² |
|---|---|---|---|
| HistGradientBoosting (LightGBM-analog) | 213.24 | 170.88 | 0.739 |
| GradientBoosting (XGBoost-analog) | 209.03 | 165.91 | 0.749 |
| Random Forest | 206.63 | 164.11 | 0.755 |
| **Stacked Ensemble (proposed)** | **206.51** | **164.57** | **0.755** |

See `results/` for full plots (predicted-vs-actual, model comparison,
feature importance) and `reports/` for the full write-ups.

## Reports
- `reports/Nutrition_RecSys_Project_Report.docx` — Review-1 (Problem Statement → Project Plan)
- `reports/Nutrition_RecSys_Review2_Report.docx` — Review-2 (Methodology, System Design, Implementation, Results)
- `reports/MiniHackathon1_SafeNutriRAG.docx` — Mini-Hackathon-1 submission (incl. Q&A prep)

## Demo Video
*(Not yet recorded — add a link here once available.)*

## Status
Core ML stacked ensemble pipeline, RAG-grounded Food.com recommendation engine, SHAP explainability system, and full-stack FastAPI + web dashboard are complete, executed, and verified with automated tests.
