# Brain-Tumor-Detection
"Brain Tumor Detection using CNN with Flask web interface for image classification" <br> <br>
from flask import Flask, request, render_template_string
import numpy as np
from tensorflow.keras.models import load_model
from tensorflow.keras.preprocessing import image
import os

app = Flask(**name**)

# folder to save uploaded images

UPLOAD_FOLDER = "static/uploads"
os.makedirs(UPLOAD_FOLDER, exist_ok=True)
app.config["UPLOAD_FOLDER"] = UPLOAD_FOLDER

# load trained model

model = load_model("model.h5")   # keep model.h5 in same folder as app.py

HTML_PAGE = """

<!DOCTYPE html>

<html>
<head>
    <title>Brain Tumor Detection</title>
    <style>
        body {
            font-family: Arial, sans-serif;
            background: #f4f7fb;
            text-align: center;
            padding: 40px;
        }
        .container {
            background: white;
            width: 450px;
            margin: auto;
            padding: 30px;
            border-radius: 12px;
            box-shadow: 0 4px 15px rgba(0,0,0,0.15);
        }
        h1 {
            color: #222;
        }
        input[type="file"] {
            margin: 20px 0;
        }
        button {
            background: #007bff;
            color: white;
            border: none;
            padding: 10px 18px;
            border-radius: 8px;
            cursor: pointer;
            font-size: 16px;
        }
        button:hover {
            background: #0056b3;
        }
        .result {
            margin-top: 20px;
            font-size: 22px;
            font-weight: bold;
            color: green;
        }
        img {
            margin-top: 20px;
            width: 220px;
            border-radius: 10px;
            border: 1px solid #ccc;
        }
    </style>
</head>
<body>
    <div class="container">
        <h1>Brain Tumor Detection</h1>

```
    <form action="/predict" method="post" enctype="multipart/form-data">
        <input type="file" name="file" required>
        <br>
        <button type="submit">Predict</button>
    </form>

    {% if prediction %}
        <div class="result">Result: {{ prediction }}</div>
    {% endif %}

    {% if image_path %}
        <img src="{{ image_path }}" alt="Uploaded Image">
    {% endif %}
</div>
```

</body>
</html>
"""

def predict_image(img_path):
img = image.load_img(img_path, target_size=(224, 224))
img = image.img_to_array(img)
img = img / 255.0
img = np.expand_dims(img, axis=0)

```
result = model.predict(img)

# binary classification example
if result[0][0] > 0.5:
    return "Tumor Detected"
else:
    return "No Tumor"
```

@app.route("/")
def home():
return render_template_string(HTML_PAGE)

@app.route("/predict", methods=["POST"])
def predict():
if "file" not in request.files:
return render_template_string(HTML_PAGE, prediction="No file selected")

```
file = request.files["file"]

if file.filename == "":
    return render_template_string(HTML_PAGE, prediction="No file selected")

filepath = os.path.join(app.config["UPLOAD_FOLDER"], file.filename)
file.save(filepath)

prediction = predict_image(filepath)
image_path = "/" + filepath.replace("\\", "/")

return render_template_string(
    HTML_PAGE,
    prediction=prediction,
    image_path=image_path
)
```

if **name** == "**main**":
app.run(debug=True) <br>


Then run it like this: <br>
pip install flask tensorflow numpy <br>
python app.py <br>

Open:<br>
http://127.0.0.1:5000/
