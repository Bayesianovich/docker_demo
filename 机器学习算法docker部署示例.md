# 机器学习算法docker部署实例

1. 创建项目，并转到该项目

mkdir ml1

cd ml1

Vim model.py

```python
import pandas as pd
from sklearn.datasets import load_iris
from sklearn.linear_model import LogisticRegression
import joblib

# 加载数据
data = load_iris()
X, y = data.data, data.target

# 训练模型
model = LogisticRegression(max_iter=200)
model.fit(X, y)

# 保存模型
joblib.dump(model, 'model.pkl')

```

Vim app.py

```python
from flask import Flask, request, jsonify
import joblib

app = Flask(__name__)

# 加载模型
model = joblib.load('model.pkl')

@app.route('/predict', methods=['POST'])
def predict():
    data = request.get_json()
    prediction = model.predict([data['features']])
    return jsonify({'prediction': int(prediction[0])})

if __name__ == '__main__':
    app.run(host='0.0.0.0', port=9000)

```

vim Dockerfile

```dockerfile
# 使用官方 Python 镜像作为基础
FROM python:3.8-slim

# 设置工作目录
WORKDIR /app

# 将当前目录内容复制到容器的 /app 目录下
COPY . /app

# 安装所需的包
RUN pip install --no-cache-dir flask scikit-learn joblib

# 指定端口
EXPOSE 9000

# 当容器启动时运行 app.py
CMD ["python", "app.py"]
```

同一目录下运行model.py得到model.pkl后

![](https://s2.loli.net/2023/10/20/Wf7sPaEB3M2QY91.png)

构建并运行Docker镜像

```
docker build -t ml_app .
docker run -p 9000:9000 ml_app
```

### 测试 API

使用工具（如 curl 或 Postman）向 `http://localhost:5000/predict` 发送 POST 请求，并在请求体中传入 `features`。例如：

curl -X POST -H "Content-Type: application/json" -d '{"features": [1.2, 3.4, 5.6]}' http://localhost:5000/predict

![](https://s2.loli.net/2023/10/20/CMcmVjP37ownS6X.png)