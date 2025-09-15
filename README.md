# Samsung Mobile Website

A simple static website for **Samsung Mobile**, containerized with **Docker** and automated using **Jenkins Pipeline**.

## 🚀 Run Locally
```bash
docker build -t samsung-site .
docker run -d -p 8080:80 samsung-site

