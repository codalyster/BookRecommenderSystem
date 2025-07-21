# 📖 Book Recommender System - Flask API

---

## 🔄 Full Process: From .NET Request to JSON Response

### 📁 1. .NET Client Sends a POST Request

The .NET frontend or backend sends a POST request to your Flask API endpoint:

```
POST http://localhost:5000/recommend
Content-Type: application/json
```

Example JSON body:

```json
{
  "age": 25,
  "gender": "Male",
  "country": "Egypt",
  "is_new_muslim": "No",
  "born_muslim": "Yes",
  "education_level": "Bachelor",
  "religious_level": "Moderate",
  "preferred_topic": "Faith"
}
```

---

### 🧬 2. Flask API Receives the Request

Your Flask app has an endpoint defined like this:

```python
@app.route("/recommend", methods=["POST"])
def recommend():
    data = request.json
```

The incoming JSON is parsed and stored as a Python dictionary.

---

### 🔮 3. Preprocess the Input

* The dictionary is converted into a DataFrame:

```python
user_df = pd.DataFrame([data])
```

* Then it's encoded and scaled using saved preprocessing tools (e.g., joblib-encoded encoders and scalers):

```python
user_df["gender"] = gender_encoder.transform(user_df["gender"])
user_df_scaled = scaler.transform(user_df[numerical_features])
```

---

### 🤖 4. Load the Model and Predict

* The saved model (e.g., TensorFlow/Keras) is loaded:

```python
model = load_model("model.h5")
prediction = model.predict([user_inputs])
```

* The model returns predictions for all books.

---

### 📚 5. Select Top Recommended Books

* The predicted scores are sorted and top N indices are selected:

```python
top_indices = prediction[0].argsort()[::-1][:5]
recommended_books = books_df.iloc[top_indices]
```

---

### 📤 6. Return Recommendations as JSON Response

* Flask sends the selected recommendations back to the .NET client as JSON:

```python
return jsonify({
    "recommendations": recommended_books.to_dict(orient="records")
})
```

---

## ✅ Summary of Steps

| Step | Description                    |
| ---- | ------------------------------ |
| 1    | Receive POST request from .NET |
| 2    | Parse incoming JSON            |
| 3    | Preprocess data (encode/scale) |
| 4    | Feed data into trained model   |
| 5    | Get top-N predictions          |
| 6    | Return recommendations as JSON |

---

## 🏦 Hosting

This API is typically run locally for testing:

```
python app.py
```

Visit: `http://localhost:5000/recommend`

Make sure the following files exist:

* `model.h5` (trained model)
* `encoder.pkl`, `scaler.pkl` (preprocessing tools)
* `books.csv` or any book metadata file

---
