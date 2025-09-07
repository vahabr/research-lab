# README.md 


## Anchored Kalman Filtering in a Markov PD Model

This repository contains a **reproducible Python/Quarto demo** of applying **anchored Kalman filters** inside a **Markov credit rating model**.  
It is designed to be:
- **Deterministic** (reproducible seeds, no hidden randomness),
- **Minimal** (only NumPy, Pandas, Matplotlib),
- **Transparent** (step-by-step explanations, runnable Quarto page).

---

## 🔍 What problem does this solve?

Traditional **Through-the-Cycle (TTC)** transition matrices don’t adapt to the macro environment.  
**Point-in-Time (PIT)** matrices tilt TTC probabilities according to a macro index.  
But macro forecasts are noisy — so we need a **Kalman filter** to estimate a smoother state.

This project compares:
- **Raw** macro index (no filter),
- **Naïve KF** (standard observation of forecast index),
- **Anchored KF** (stacked observation that pins the long-run level to 0).

The result: **anchored KF reduces volatility** of PIT transition matrices and stabilises lifetime PD term structures.

---

## 📂 Repository structure

```

.
├─ *quarto.yml                # Quarto project config (with thebe enabled)
├─ requirements.txt           # Binder environment (numpy, pandas, matplotlib, jupyter)
├─ runtime.txt                # Binder runtime (python-3.11)
├─ posts/
│   └─ anchored-kf-markovpd.qmd  # Full step-by-step Quarto document
├─ outputs/                   # Generated CSVs + PNGs (after running)
│   ├─ transition\_matrices**.csv
│   ├─ macro\_paths\_*.csv
│   ├─ pd\_term\_structures\_*.csv
│   ├─ macro\_filter\_*.png
│   ├─ pd\_bands\_\*.png
│   ├─ variance\_Y\_across\_scenarios.csv
│   ├─ lifetime\_loss\_volatility\_mc.csv
│   └─ appendix figures (boxplots, violins, bars)

````

---

## ▶️ How to run

### 1. Locally (with Python)
```bash
# clone repo
git clone https://github.com/<yourname>/<yourrepo>.git
cd <yourrepo>

# install deps
pip install -r requirements.txt

# run the main script or open the Quarto page
quarto render posts/anchored-kf-markovpd.qmd --execute
````

Results will be in `outputs/` and embedded into the rendered HTML.

---

### 2. On the website (with Binder/Thebe)

* Visit the published Quarto page.
* Each code block has a **Run ▶️ button**.
* First click will launch Binder (takes \~1–2 minutes).
* You can then run, edit, and explore the model in the browser — no install needed.

---

## 📊 Key outputs

* **Macro filter figures** (`macro_filter_*.png`) — show raw vs filtered macro index.
* **PD bands** (`pd_bands_*.png`) — cumulative default probabilities across scenarios.
* **Variance table** (`variance_Y_across_scenarios.csv`) — variability of PDs across scenarios.
* **Lifetime loss volatility** (`lifetime_loss_volatility_mc.csv`) — mean ± std of final PD across 200 Monte Carlo runs.
* **Appendix figures** — boxplots, violins, and jittered scatter for the distribution of lifetime PDs.

---

## ⚙️ Model design principles

* **Determinism**: Seeds controlled via `zlib.crc32` (not salted `hash()`).
* **Minimal stack**: No obscure libraries; reproducible on Binder.
* **Stepwise pedagogy**: Each block has description → code → saved figure.
* **Portability**: Outputs written to `outputs/`, not hard-coded `/mnt/data`.

---

## 🚧 Common pitfalls

* **TypeError: 'type' object is not subscriptable**
  → Use Python ≥3.9 (or change `tuple[np.ndarray,...]` → `Tuple[np.ndarray,...]` with `from typing import Tuple`).

* **GitHub Actions fails (`nbformat` missing)**
  → We don’t execute on build. Each page sets:

  ```yaml
  execute:
    enabled: false
  ```

  Code is executed only locally or via Thebe/Binder.

* **Binder slow to start**
  → First spin-up takes 1–2 minutes. Be patient; reuse the same tab for instant reruns.

---

## 📚 Further exercises

Try modifying:

* **Stress severity**: scale up/down the `β` values in `_build_betas()`.
* **Anchor timing**: set `T_anchor` earlier/later and compare PD bands.
* **Macro noise**: increase Gaussian noise (0.2 → 0.4) in scenario generation.

Each change illustrates sensitivity of PIT transitions and PD volatility.

---

## 📚 Citation

If using this code in academic work:

```
Rostampour, V. (2025). Stabilising Lifetime PD Models under Forecast Uncertainty.  
GitHub repository: https://github.com/research-lab/lifetime-PD-AKF
```


---

## 🙋‍♂️ Author

Dr. Vahab Rostampour
Senior Quantitative Analyst
📍 Zurich, Switzerland

