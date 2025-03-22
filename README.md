# Depth Prediction Model API

## Project Overview
This project implements a machine learning model for depth prediction using Ridge Regression. The model is deployed as a REST API using FastAPI and can be hosted on Vercel.

## Technical Stack
- **Machine Learning**: scikit-learn (Ridge Regression)
- **API Framework**: FastAPI
- **Deployment**: Vercel
- **Dependencies**: 
  - fastapi
  - uvicorn
  - scikit-learn
  - joblib
  - pydantic

## Model Details
### Ridge Regression Model
- The model uses Ridge Regression with polynomial features (degree 4)
- Hyperparameters:
  - alpha: 0.1 (regularization strength)
  - solver: cholesky
  - fit_intercept: True

### Input Features
The model accepts three input parameters:
- param1(date)
- param2(loactation)
- param3(Previous water level prediction)

These parameters are transformed using polynomial features up to degree 4 to capture non-linear relationships in the data.

### Quick Reference
- `GET /`: Health check endpoint
- `POST /predict`: Single prediction endpoint
- `GET /model/info`: Model information and metadata

### Endpoint Details

#### Health Check
`GET /`
- Returns API status
- No parameters required
- Response: `{"status": "ok"}`

#### Predict Water Level
`POST /predict`
- Input:
  ```json
  {
    "date": "2024-03-15",
    "location": "Bilaspur",
    "previous_level": -12.5
  }
  ```
- Output:
  ```json
  {
    "prediction": -12.8,
    "location": "Bilaspur",
    "timestamp": "2024-03-15"
  }
  ```

#### Model Information
`GET /model/info`
- Returns model configuration and training details
- Includes supported locations and feature specifications

### Error Codes
- `400`: Invalid input
- `422`: Validation error
- `500`: Server error

## Dataset
The model was trained on Piezometer DWLR (Digital Water Level Recorder) Data from Bilaspur District, available (https://docs.google.com/spreadsheets/d/1kx9aI-T6hKZNK3xkSmZ-PXf0by-JGOssTHLLtIfyNm8/edit?usp=sharing).

### Dataset Features
- **param1**: Date (YYYY-MM-DD format)
- **param2**: Location ID (categorical)
- **param3**: Previous day's water level (meters)
- **Target**: Water level depth (meters)

### Dataset Statistics
- Temporal coverage: Daily readings
- Spatial coverage: Multiple locations in Bilaspur District
- Locations monitored: 7 sites (Bilaspur, Seepat, Belha, Takhatpur, Kathakoni, Ratanpur, Tendua)
- Measurement type: Automated readings from DWLR sensors.
- dataset from jan 2023 -may 2024

### Data Collection Method
- Digital Water Level Recorders (DWLR) installed in piezometer wells
- Automated daily measurements
- Negative values indicate depth below ground level
- Readings are quality-checked and validated

### File Structure
The dataset is organized in an Excel file with the following structure:

1. **Primary Sheet - "BSP Data"**:
   - Serial Number
   - Date (YYYY-MM-DD)
   - Location-wise water level readings
   - Remarks column for annotations

### Applications
This dataset enables:
- Groundwater level prediction
- Seasonal trend analysis
- Water resource management
- Drought and flood risk assessment
- Environmental impact studies

### Data Preprocessing
For the machine learning model:
1. Missing values are interpolated using time-series methods
2. Features are normalized to standard scale
3. Categorical variables (locations) are one-hot encoded
4. Time features are extracted from dates (month, season, etc.)

