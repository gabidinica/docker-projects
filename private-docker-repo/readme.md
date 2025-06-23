# 🐳 Dockerize Node.js App & Push to AWS ECR

## ✅ Pre-requisites

- ✅ Have an **AWS Free Tier account**.
- 📦 Install and configure the **AWS CLI**:

| Platform | Link |
|----------|------|
| **Linux** | [Install AWS CLI on Linux](https://docs.aws.amazon.com/cli/latest/userguide/install-cliv2-linux.html) |
| **macOS** | [Install AWS CLI on macOS](https://docs.aws.amazon.com/cli/latest/userguide/install-cliv2-macOS.html) |
| **Windows** | [Install AWS CLI on Windows](https://docs.aws.amazon.com/cli/latest/userguide/install-cliv2-windows.html) |
| **Configure AWS CLI** | [CLI Configuration Guide](https://docs.aws.amazon.com/cli/latest/userguide/cli-chap-configure.html) |

---

## 📦 Demo Project: Dockerize Node.js App & Push to AWS ECR

### 🛠️ Technologies Used

- Docker  
- Node.js  
- Amazon ECR (Elastic Container Registry)

---

## 📌 Project Description

- Write a `Dockerfile` to build a Docker image for a Node.js app
- Create a private Docker registry using **Amazon ECR**
- Push the Docker image to the private AWS ECR repository

---

## 🔧 Steps

### 1. Create Private ECR Repository

1. Go to [https://aws.amazon.com](https://aws.amazon.com/) and log in.
2. Search for **Elastic Container Registry (ECR)**.
3. Click **Get Started**.
4. Enter a repository name: `my-app`
5. Click **Create Repository**.
6. After creation, click on the repo name. It should be empty initially.
---

### 2. Push Docker Image to AWS ECR

#### 🔐 Login Docker to AWS:

1. Go back to the ECR console → select your repo → click **View push commands**.
2. Copy **step 1** command from the modal:

```bash
aws ecr get-login-password --region eu-central-1 | docker login --username AWS --password-stdin EXAMPLE_ID.dkr.ecr.eu-central-1.amazonaws.com
```

> If successful, you’ll see: `Login Succeeded`

🏷️ Tag and Push the Docker Image:
3. Tag your image with the full ECR repo URL:
```bash
docker tag my-app:1.0 EXAMPLE_ID.dkr.ecr.eu-central-1.amazonaws.com/my-app:1.0
```

4. Confirm image tagging:
```bash
docker images
```

> you’ll see 2 identical images called in different way.

5. Push the tagged image to ECR:
```bash
docker push EXMAPLE_ID.dkr.ecr.eu-central-1.amazonaws.com/my-app:1.0
```

6. Go back to the AWS Console → ECR → refresh to confirm the image was uploaded.

### 🔄 Updating and Rebuilding the Image

1. Make changes to your app in Visual Studio Code or editor of choice.
2. Navigate to the folder containing the `Dockerfile`:
```bash
cd js-app
docker build -t my-app:1.1 .
```

3. Confirm image creation:
```bash
docker images
```

4. Tag the new image for pushing:
```bash
docker tag my-app:1.1 EXAMPLE_ID.dkr.ecr.eu-central-1.amazonaws.com/my-app:1.1
```

5. Push the new image:
```bash
docker push EXAMPLE_ID.dkr.ecr.eu-central-1.amazonaws.com/my-app:1.1
```

6. Refresh your ECR repository page. Now you’ll see *both image versions* (`1.0` and `1.1`) listed.
