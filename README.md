# 🌿 AI-Driven Smart Irrigation for Ireland

Forecasts **tomorrow's** net irrigation requirement (NIR) for Irish counties
from freely available weather data — no IoT sensors, no soil probes.

MSc Data Analytics thesis, Technological University of the Shannon (TUS),
Limerick · Author: Vignesh Gunnala · Supervisor: Eric McNamara

---

## The question

Irish farms are rain-fed, so irrigation is a marginal decision — until a three
week dry spell lands on a crop's most sensitive growth stage. The practical
question is not "how much water this season" but:

> **Should I irrigate tomorrow, or wait for the rain?**

## The method

```
Met Éireann daily weather (2010–2024, Irish counties)
        │
FAO-56 agronomy:  ET0 (Hargreaves-Samani) → ETc = ET0 × Kc → effective rainfall Pe
        │
label:  NIR = max(0, ETc − Pe)
        │
features known on day t: today's weather and agronomy, lags (1/2/3/7 d),
rolling sums and means (3/7/14/30 d), days since rain, seasonality
        │
target: NIR on day t+1        ← tomorrow's weather is never used
        │
Ridge · Random Forest · XGBoost, chronological split, tested once
```

## Results

Test period: 2023–2024, held out and never tuned on.

| Model | MAE (mm) | RMSE (mm) | R² |
|---|---|---|---|
| _fill from `results/metrics.md` after running_ | | | |
| Baseline: persistence | | | |
| Baseline: climatology | | | |

Irrigate/skip decision at a 1 mm threshold: precision _, recall _, F1 _.

A model only counts here if it beats both baselines. The numbers go in this
table exactly as the notebook prints them.

## A correction worth recording

An earlier version of this work predicted NIR for the **same day** while `ETc`
and `Pe` were in the feature list. Since `NIR = max(0, ETc − Pe)`, the model was
handed the two numbers the answer is computed from and scored **R² = 1.000**.
That is target leakage, not accuracy.

Restating the task as a t+1 forecast removed it. The current numbers are lower
and they mean something. Keeping this note in the README is deliberate —
finding your own leak and fixing it is part of the work.

## Run it

Open `smart_irrigation_v2_leakage_free.ipynb` in Colab and choose
**Runtime → Run all**. The dataset downloads itself; nothing needs uploading.

Outputs land in `results/` (metrics, summary, sample predictions) and
`outputs/figures/` (feature importance, predicted vs actual).

## Known limitations

- Soil state is a constant available-water term, not a running water balance
- Monthly Kc approximates regional growth stages, not field-level ones
- Effective rainfall uses a fixed fraction rather than an intensity-aware method
- Labels are agronomic estimates, not measured irrigation applications — the
  system is validated against a model of reality, not reality

## Related

[smart-agri-rag](https://github.com/vigneshgunnala/smart-agri-rag) — a
retrieval-augmented assistant that explains these recommendations in plain
English, grounded in the same agronomy.

## Author

**Vignesh Gunnala** — Data & AI Analyst, Limerick, Ireland
[LinkedIn](https://linkedin.com/in/vigneshgunnala) · vigneshgunnala440@gmail.com

Data: Met Éireann and ESDAC, subject to their own licensing terms.
