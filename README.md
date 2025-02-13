# Euro-park: Attendance and Waiting Time Forecast

## Project Context

**Euro-park**, a global theme park, is facing a significant increase in attraction waiting times, negatively impacting visitor satisfaction. The goal of this project is to:

1. **Predict** waiting times (or attendance) for attractions accurately, over the medium and long term, while excluding non-representative periods (e.g., the Covid period).
2. **Identify** ways to improve the park’s KPIs using these forecasts (better visitor flow management, resource optimization, etc.).

We have multiple data sources:
- **waiting_times.csv**: waiting times for each attraction (in 15-minute intervals).
- **attendance.csv**: the park’s overall daily attendance.
- **entity_schedule.csv**: attraction opening/closing times and maintenance periods.
- **weather.csv**: hourly weather data.
- **parade_night_show.csv**: parade and night show schedules.
- **link_attraction_park.csv**: reference table for attractions and their locations.

These different sources were cleaned, enriched, and merged into a **unified dataset** used in our forecasting models.

---

## General Organization

The project is structured into several Jupyter notebooks, each focusing on a specific part of the data science pipeline:

1. **preprocessing.ipynb**  
   - Combines and cleans the different data sources (waiting_times, attendance, weather, etc.).
   - Handles the exclusion of non-representative periods (e.g., Covid period).
   - Produces a **unified dataset** (e.g., `merged_datasets_with_events.csv`) that serves as the foundation for the modeling notebooks.

2. **attendence-forecast.ipynb**  
   - Focuses on forecasting the park’s monthly or daily attendance (complementary to `prophet.ipynb`).
   - Uses historical data (excluding the Covid period) to build a time series model and estimate future attendance.

3. **prophet.ipynb**  
   - Implements the Prophet method (Facebook/Meta library) for time series forecasting.
   - Compares or complements the results from `attendence-forecast.ipynb`.
   - Allows visualization of the predictions (trend, seasonality, etc.).

4. **light_gbm.ipynb**  
   - Sets up a LightGBM model to predict waiting times or attendance.
   - Compares performance with other algorithms (XGBoost, Prophet, etc.).
   - Evaluates accuracy (MSE, RMSE, MAE, etc.) and execution speed.

5. **xgboost.ipynb**  
   - Sets up an XGBoost model for prediction (similar to LightGBM, but with a different internal approach).
   - Compares performance with LightGBM and other methods.

6. **xgboost_copie.ipynb**  
   - An additional test/experimental version of XGBoost.
   - May contain variations in hyperparameters or other experimental settings.
   - Not used in the final pipeline but kept for reference.

7. **Auto_ML.ipynb**  
   - Automates the process of selecting and evaluating various machine learning algorithms.
   - Tests several models (including Distributed Random Forest, Gradient Boosted Trees, etc.).
   - Compares results using the chosen metric (e.g., RMSE) and helps choose the best model.

8. **README.md**  
   - This file: documents the project structure, context, explanation of the notebooks, and the methodology used.

9. **requirements.txt**  
   - Lists Python dependencies and their versions (libraries used in the notebooks).
   - Allows you to recreate the environment needed to run the project (via `pip install -r requirements.txt`).

---

## Methodology

1. **Data Collection and Cleaning**  
   - Raw data (waiting times, attendance, weather, etc.) is imported and combined.  
   - Outliers, duplicates, and the Covid period (non-representative) are removed or filtered out.

2. **Feature Engineering**  
   - Creation of relevant variables (e.g., calendar effects, school holidays, special events, weather variables, etc.).
   - Aggregation to the desired granularity (daily, monthly, or 15-minute intervals).

3. **Model Selection and Comparison**  
   - **Time series models**: Prophet, ARIMA, etc., for attendance forecasting.  
   - **Supervised models**: LightGBM, XGBoost, Distributed Random Forest (via Auto_ML) for waiting time prediction.
   - **Evaluation**: use of metrics such as MSE, RMSE, or MAE to compare performance.

4. **Best Model Selection**  
   - After comparison, we selected a **Distributed Random Forest** (via Auto_ML) for its performance and speed.
   - The final predictions are integrated into an **API** or **Power BI dashboard** to be easily utilized by the client.

5. **Deployment**  
   - The notebooks are arranged sequentially to facilitate reuse.  
   - Forecasts can be generated on a regular basis (e.g., daily/weekly) to adjust park management in real-time or over the medium term.

---

## Usage

1. **Clone the repository**:  
   ```bash
   git clone <REPO_URL>
   cd euro-park-waiting-times
