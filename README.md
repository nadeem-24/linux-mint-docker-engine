# linux-mint-docker-engine
Documentation to install Linux mint 22.2

🐳 Docker Installation on Linux Mint (Working & Stable Method)

This guide installs Docker on Linux Mint by forcing the Ubuntu Jammy (22.04) Docker repository.
This avoids common errors caused by the Ubuntu Noble (24.04) repo.

🧹 Step 1: Remove existing / broken Docker installation
sudo apt remove docker docker-engine docker.io containerd runc -y
sudo rm -f /etc/apt/sources.list.d/docker*
sudo rm -f /etc/apt/keyrings/docker*

🔍 Why this is needed

Removes any partially installed Docker packages

Deletes conflicting Docker APT sources

Fixes Signed-By conflicts and NO_PUBKEY errors

🔄 Step 2: Update package index & install prerequisites
sudo apt update
sudo apt install ca-certificates curl gnupg -y

🔍 Explanation

ca-certificates → allows secure HTTPS downloads

curl → used to fetch Docker’s GPG key

gnupg → verifies package authenticity

🔐 Step 3: Add Docker’s official GPG key
sudo install -m 0755 -d /etc/apt/keyrings
curl -fsSL https://download.docker.com/linux/ubuntu/gpg | \
sudo gpg --dearmor -o /etc/apt/keyrings/docker.gpg
sudo chmod a+r /etc/apt/keyrings/docker.gpg

🔍 Explanation

Creates a secure keyring directory

Downloads Docker’s signing key

Converts it to APT-readable format

Ensures APT can verify Docker packages

📦 Step 4: Add Docker repository (Force Ubuntu Jammy)
echo \
"deb [arch=amd64 signed-by=/etc/apt/keyrings/docker.gpg] \
https://download.docker.com/linux/ubuntu jammy stable" | \
sudo tee /etc/apt/sources.list.d/docker.list > /dev/null

🔍 Explanation

Forces Jammy (22.04) instead of Noble (24.04)

Jammy Docker packages are stable and complete

Prevents 404 Not Found and containerd.io issues

⚠️ Important:
Even on Linux Mint 21/22 → always use jammy

⬇️ Step 5: Install Docker Engine & tools
sudo apt update
sudo apt install docker-ce docker-ce-cli containerd.io \
docker-buildx-plugin docker-compose-plugin -y

🔍 Explanation

docker-ce → Docker Engine

docker-ce-cli → Docker CLI

containerd.io → Container runtime

docker-buildx-plugin → Advanced image builds

docker-compose-plugin → docker compose command

▶️ Step 6: Start Docker and enable auto-start
sudo systemctl enable docker
sudo systemctl start docker

🔍 Explanation

Starts Docker immediately

Ensures Docker starts automatically on boot

👤 Step 7: Run Docker without sudo (Recommended)
sudo usermod -aG docker $USER
newgrp docker

🔍 Explanation

Adds current user to docker group

Allows running docker commands without sudo

newgrp applies the change instantly (no logout needed)

✅ Step 8: Verify Docker installation
docker --version
docker compose version
docker run hello-world

🔍 Expected result

Docker version is displayed

Docker Compose version is displayed

hello-world container runs successfully

🧠 Optional: Useful Docker maintenance commands
Check disk usage
docker system df

Clean unused Docker data
docker system prune


⚠️ Warning: Removes unused containers, images, and networks.

🏁 Summary


Linux Mint + Docker official instructions may fail

Forcing Ubuntu Jammy repo is the most reliable method

This setup is stable for development, Docker Compose, and Kubernetes

-----------------------------------------Fedora KDE 43 GPU commands---------------------------
Nice! 👍
Here’s a **clean copy-paste summary of all the important commands**, in order.

---

## 1️⃣ Check NVIDIA driver & GPU

```bash
nvidia-smi
```

---

## 2️⃣ Check which GPU is currently used (default)

```bash
glxinfo | grep "OpenGL renderer"
```

---

## 3️⃣ Test NVIDIA PRIME offloading (should show GTX 1650)

```bash
__NV_PRIME_RENDER_OFFLOAD=1 __GLX_VENDOR_LIBRARY_NAME=nvidia glxinfo | grep "OpenGL renderer"
```

---

## 4️⃣ Run Steam using NVIDIA GPU

```bash
__NV_PRIME_RENDER_OFFLOAD=1 __GLX_VENDOR_LIBRARY_NAME=nvidia steam
```

---

## 5️⃣ Steam per-game launch option (BEST METHOD)

Paste this into **Steam → Properties → Launch Options**:

```bash
__NV_PRIME_RENDER_OFFLOAD=1 __GLX_VENDOR_LIBRARY_NAME=nvidia %command%
```

---

## 6️⃣ Verify game is using NVIDIA while running

```bash
nvidia-smi
```

---

## 7️⃣ Make sure nouveau is NOT loaded

```bash
lsmod | grep nouveau
```

(no output = good)

---

## Optional (X11 session check)

```bash
echo $XDG_SESSION_TYPE
```

---

### ✅ Expected result

* `glxinfo` default → AMD Radeon
* Offload command → **NVIDIA GTX 1650**
* `nvidia-smi` → game listed under Processes
* FPS improves 🚀

If you want, I can also show how to:

* Make NVIDIA default for **all games**
* Create a desktop shortcut
* Optimize Steam + Proton for Fedora KDE
