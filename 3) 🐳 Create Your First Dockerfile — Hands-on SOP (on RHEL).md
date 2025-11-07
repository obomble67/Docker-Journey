# 🐳 **Create Your First Dockerfile — Hands-on SOP (on RHEL)**

---

## 🎯 **Objective**

To manually build and then automate a simple **Nginx web server** using a **Dockerfile**, displaying a **custom HTML message** inside a running container.

You’ll learn:

* How to run and configure a container manually
* How to convert manual steps into a Dockerfile
* How `COPY`, `CMD`, and `EXPOSE` instructions work
* How to build and run your own Docker image

---

## ⚙️ **Section 1: Manual Steps (Before Dockerfile)**

---

### 🧩 Step 1: Run a Base Container

```bash
[omkar67@omkar ~]$ docker container run -dt -p 80:80 --name mynginx ubuntu
```

**Explanation:**

* `-d` → run detached
* `-t` → allocate terminal
* `-p 80:80` → map container port 80 to host port 80
* `--name mynginx` → gives a readable name

---

### 🧩 Step 2: Enter the Container

```bash
[omkar67@omkar ~]$ docker container exec -it mynginx bash
```

You’re now inside the **Ubuntu container**.

---

### 🧩 Step 3: Install Nginx

```bash
root@container# apt-get update
root@container# apt-get install -y nginx
```

> 🟢 You’ll see:
> *nginx is already the newest version (1.24.0...)*

---

### 🧩 Step 4: Check if Nginx is Running

```bash
root@container# /etc/init.d/nginx status
```

Output:

```
* nginx is not running
```

---

### 🧩 Step 5: Check Default Nginx Directory

```bash
root@container# cd /var/www/html/
root@container# ls
```

Output:

```
index.nginx-debian.html
```

---

### 🧩 Step 6: View the Default HTML

```bash
root@container# cat index.nginx-debian.html
```

You’ll see the **default “Welcome to nginx!”** HTML file.

---

### 🧩 Step 7: Add Your Custom Message

```bash
root@container# echo "Hello Omkar Docker is working!" > index.nginx-debian.html
root@container# cat index.nginx-debian.html
```

✅ Output:

```
Hello Omkar Docker is working!
```

---

### 🧩 Step 8: Start Nginx

```bash
root@container# nginx -g 'daemon off;'
```

This keeps Nginx **running in the foreground**.

💡 Press `Ctrl + Z` and `Ctrl + D` to exit when you’re done.

---

### ✅ Verification

Now, from your RHEL host, open a browser or run:

```bash
[omkar67@omkar ~]$ curl localhost:80
```

Output:

```
Hello Omkar Docker is working!
```

🎉 You’ve successfully configured Nginx **manually** inside a container.

---

## 🧱 **Section 2: Automate Using Dockerfile**

Now that you’ve done everything manually, let’s **automate** those same steps using a **Dockerfile**.

---

### 📂 **Folder Setup**

```bash
[omkar67@omkar ~]$ cd /root
[omkar67@omkar ~]$ mkdir demo
[omkar67@omkar ~]$ cd demo/
```

Create two files:

1. `Dockerfile`
2. `index.nginx-debian.html`

---

### 📜 **index.nginx-debian.html**

```html
Hello Omkar Docker is working!
```

---

### 📜 **Dockerfile**

```Dockerfile
# Step 1: Base image
FROM ubuntu:22.04

# Step 2: Update and install nginx
RUN apt-get update && apt-get install -y nginx

# Step 3: Copy website file
COPY index.nginx-debian.html /var/www/html/

# Step 4: Expose port 80
EXPOSE 80

# Step 5: Keep nginx running
CMD ["nginx", "-g", "daemon off;"]
```

---

### 🧠 **Explanation of Key Instructions**

| Instruction | Purpose          | Example                                       | Notes                           |
| ----------- | ---------------- | --------------------------------------------- | ------------------------------- |
| `FROM`      | Base OS image    | `FROM ubuntu:22.04`                           | Starts from Ubuntu              |
| `RUN`       | Execute commands | `RUN apt-get update`                          | Installs Nginx                  |
| `COPY`      | Copy files       | `COPY index.nginx-debian.html /var/www/html/` | Puts your file inside the image |
| `EXPOSE`    | Open port        | `EXPOSE 80`                                   | Makes port 80 available         |
| `CMD`       | Default command  | `CMD ["nginx", "-g", "daemon off;"]`          | Keeps Nginx running             |

💡 **COPY Command:**
This instruction copies your **local file** (`index.nginx-debian.html`) from the **build context (demo folder)** into the container path `/var/www/html/`.

---

### 🏗️ **Build the Docker Image**

```bash
[omkar67@omkar ~]$ docker build -t mycustomnginx .
```

Output (summary):

```
[+] Building 29.4s (8/8)
 => [2/3] RUN apt-get update && apt-get install -y nginx
 => [3/3] COPY index.nginx-debian.html /var/www/html/
 => exporting to image
 => writing image sha256:420e09fd6e2c...
```

✅ Successfully built the image!

---

### 📦 **Check Image**

```bash
[omkar67@omkar ~]$ docker images
```

Output:

```
REPOSITORY      TAG       IMAGE ID       CREATED          SIZE
mycustomnginx   latest    420e09fd6e2c   45 seconds ago   203MB
```

---

### 🚀 **Run Container from Custom Image**

```bash
[omkar67@omkar ~]$ docker container run -dt -p 8001:80 --name mycustomngin mycustomnginx
```

Check running containers:

```bash
[omkar67@omkar ~]$ docker ps
```

Output:

```
CONTAINER ID   IMAGE          COMMAND                  STATUS          PORTS
bec29734f923   mycustomnginx  "nginx -g 'daemon of…"   Up 1 min        0.0.0.0:8001->80/tcp
```

---

### 🌐 **Test Your Custom Web Page**

Use `curl` with your host IP and port:

```bash
[omkar67@omkar ~]$ curl 10.160.15.214:8001
```

✅ Output:

```
Hello Omkar Docker is working!
```

🎉 Congratulations — you’ve just built your **own Docker image** and automated the entire process!

---

## 🧾 **Summary Table**

| Step | Action               | Command                     | Purpose        |
| :--: | :------------------- | :-------------------------- | :------------- |
|  1️⃣ | Run Ubuntu container | `docker run -dt ubuntu`     | Start base     |
|  2️⃣ | Install Nginx        | `apt-get install nginx`     | Web server     |
|  3️⃣ | Edit HTML            | `echo "Hello Omkar..."`     | Custom message |
|  4️⃣ | Build Dockerfile     | `docker build -t mynginx .` | Create image   |
|  5️⃣ | Run Image            | `docker run -p 8001:80 ...` | Access site    |
|  6️⃣ | Verify               | `curl <IP>:8001`            | Check output   |

---

## 💬 **Key Takeaways**

* You first created and tested everything **manually** 🧑‍💻
* Then automated the same setup using a **Dockerfile** ⚙️
* Learned how **COPY** puts your custom files inside the image 📁
* Understood how **CMD** keeps your process running 🌀
* Saw **EXPOSE** to open container ports 🌐

---

## 💡 **Next Steps (For Your Repo Series)**

Continue your Docker journey by:

1. Adding a `HEALTHCHECK` instruction 🩺
2. Creating a custom tag →

   ```bash
   [omkar67@omkar ~]$ docker tag mycustomnginx omkar/nginx:v1
   ```
3. Pushing to Docker Hub →

   ```bash
   [omkar67@omkar ~]$ docker push omkar/nginx:v1
   ```
