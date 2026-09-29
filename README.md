# Thunderstorm Forecasting

A machine-learning application for predicting thunderstorm occurrence from eight atmospheric indices. The project uses a saved scikit-learn K-nearest neighbors model, a FastAPI prediction service, and a Streamlit interface.

## Project Structure

- `streamlit_app/ui.py` - Streamlit interface for entering model features and viewing predictions.
- `api/main.py` - FastAPI application with the prediction endpoint.
- `app/predictor.py` - Builds the model input and formats prediction results.
- `app/model_loader.py` - Loads the serialized model.
- `models/KNN_best_model.pkl` - Model artifact required to serve predictions.
- `data/` - Raw and processed project datasets.


## Requirements

- Python 3.12 or newer
- The model artifact at `models/KNN_best_model.pkl`

## Installation

From the project root, create and activate a virtual environment, then install dependencies:

```command prompt
uv venv .venv
.venv/Scripts/activate
pip install -r requirements.txt
```

## Run the Application

Open two terminals in the project root. Start the API in the first terminal:

```cmd
uvicorn api.main:app --host 0.0.0.0 --port 8000
```

The API is available at `http://localhost:8000`. Its interactive documentation is at `http://localhost:8000/docs`.

Start the Streamlit interface in the second terminal:

```cmd
streamlit run streamlit_app/ui.py
```

Open the local URL printed by Streamlit, typically `http://localhost:8501`.

## Making a Prediction

Enter values for all eight model features in the Streamlit interface and select **Predict**. The API accepts the same fields as JSON at `POST /predict`:

```json
{
	"SWEAT_index": 0,
	"K_index": 0,
	"Totals_totals_index": 0,
	"Environmental_Stability": 0,
	"Moisture_Indices": 0,
	"Convective_Potential": 0,
	"Temperature_Pressure": 0,
	"Moisture_Temperature_Profiles": 0
}
```

The response contains `prediction` (the model's predicted class) and `probability` (the model's probability for class `1`). Values of zero are valid numeric inputs, not an instruction to return class `0`; use observed feature values for meaningful predictions.

## Troubleshooting

- If the UI reports an API error, make sure the FastAPI server is running at `http://localhost:8000`.
- If the API cannot load the model, check that `models/KNN_best_model.pkl` exists and start Uvicorn from the project root.
- The model path is configured in `app/config.py` as a relative path from the project root.
c      