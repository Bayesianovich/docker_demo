# 使用Docker Compose 部署Web 应用程序

1. 创建目录并切换

mkdir flask_web -p

cd flask_web

创建app.py

vim app.py

```python
import time

import redis
from flask import Flask

app = Flask(__name__)
cache = redis.Redis(host='redis', port=6379)

def get_hit_count():
    retries = 5
    while True:
        try:
            return cache.incr('hits')
        except redis.exceptions.ConnectionError as exc:
            if retries == 0:
                raise exc
            retries -= 1
            time.sleep(0.5)

@app.route('/')
def hello():
    count = get_hit_count()
    return 'Hello from Docker! I have been seen {} times.\n'.format(count)

if __name__ == "__main__":
    app.run(host="0.0.0.0", debug=True)
```

vim requirements.txt

```
flask==2.2.3
redis
```

vim Dockerfile

```dockerfile
FROM python:3.7-alpine
ADD . /code
WORKDIR /code
RUN pip install -r requirements.txt
CMD ["python", "app.py"]
```

vim docker-compose.yml

```
version: '3'
services:
  web:
    build: .
    ports:
     - "9000:5000"
    volumes:
     - .:/code
  redis:
    image: "redis:alpine"
```

构建并后台运行应用程序

docker-compose up -d 

可以从http://localhost:9000/ 查看 



停止应用程序，完全删除容器及卷

docker-compose down --