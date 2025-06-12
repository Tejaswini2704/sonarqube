# 📺 CI/CD Pipeline with Jenkins & SonarQube for Netflix UI

# Project Architecture
![WhatsApp Image 2025-06-12 at 11 26 23_393e2e0f](https://github.com/user-attachments/assets/1524f9d7-c2f6-42d7-a82a-e57e9b757357)


---

## 🚀 Project Overview

This project demonstrates an automated CI/CD pipeline using **Jenkins** and **SonarQube**, integrated with **Docker**, and built around a **Netflix-style UI** application. It's designed to showcase DevOps practices like automated builds, static code analysis, and containerized deployment.

---

## 🧰 Tech Stack

| Tool        | Purpose                         |
|-------------|----------------------------------|
|  AWS   | EC2 instance  |
|GitHub      | Source code repository           |
| Jenkins     | CI/CD automation                 |
| SonarQube   | Static code analysis             |
| Docker      | Containerization                 |
| Netflix UI  | Sample front-end application     |

---

## 🛠️ Features

- ✅ Jenkins pipeline with GitHub integration  
- ✅ SonarQube code quality checks on each build  
- ✅ Dockerized services for scalable deployment  
- ✅ Trigger pipeline automatically on code changes  
- ✅ Deploy a sample Netflix-style UI as the final output  

---

## 📂 Project Structure

```
📁 sonarqube/
├── docker-compose.yml        # Docker setup for Jenkins & SonarQube
├── jenkins/
│   └── Jenkinsfile           # Pipeline configuration
├── src/
│   └── netflix-ui/           # Frontend project (Netflix clone)
└── README.md                 # Project documentation
```

---

## ⚙️ How It Works

1. 📝 Code is pushed to GitHub  
2. 🔁 Jenkins detects the change via webhook  
3. 🔍 Code is analyzed by SonarQube  
4. 🐳 Build and deployment are done using Docker  
5. 🌐 The Netflix-style UI is deployed locally or on a container platform  

---

## 📦 Prerequisites

- Docker & Docker Compose  
- GitHub account  
- Jenkins and SonarQube Docker images  
- Basic understanding of CI/CD concepts

---

## 🧪 Run the Project

```bash
# Clone the repository
git clone https://github.com/Tejaswini2704/sonarqube.git
cd sonarqube

# Start Jenkins and SonarQube using Docker
docker-compose up -d

# Access Jenkins at http://localhost:8080
# Access SonarQube at http://localhost:9000
```

---

## 🙌 Connect with Me

If you like this project or have any questions, feel free to connect:

👤 **Tejaswini**  
- 🔗 [LinkedIn Profile](https://www.linkedin.com/in/tejaswini-shirke-85aa3a27a)
- 🔗 [GitHub](https://github.com/Tejaswini2704)  
 - 📧 shirketejaswini10@gmail.com 
## ⭐ Like it?

If you found this helpful, star the repository and share it with your network!


