
# Warehouse Location Optimization for The Good Acre

This project helps The Good Acre, a nonprofit food aggregator in Minnesota, identify the most impactful new warehouse locations across five candidate counties. We built a scoring framework using agricultural census data and supplier proximity, combined with spatial distance analysis and interactive visualizations to support data-informed expansion planning.

## 📍 Problem Overview

The Good Acre considered five candidate counties—Owatonna, Wilmar, Fergus Falls, Rochester, and Duluth—for opening new aggregation hubs. The objective was to select the most effective location(s) based on proximity to farms, support for marginalized groups, and regional supply chain presence.

## 🧠 Methodology

- **Data Integration**: Combined USDA census data, supplier locations, and farm profiles.
- **Distance Calculation**: Used OpenRouteService API (hosted on a local Docker instance) to compute road network distances from each supplier to all warehouse sites.
- **Scoring Framework**: Built a two-level weighting model across farm types (emerging, mature, high-yield) and impact metrics (acreage, supplier density, marginalized farms).
- **Visualization**: Developed an interactive Tableau dashboard to explore trade-offs and simulate different stakeholder preferences.

## 🔧 Code Highlights

Each supplier’s road distance to 7 potential warehouse sites was calculated using the following logic:

```python
url = "http://localhost:8080/ors/v2/directions/driving-car"

params = {
    "start": f"{row['Longitude']},{row['Latitude']}",
    "end": "-93.167149,44.990372"  # Falcon Heights example
}

response = requests.get(url, params=params)
if response.status_code == 200:
    data = response.json()
    distance = data["features"][0]["properties"]["segments"][0]["distance"]
```

The shortest distance was retained to compute metrics like average and median accessibility per county.

## 🧮 Scoring Model

We used a customizable scoring framework:

**Level 1 – Farm Type Weights**
- Emerging: 50%
- Mature: 20%
- High-Yield: 30%

**Level 2 – Impact Metrics**
- Number of farms
- Total acreage
- Share of marginalized farmers
- Existing supplier and buyer counts

Final county scores were calculated as a weighted sum of normalized values across these dimensions.

## 📊 Dashboard Preview

![Warehouse Tableau Dashboard](Images/Tableau%20Dashboard.png)

The interactive Tableau dashboard allows:
- Adjusting weight sliders at both levels
- Viewing map-based warehouse reach
- Ranking counties by computed score

## 🧭 Scoring Logic Overview

![Scoring Workflow](Images/Equation%20workflow.png)

This diagram outlines how Level 1 and Level 2 weights feed into the overall scoring formula and county rankings.

## ✅ Outcome

Based on our framework, **Owatonna (Steele County)** emerged as the top candidate due to its strong accessibility profile and farm density. The tool also supports scenario testing to adjust priorities based on The Good Acre’s evolving goals.