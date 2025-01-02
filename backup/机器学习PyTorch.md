# 简介

开始学习机器学习了，关注哔哩哔哩的up主[跟李沐学AI](https://space.bilibili.com/1567748478)，学习 **【完结】动手学深度学习 PyTorch版** 合集视频。这篇博客是我对机器学习的补充。

配套资料[《动手学深度学习》](https://zh.d2l.ai/)

# flask应用

```py
from flask import Flask, render_template, request, jsonify

app = Flask(__name__)


@app.route('/')
def home():
    return render_template('home.html')


@app.route('/predictTest', methods=['POST'])
def predictTest():
    image = request.files['image']
    image_tensor = transform(Image.open(image).convert('RGB')).unsqueeze(0)
    net.load_state_dict(torch.load(model_path, map_location=device))
    predicted_class = ML.image_classification_predict(net, labels, image_tensor, device)
    return render_template('predict.html', predicted_class=predicted_class)


@app.route('/predict', methods=['POST'])
def predict():
    image = request.files['image']
    image_tensor = transform(Image.open(image).convert('RGB')).unsqueeze(0)
    net.load_state_dict(torch.load(model_path, map_location=device))
    predicted_class = ML.image_classification_predict(net, labels, image_tensor, device)
    data = {'predicted_class': predicted_class}
    response = jsonify(data)
    response.headers['Access-Control-Allow-Origin'] = '*'
    return response
```