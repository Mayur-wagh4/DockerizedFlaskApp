# **Flask & MySQL App with Docker Integration**  

This project demonstrates a **Flask web application** that interacts with a **MySQL database**, running in a **Dockerized environment**. The application allows users to submit messages, store them in the database, and display them on the frontend.

---

## **📌 Prerequisites**  
Ensure you have the following installed before proceeding:  
✅ **Docker & Docker Compose** (For containerization)  
✅ **Git** (Optional, for cloning the repository)  

---

## **⚙️ Setup Instructions**  

### **1️⃣ Clone the Repository**  
```bash
git clone https://github.com/your-username/your-repo-name.git
cd your-repo-name
```

### **2️⃣ Configure Environment Variables**  
Create a `.env` file in the root directory to store MySQL credentials:  
```bash
touch .env
```
Edit the `.env` file and add the following details:  
```env
MYSQL_HOST=mysql
MYSQL_USER=your_username
MYSQL_PASSWORD=your_password
MYSQL_DB=your_database
```

---

## **🚀 Running the Application**  

### **Using Docker Compose (Recommended)**  
This method **automates** container creation and networking.  
```bash
docker-compose up --build
```
✅ **Access the Application:**  
- **Frontend:** `http://localhost`  
- **Backend:** `http://localhost:5000`  

---

### **Manually Running the Application (Without Docker Compose)**  
If you prefer to start containers individually:  

#### **1️⃣ Build the Flask App Docker Image**  
```bash
docker build -t flaskapp .
```

#### **2️⃣ Create a Custom Network**  
```bash
docker network create flask-network
```

#### **3️⃣ Run the MySQL Database Container**  
```bash
docker run -d \
    --name mysql \
    --network=flask-network \
    -v mysql-data:/var/lib/mysql \
    -e MYSQL_DATABASE=mydb \
    -e MYSQL_ROOT_PASSWORD=admin \
    -p 3306:3306 \
    mysql:5.7
```

#### **4️⃣ Run the Flask Application Container**  
```bash
docker run -d \
    --name flaskapp \
    --network=flask-network \
    -e MYSQL_HOST=mysql \
    -e MYSQL_USER=root \
    -e MYSQL_PASSWORD=admin \
    -e MYSQL_DB=mydb \
    -p 5000:5000 \
    flaskapp:latest
```

---

## **🔹 Setting Up the MySQL Database**  
After running the containers, execute the following SQL command to create the necessary table:  
```sql
CREATE TABLE messages (
    id INT AUTO_INCREMENT PRIMARY KEY,
    message TEXT
);
```
You can use **phpMyAdmin**, **MySQL CLI**, or any MySQL client to run this command.

---

## **🛠 Stopping & Cleaning Up**  
To stop and remove the containers:  
```bash
docker-compose down
```
If running manually, stop containers with:  
```bash
docker stop flaskapp mysql
docker rm flaskapp mysql
docker network rm flask-network
```

---

## **⚠️ Notes & Best Practices**  
- **Use strong credentials** for MySQL in production.  
- **Sanitize user inputs** to prevent SQL injection attacks.  
- **Check logs for troubleshooting:**  
  ```bash
  docker logs flaskapp
  docker logs mysql
  ```
- Consider **using volumes** for persistent database storage in production.

---

## **📌 Summary**  
This project demonstrates how to:  
✅ **Containerize** a Flask + MySQL app  
✅ **Use Docker Compose** for automation  
✅ **Manually manage containers** if needed  
✅ **Deploy a functional two-tier architecture**  

