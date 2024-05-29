# DOcker learning

## overriding docker commands to run shell

```bash
docker run -it rembg-webapp-tutorial-web sh
```

## Deploying python

To deploy on render, we need to make sure to expose specifically port 80, and set that as an environment variable both in docker and in the render dashboard. 

1. Use a dockerfile that looks like this:

  ```docker
  FROM python:3.9
  
  # download this https://github.com/danielgatis/rembg/releases/download/v0.0.0/u2net.onnx
  # copy model to avoid unnecessary download
  RUN mkdir /home/.u2net
  RUN wget -P /home/.u2net https://github.com/danielgatis/rembg/releases/download/v0.0.0/u2net.onnx
  
  WORKDIR /app
  
  COPY requirements.txt .
  
  RUN pip install --no-cache-dir -r requirements.txt
  
  COPY . .
  
  ENV PORT=80
  
  EXPOSE ${PORT}
  
  CMD ["python", "app.py"]
  ```

2. Upload to render as web service using docker
