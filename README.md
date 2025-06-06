# EC2-deploy

Absolutely! Here's a **complete step-by-step guide** for deploying a **MERN app from GitHub to an EC2 instance**, including how to set up the `.env` file and all commands clearly explained.

---

## 🔧 MERN Project Structure Assumption:

```
mern-project/
├── backend/
│   ├── server.js (or index.js)
│   ├── .env
│   └── package.json
├── frontend/
│   ├── src/
│   ├── package.json
│   └── build/ (created after build)
```

---

## ✅ Step-by-Step: MERN Deployment to EC2

---

### 🧩 1. SSH into EC2 Instance

```bash
ssh -i "your-key.pem" ubuntu@<your-ec2-public-ip>
```

---

### ⚙️ 2. Install Required Software

```bash
# Update system
sudo apt update && sudo apt upgrade -y

# Install Node.js and npm
curl -fsSL https://deb.nodesource.com/setup_18.x | sudo -E bash -
sudo apt install -y nodejs

# Install Git and MongoDB (local MongoDB optional if using Atlas)
sudo apt install git mongodb -y
```

---

### 📦 3. Clone Your GitHub Repository

```bash
git clone https://github.com/your-username/your-mern-project.git
cd your-mern-project
```

---

### 🛠️ 4. Setup Backend

```bash
cd backend
npm install
```

---

### 🔐 5. Create `.env` File in `backend/`

```bash
nano .env
```

Paste something like this (change values as per your app):

```
PORT=5000
MONGO_URI=mongodb+srv://<username>:<password>@cluster.mongodb.net/mydb?retryWrites=true&w=majority
JWT_SECRET=mySuperSecret
```

> Press `CTRL+O` → `Enter` to save
> Press `CTRL+X` to exit

---

### 🚀 6. Start Backend

**Option 1: Development**

```bash
node server.js   # or index.js / app.js
```

**Option 2: Production (Recommended)**

```bash
sudo npm install -g pm2
pm2 start server.js --name backend
pm2 save
```

---

### 💻 7. Setup Frontend

```bash
cd ../frontend
npm install
npm run build
```

---

### 🌐 8. Serve Frontend

#### Option A: Serve using `serve`

```bash
sudo npm install -g serve
serve -s build -l 3000
```

#### Option B: Serve with Nginx (recommended for production)

```bash
sudo apt install nginx -y
sudo rm -rf /var/www/html/*
sudo cp -r build/* /var/www/html/
sudo systemctl restart nginx
```

Now visit:
**http\://<your-ec2-public-ip>**

---

### 🔓 9. Open EC2 Ports in Security Group

Go to AWS Console → EC2 → Security Groups → Inbound Rules:

| Type       | Port | Description                       |
| ---------- | ---- | --------------------------------- |
| SSH        | 22   | For terminal access               |
| HTTP       | 80   | For frontend via browser          |
| Custom TCP | 5000 | For backend API (optional)        |
| Custom TCP | 3000 | If using `serve` instead of nginx |

---

### 🧠 10. Auto Start on Reboot (Optional)

```bash
pm2 startup
pm2 save
```

---

## ✅ Your App is Live!

* Frontend: `http://<ec2-ip>` (if nginx)
* Backend API: `http://<ec2-ip>:5000/api` (if exposed)

---


