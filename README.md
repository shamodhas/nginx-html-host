# Deployment of Static Website with NGINX (Ubuntu Server)

![NGINX](https://img.shields.io/badge/NGINX-1.23+-009639?logo=nginx)
![Linux](https://img.shields.io/badge/Linux-Ubuntu-E95420?logo=ubuntu)

> **A practical guide to deploy a static website using NGINX on Ubuntu, served over a custom port (8080).**

---

## 🚀 Deployment Workflow

1. **Configure firewall** to allow HTTP, HTTPS, and custom port.
2. **Prepare static website files.**
3. **Create an NGINX server block** (virtual host).
4. **Enable the site and test the configuration.**
5. **Restart NGINX** to apply changes.
6. **Verify access via browser.**

---

## 🛠️ First Time Server Setup

### 1️⃣ Update System and Install NGINX

```bash
sudo apt update
sudo apt install nginx -y
```

---

### 2️⃣ Start and Enable NGINX Service

```bash
sudo systemctl start nginx
sudo systemctl enable nginx
sudo systemctl status nginx
```

---

### 3️⃣ Configure Firewall (UFW)

#### Allow HTTP and HTTPS Traffic

```bash
sudo ufw allow 'Nginx Full'
sudo ufw enable
sudo ufw status
```

---

### 4️⃣ Set Permissions for Web Files

```bash
sudo chown -R www-data:www-data /var/www/html
sudo chmod -R 755 /var/www/html
```

---

## 🏗️ Configure NGINX Server Block (Virtual Host)

### 1️⃣ Upload Website Files Using FileZilla

- **Install and Open FileZilla:**  
  Download from [filezilla-project.org](https://filezilla-project.org)

- **Open FileZilla and go to:**  
  `File → Site Manager`

#### Set Up Connection

- **Protocol:** SFTP - SSH File Transfer Protocol  
- **Host:** your-server-ip  
- **Port:** 22  
- **Logon Type:** Normal  
- **Username:** (e.g., ubuntu or root)  
- **Password:** (your server password)  
- Click **Connect**

#### Create Folder on Server

- In the right panel (**Remote site**), go to:
  ```css
  /var/www/html/
  ```
- Right-click inside the directory → **Create Directory**  
- Name it:
  ```vbnet
  custom-site
  ```

#### Upload Your Files

- In the left panel (**Local site**), locate your website folder (e.g., with `index.html`)
- Drag and drop all files into:
  ```css
  /var/www/html/custom-site/
  ```

#### Set Permissions

After uploading, run on terminal:

```bash
sudo chown -R www-data:www-data /var/www/html/custom-site
sudo chmod -R 755 /var/www/html/custom-site
```

---

### 2️⃣ Create NGINX Config File

```bash
nano /etc/nginx/sites-available/custom-port-site.conf
```

```nginx
server {
    listen 8080;
    server_name localhost;

    root /var/www/html/custom-site;
    index index.html;
}
```

---

### 3️⃣ Enable the Site

```bash
sudo ln -s /etc/nginx/sites-available/custom-port-site.conf /etc/nginx/sites-enabled/
```

---

### 4️⃣ Allow the Port Through the Firewall

```bash
sudo ufw allow 8080
```

---

### 5️⃣ Restart NGINX

```bash
sudo systemctl restart nginx
```

---

### 6️⃣ Access Website

Open in your browser:

```text
http://your-vps-ip:8080
```