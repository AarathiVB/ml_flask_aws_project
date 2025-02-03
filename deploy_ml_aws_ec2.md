## **🚀 Step-by-Step Guide to Deploy an ML Model on AWS EC2 using Flask**
This guide covers: **EC2 setup, Security Group configuration, SSH access (PuTTY, WinSCP), and Flask deployment.**

## Model Details:

### Salary Prediction using Linear Regression
1. Developed using sklearn
linear_model --> LinearRegression
2. Model Serializer
pickle
3. Web Framework / API Handler
Flask
4. Deployment
AWS

Steps:
1. Train our Model
2. Create Web App using Flask
3. Create AWS account
4. Create EC2 instance
5. Download putty and putty gen
6. Generate private key with putty gen
7. Download WinSCP
8. Update the port in Flask code
9. Install the libraries by connecting through putty
10. python3 app.py
11. Web url and verify it

---

## **1️⃣ Create an AWS EC2 Instance**
1. **Log in to AWS Console** → Go to **EC2**.
2. Click **Launch Instance**.
3. **Choose Amazon Machine Image (AMI)**:
   - Select **Ubuntu Server 24.04 LTS** (or the latest Ubuntu free-tier eligible AMI).
4. **Choose Instance Type**:
   - **t2.micro** (Free tier eligible, 1 vCPU, 1 GiB RAM).
5. **Create or Select a Key Pair**:
   - Click **Create new key pair**.
   - Name it **ML_DEPLOY_KEY**.
   - Choose **RSA, .pem format**.
   - Click **Create key pair** (download `.pem` file).
6. **Configure Security Group**:
   - **Create a new security group** (e.g., `ML_DEPLOY_SG`).
   - Allow:
     - **SSH (Port 22) → Source: Your IP (`My IP`)**
     - **Custom TCP (Port 8080) → Source: `0.0.0.0/0`** (for Flask app).
7. **Launch the Instance**.
8. Go to **EC2 Dashboard** → Click **Instances** → Copy **Public IPv4 Address**.

---

## **2️⃣ Configure Security Group to Allow Flask (Port 8080)**
1. Go to **EC2 Dashboard** → **Security Groups**.
2. Select **ML_DEPLOY_SG**.
3. Click **Edit inbound rules**.
4. Add the following rules:
   - **Type:** `SSH`, **Port:** `22`, **Source:** `My IP`
   - **Type:** `Custom TCP`, **Port:** `8080`, **Source:** `0.0.0.0/0`
5. Click **Save Rules**.

---

## **3️⃣ Convert `.pem` Key to `.ppk` Using PuTTYgen**
1. **Open PuTTYgen**.
2. Click **Load** → Select **ML_DEPLOY_KEY.pem**.
3. Click **Save private key** → Save as **ML_DEPLOY_KEY.ppk**.

---

## **4️⃣ Connect to EC2 via PuTTY**
1. Open **PuTTY**.
2. In **Host Name**, enter:
   ```bash
   ubuntu@your-ec2-public-ip
   ```
3. In **Connection → SSH → Auth**, load **ML_DEPLOY_KEY.ppk**.
4. Click **Open** → **Accept** if prompted.
5. You are now logged into your EC2 instance!

---

## **5️⃣ Transfer ML Model to EC2 via WinSCP**
1. Open **WinSCP**.
2. **File Protocol**: `SFTP`
3. **Host Name**: `your-ec2-public-ip`
4. **User Name**: `ubuntu`
5. **Private Key File**: Load **ML_DEPLOY_KEY.ppk**.
6. Click **Login**.
7. **Upload your Flask project** (including `app.py`, `model.pkl`, `requirements.txt`) to `/home/ubuntu/`.

---

## **6️⃣ Install Required Packages on EC2**
In PuTTY (SSH terminal), run:

```bash
sudo apt update
sudo apt install python3-pip python3-venv -y
```

### **Create a Virtual Environment and Install Dependencies**
```bash
cd /home/ubuntu/
python3 -m venv ml_aws_venv
source ml_aws_venv/bin/activate
pip install -r requirements.txt
```

---

## **7️⃣ Run Flask App**
Modify `app.py` to ensure it runs on **0.0.0.0:8080**:

```python
app.run(host="0.0.0.0", port=8080)
```

Then start the Flask app:
```bash
python3 app.py
```

To keep it running in the background:
```bash
nohup python3 app.py --host=0.0.0.0 --port=8080 &
```

---

## **8️⃣ Test the Deployment**
Open in your browser:
```
http://your-ec2-public-ip:8080
```
Your ML model should now be accessible via the Flask web app! 🚀

---

## **✅ Summary of Steps**
| Step | Action |
|------|--------|
| **1** | Create AWS EC2 instance (Ubuntu, t2.micro, key pair) |
| **2** | Configure Security Group (Allow SSH 22, Flask 8080) |
| **3** | Convert `.pem` to `.ppk` using PuTTYgen |
| **4** | Connect to EC2 via PuTTY |
| **5** | Transfer ML model files using WinSCP |
| **6** | Install Python, dependencies, and virtual environment |
| **7** | Run Flask app (`python3 app.py --host=0.0.0.0 --port=8080`) |
| **8** | Open in browser (`http://your-ec2-public-ip:8080`) |

Now your **ML model is deployed on AWS EC2!** 🎉 Let me know if you need further help! 🚀🔥
