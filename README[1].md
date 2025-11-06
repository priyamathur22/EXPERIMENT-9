# 🚀 React App with Multi-Stage Docker Build

This project demonstrates how to create a **production-ready Docker image** for a React app using **multi-stage builds**.

## 🧠 Objective
To build a smaller, optimized Docker image that separates:
- **Build stage (Node.js)** – installs dependencies and compiles React app
- **Runtime stage (Nginx)** – serves the compiled app

## 🧱 Folder Structure
```
my-react-app/
├── Dockerfile
├── .dockerignore
├── package.json
├── public/
├── src/
└── README.md
```

## 🐳 Docker Commands

### Build the image
```bash
docker build -t react-multistage-app .
```

### Run the container
```bash
docker run -p 80:80 react-multistage-app
```

Then open your browser and visit:
👉 **http://localhost**

## ✅ Expected Output
- A working Docker container serving your React app  
- Smaller, optimized image size  
- Clear separation between build and production stages
