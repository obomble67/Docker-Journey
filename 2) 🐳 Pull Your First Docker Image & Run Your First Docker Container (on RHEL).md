# 🐳 Pull Your First Docker Image & Run Your First Docker Container (on RHEL)

---

## 🔹 1. Pull Your First Docker Image 🐳🔽

The first thing you need to do is pull an image from Docker Hub.
Here, we’ll use the official **Nginx** image.

```bash
[omkar67@omkar ~]$ docker pull nginx
```

📝 This command downloads the **Nginx image** to your local RHEL machine.

---

## 🔹 2. Verify the Image is Downloaded 📸

Once the image is pulled, verify it with:

```bash
[omkar67@omkar ~]$ docker images
```

Example output:

```
REPOSITORY          TAG       IMAGE ID       CREATED         SIZE
nginx               latest    abc12345abc9   2 weeks ago     133MB
```

---

## 🔹 3. Run Your First Docker Container 🚀

Now, run your first container using the Nginx image:

```bash
[omkar67@omkar ~]$ docker run -dt -p 8080:80 --name my_nginx_container nginx
```

**Explanation:**

* `-dt` → Run container in detached mode (background).
* `-p 8080:80` → Map port **8080** on your host (RHEL) to port **80** inside the container.
* `--name my_nginx_container` → Give your container a custom name.

🎉 After running this, your **Nginx container** will be up and running!

---

## 🔹 4. Check Running Containers 📊

To verify that your container is running:

```bash
[omkar67@omkar ~]$ docker container ps
```

Example output:

```
CONTAINER ID   IMAGE     COMMAND                  CREATED          STATUS          PORTS                  NAMES
abc12345abcd   nginx     "/docker-entrypoint.…"   1 minute ago     Up 1 minute     0.0.0.0:8080->80/tcp   my_nginx_container
```

✅ The `PORTS` column (`0.0.0.0:8080->80/tcp`) means you can access Nginx at:

```
http://localhost:8080
```

---

## 🔹 5. Verify the Nginx Server is Running 🌍

Open a browser (or use `curl`) and visit:

```bash
[omkar67@omkar ~]$ curl http://localhost:8080
```

If it’s working, you’ll see the **Nginx Welcome Page HTML** output.
You can also open it from a browser on your RHEL machine:

```
http://localhost:8080
```

🎉 Congratulations — your first Docker container is running successfully!

---

## 🔹 6. Check the Container’s Network Interface 🌐

You can check Docker network interfaces like `docker0` using:

```bash
[omkar67@omkar ~]$ ifconfig
```

*(If `ifconfig` isn’t found, install it using `sudo dnf install net-tools`)*

Look for the **docker0** interface — it shows the internal bridge network for your containers.

---

## 🔹 7. Stop the Container 🛑

To stop your running container gracefully:

```bash
[omkar67@omkar ~]$ docker container stop my_nginx_container
```

---

## 🔹 8. Verify the Stopped Container 🧐

To see all containers (running and stopped):

```bash
[omkar67@omkar ~]$ docker container ps -a
```

Example output:

```
CONTAINER ID   IMAGE     COMMAND                  CREATED          STATUS                     PORTS                  NAMES
abc12345abcd   nginx     "/docker-entrypoint.…"   1 minute ago     Exited (0) 2 minutes ago    0.0.0.0:8080->80/tcp   my_nginx_container
```

✅ **Exited (0)** → means the container stopped normally without any errors.

---

## 🔹 9. Clean Up the Container (Optional) 🧹

Remove the stopped container:

```bash
[omkar67@omkar ~]$ docker container rm my_nginx_container
```

Remove the Nginx image if you wish:

```bash
[omkar67@omkar ~]$ docker rmi nginx
```

---

## 🧾 Basic Docker Commands Summary (RHEL Edition)

| Command                             | Description                                               |
| ----------------------------------- | --------------------------------------------------------- |
| `docker pull <image>`               | Pull an image from Docker Hub (e.g., `docker pull nginx`) |
| `docker images`                     | View all Docker images on your system                     |
| `docker run <options> <image>`      | Run a container from an image                             |
| `docker container ps`               | List running containers                                   |
| `docker container ps -a`            | List all containers (running & stopped)                   |
| `docker container stop <container>` | Stop a running container                                  |
| `docker container rm <container>`   | Remove a stopped container                                |
| `docker rmi <image>`                | Remove a Docker image                                     |

---

## 🔚 Wrap Up

🎉 You’ve successfully:

* Pulled your first Docker image (**Nginx**)
* Created and managed your first Docker container
* Verified network, ports, and service
* Learned how to stop, remove, and clean up containers

