---

```markdown
# 🧮 Angular Calculator (Docker + Nginx)

This is a calculator web application built with Angular, containerized using Docker, and served via Nginx. It is designed for fast, production-ready deployment using a multi-stage Docker build.

---

## 🚀 Live via Docker + Nginx

The app is built with Node.js, and the production build is served using a lightweight Nginx server.

- 🧱 Built using: Angular CLI
- 🌐 Served using: Nginx
- 🐳 Containerized using: Docker
- 🖥️ Accessible at: [http://localhost](http://localhost)

---

## 📁 Project Structure

```

AngularCalculator/
├── Dockerfile
├── nginx.conf
├── src/
├── dist/
└── ...

````

---

## 🛠️ Prerequisites

- [Docker](https://www.docker.com/products/docker-desktop)
- (Optional) [Node.js](https://nodejs.org/) + Angular CLI for local dev

---

## 🐳 Docker Deployment Instructions

### 1️⃣ Clone the Repository

```bash
git clone https://github.com/CeeyIT-Solutions/AngularCalculator.git
cd AngularCalculator
````

### 2️⃣ Dockerfile Setup

Your `Dockerfile` should look like this:

```dockerfile
# Stage 1: Build Angular app
FROM node:18 AS builder

WORKDIR /app
ENV NODE_OPTIONS=--openssl-legacy-provider

COPY package*.json ./
RUN npm install

COPY . .
RUN npm run build --prod

# Stage 2: Serve with Nginx
FROM nginx:alpine

COPY --from=builder /app/dist/angular-calculator /usr/share/nginx/html
COPY nginx.conf /etc/nginx/conf.d/default.conf

EXPOSE 80
CMD ["nginx", "-g", "daemon off;"]
```

> 🔁 Make sure `angular-calculator` matches the output directory from your `dist/` folder. Adjust if necessary based on your actual app name.

---

### 3️⃣ Nginx Configuration

Create a file named `nginx.conf` in the root of your project:

```nginx
server {
    listen 80;
    server_name localhost;

    root /usr/share/nginx/html;
    index index.html;

    location / {
        try_files $uri $uri/ /index.html;
    }
}
```

This ensures that Nginx serves your single-page app properly by routing all paths to `index.html`.

---

### 4️⃣ Build the Docker Image

```bash
docker build -t yourusername/angular-calculator .
```

### 5️⃣ Run the Docker Container

```bash
docker run -d -p 80:80 --name angular-calculator yourusername/angular-calculator
```

Then open your browser and navigate to:

```
http://localhost
```

---

### 6️⃣ Push to Docker Hub

```bash
docker login
docker tag yourusername/angular-calculator yourusername/angular-calculator:latest
docker push yourusername/angular-calculator:latest
```

---

## ⚠️ Common Issues

### ❗ OpenSSL Crypto Error

If you see this:

```
Error: error:0308010C:digital envelope routines::unsupported
```

Add this to your Dockerfile:

```dockerfile
ENV NODE_OPTIONS=--openssl-legacy-provider
```

This fixes Webpack’s crypto issues in Node.js 17+.

---

## 🧪 Development Mode (Angular CLI)

You can still run the app locally using Angular CLI:

```bash
npm install
ng serve
```

Visit [http://localhost:4200](http://localhost:4200)

---

## 🧼 Clean Production Build

```bash
ng build --prod
```

The output will be stored in `dist/angular-calculator`.

---


## 📸 Screenshots to Include

* ✅ Dockerfile contents
* ✅ nginx.conf contents
* ✅ Working app at [http://localhost](http://localhost)
* ✅ Docker Hub image page

```
```
