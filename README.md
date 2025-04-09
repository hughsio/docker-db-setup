![UDC SEAS Banner](GITHUB-banner.png)

# Setting Up MySQL with Docker and phpMyAdmin

This guide walks you through setting up a MySQL environment using Docker for containerization and phpMyAdmin for web-based database management. It is designed for beginners and does not require prior experience with Docker or MySQL.

---

## 📋 Prerequisites

Before you begin, ensure the following:

- **Docker** is installed on your computer. You can download it here:  
  👉 [https://docs.docker.com/desktop/](https://docs.docker.com/desktop/)
- Basic familiarity with using the terminal or command prompt.

---

## ⚙️ Step 1: Set Up the MySQL Container

### 1. Install Docker

Follow the installation guide for your operating system here:  
👉 [https://docs.docker.com/desktop/](https://docs.docker.com/desktop/)

### 2. Verify Docker Installation

Open a terminal or command prompt and run:

```bash
docker version
```

You should see version info if Docker is installed correctly.

---

### 3. Pull the MySQL Image

Run this command to download the MySQL 8.0 image:

```bash
docker pull mysql:8.0
```

#### 💡 For Apple Silicon (M1/M2) or ARM users:

```bash
docker pull arm64v8/mysql:8.0
# or
docker pull arm64v8/mysql:latest
```

---

### 4. Create a Docker Network

This allows containers to talk to each other:

```bash
docker network create my-network
```

---

### 5. Run the MySQL Container

Use this command to start your MySQL container:

```bash
docker run -p 3306:3306 --name my-mysql -e MYSQL_ROOT_PASSWORD=MyPassword --network my-network -d mysql:8.0
```

#### Command breakdown:
- `-p 3306:3306`: Exposes MySQL on port 3306 (default).
- `--name my-mysql`: Names your container.
- `-e MYSQL_ROOT_PASSWORD=MyPassword`: Sets the root password. **Change this!**
- `--network my-network`: Uses the shared Docker network.
- `-d`: Runs the container in the background.

#### ⚠️ Port Conflicts?

If port 3306 is already in use (e.g., by a native MySQL installation), choose a different one:

```bash
docker run -p 3456:3306 --name my-mysql -e MYSQL_ROOT_PASSWORD=MyPassword --network my-network -d mysql:8.0
```

Check available ports with:

- **Linux/macOS:** `netstat -tulnp`
- **Windows:** `netstat -ano`

---

### 6. Check if the Container is Running

```bash
docker ps -a
```

You should see a container named `my-mysql` with a status like `Up`.

---

### 7. Access MySQL Terminal

Enter the container:

```bash
docker exec -it my-mysql bash
```

Log in to MySQL:

```bash
mysql --user=root --password=MyPassword
```

Then test it:

```sql
show databases;
```

---

## 🌐 Step 2: Set Up phpMyAdmin

phpMyAdmin is a browser-based MySQL management interface.

### 1. Pull the phpMyAdmin Image

```bash
docker pull phpmyadmin/phpmyadmin:latest
```

> If there's a platform compatibility error, visit [hub.docker.com](https://hub.docker.com/) and search for "phpmyadmin". Select your platform (e.g., `arm64v8`) to get the correct image.

---

### 2. Run phpMyAdmin Container

```bash
docker run --name my-phpmyadmin -d --network my-network --link my-mysql:db -p 8081:80 phpmyadmin/phpmyadmin
```

#### Command breakdown:
- `--name my-phpmyadmin`: Container name
- `--network my-network`: Same network as MySQL
- `--link my-mysql:db`: Links to the MySQL container (legacy syntax, but still used)
- `-p 8081:80`: Access phpMyAdmin at `http://localhost:8081`

---

### 3. Check Container Status

```bash
docker ps -a
```

---

### 4. Access phpMyAdmin

Open your browser and go to:  
👉 [http://localhost:8081](http://localhost:8081)

- **Username:** root  
- **Password:** MyPassword (or whatever you used earlier)

> If the page doesn't load, check that the port is open and the container is running.

---

## 🖥️ Optional: Connect with MySQL Workbench

1. Download and install MySQL Workbench:  
   👉 [https://dev.mysql.com/downloads/workbench/](https://dev.mysql.com/downloads/workbench/)

2. Create a new connection using the **host** `localhost` and the **port** you mapped in Step 1 (e.g., 3306 or 3456).

---

## 🛠️ Troubleshooting

| Issue | Solution |
|------|----------|
| "Platform not supported" | Check Docker Hub for a compatible image |
| Port already in use | Change the `-p` flag to a free port |
| Can't connect via terminal | Make sure the container is up and the password is correct |
| phpMyAdmin won't load | Confirm correct port and network settings |

---

## 🔗 Resources

- [Docker Documentation](https://docs.docker.com/)
- [Docker Hub](https://hub.docker.com/)
- [MySQL Documentation](https://dev.mysql.com/doc/)
- [phpMyAdmin Docker Image](https://hub.docker.com/r/phpmyadmin/phpmyadmin)

---

## ✅ Conclusion

You've now created a fully functional MySQL environment using Docker, complete with a web-based admin interface. This is a flexible and portable way to manage MySQL without the need for local installs.

Remember to change default credentials and secure your setup for production use.