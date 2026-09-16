# SpaceX Falcon 9 First Stage Landing Prediction

A data science capstone project predicting whether a SpaceX Falcon 9 first stage will land
successfully, based on launch data collected via the SpaceX REST API and Wikipedia web scraping.

## Why This Matters

SpaceX advertises Falcon 9 launches at a fraction of other providers' cost, largely because it
reuses the first stage instead of discarding it after every launch. Reliably predicting landing
success ahead of time makes it possible to estimate launch cost in advance — useful for any
company evaluating a bid against SpaceX.

**Key questions this project answers:**
- How do payload mass and orbit type relate to landing success?
- Has the success rate improved over time?
- Which combination of features best predicts a successful landing?

## Project Structure

| File | Purpose |
|---|---|
| `IBM_Data_Science_Certification_Final_Project_API.ipynb` | Collects launch data from the SpaceX REST API |
| `IBM_Data_Science_Certification_Final_Project_Web_Scraping.ipynb` | Scrapes supplementary launch data from Wikipedia |
| `IBM_Data_Science_Certification_Final_Project_Data_Wrangling.ipynb` | Cleans data and engineers the binary landing-outcome target |
| `EDA_with_Visualization_Lab.ipynb` | Visual exploration of launch site, orbit, payload, and outcome relationships |
| `Interactive_Visual_Analytics_and_Dashboard` | Folium map of launch sites/outcomes; source for the Dash app below |
| `spacex-dash-app.py` | Interactive Plotly Dash dashboard |
| `Machine_Learning_Prediction.ipynb` | Trains and tunes four classification models |
| `IBM Data Science Capstone_Raj_Sep-16.pdf.pptx` | Final presentation deck |

## Methodology

1. **Data Collection** — SpaceX REST API for structured launch records, cross-checked and
   supplemented with Wikipedia web scraping
2. **Data Wrangling** — missing-value handling, GTO orbit filtering, binary `Class` target
   engineering (1 = successful landing, 0 = not)
3. **EDA (visualization)** — relationships between flight number, payload mass, orbit type,
   launch site, and outcome
4. **EDA (SQL)** — 10 targeted queries against the wrangled dataset (launch site distribution,
   payload totals by customer/booster, landing outcome breakdowns, etc.)
5. **Interactive Visual Analytics** — a Folium map plotting every launch site and outcome
   geographically, plus proximity analysis to nearby infrastructure
6. **Interactive Dashboard** — a Plotly Dash app (see below) for on-demand exploration
7. **Predictive Analysis** — four classifiers (Logistic Regression, SVM, Decision Tree, KNN),
   each tuned via `GridSearchCV` with 10-fold cross-validation, compared on held-out test data

## Dashboard (`spacex-dash-app.py`)

**Plots:**
- Pie chart — total successful launches by site (all sites), or success/failure split for one
  selected site
- Scatter chart — Payload Mass vs. landing outcome, colored by Booster Version Category

**Interactions:**
- Site dropdown — switch between an all-sites aggregate view and a single-site view
- Payload range slider — filter the scatter chart to a specific payload mass band

These were chosen to let a reviewer move from a high-level "which sites succeed most" view down
to "does payload mass relate to outcome for this specific site," without touching any code.

## Key Findings

- Launch success rate has improved substantially since 2013, consistent with SpaceX's growing
  landing expertise
- Certain orbit types (ES-L1, GEO, HEO, SSO, VLEO) show a 100% success rate in this dataset;
  GTO and PO trail behind
- KSC LC-39A and CCAFS SLC-40 host the majority of launches
- All four tuned classifiers reach the same 0.8333 test-set accuracy on the (small, 18-sample)
  test set; ranking by cross-validation score instead points to Decision Tree as the more
  robust model


## Running the Dashboard

```bash
pip install dash pandas plotly
python spacex-dash-app.py
```
Then open `http://localhost:8050` in a browser.

## Presentation

A full slide deck (`IBM Data Science Capstone_Raj_Sep-16.pdf.pptx`) walks through the methodology and
results end to end, including the flowcharts, EDA visualizations, SQL query results, and
classification comparison summarized above.
