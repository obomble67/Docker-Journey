

# 🐳 Docker Installation on RHEL (Red Hat Enterprise Linux)


## 🔧 Step 1: Uninstall Old Versions (if any)

[omkar67@omkar ~]$ sudo dnf remove docker \
docker-client \
docker-client-latest \
docker-common \
docker-latest \
docker-latest-logrotate \
docker-logrotate \
docker-engine
```

---

## 📦 Step 2: Set Up Docker Repository

### 1️⃣ Install required packages:

[omkar67@omkar ~]$ sudo dnf install -y dnf-plugins-core
```

### 2️⃣ Add the official Docker CE repository:

```bash
[omkar67@omkar ~]$ sudo dnf config-manager \
--add-repo \
https://download.docker.com/linux/rhel/docker-ce.repo
```

*(Same repo works for RHEL 8 & 9)*

---

## 🧰 Step 3: Install Docker Engine

```bash
[omkar67@omkar ~]$ sudo dnf install -y docker-ce docker-ce-cli containerd.io docker-buildx-plugin docker-compose-plugin
```

---

## 🚀 Step 4: Start and Enable Docker

```bash
[omkar67@omkar ~]$ sudo systemctl start docker
[omkar67@omkar ~]$ sudo systemctl enable docker
```

Check Docker service status:

```bash
[omkar67@omkar ~]$ sudo systemctl status docker
```

---

## 🧾 Step 5: Verify Docker Installation

Run the “Hello World” test container:

```bash
[omkar67@omkar ~]$ sudo docker run hello-world
```

✅ If you see:

```
Hello from Docker!
This message shows that your installation appears to be working correctly.
```

Then Docker is successfully installed 🎉

---

## 👤 Step 6: Manage Docker as a Non-Root User (Optional)

1️⃣ Add your user to the `docker` group:

```bash
[omkar67@omkar ~]$ sudo usermod -aG docker $USER
```

2️⃣ Log out and back in, or activate group immediately:

```bash
[omkar67@omkar ~]$ newgrp docker
```

3️⃣ Test without sudo:

```bash
[omkar67@omkar ~]$ docker run hello-world
```

---

## 🔄 Step 7: Enable Docker at Boot

```bash
[omkar67@omkar ~]$ sudo systemctl enable docker.service
[omkar67@omkar ~]$ sudo systemctl enable containerd.service
```

---

## 🧹 Step 8: Uninstall Docker (if needed)

```bash
[omkar67@omkar ~]$ sudo dnf remove docker-ce docker-ce-cli containerd.io docker-buildx-plugin docker-compose-plugin
[omkar67@omkar ~]$ sudo rm -rf /var/lib/docker
[omkar67@omkar ~]$ sudo rm -rf /var/lib/containerd
```

---

## 📘 Reference Notes

* **Docker config file:** `/etc/docker/daemon.json`
* **Docker socket file:** `/var/run/docker.sock`
* **Docker images location:** `/var/lib/docker/`

---


