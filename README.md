# 🐳 Instalación de Docker en Ubuntu

Este documento describe los pasos para instalar Docker correctamente en Ubuntu desde el repositorio oficial.

---

## 🔧 1. Eliminar versiones antiguas (opcional pero recomendado)

```bash
sudo apt remove -y docker.io docker-compose docker-compose-v2 docker-doc podman-docker containerd runc
```

---

## 🔄 2. Actualizar paquetes e instalar dependencias

```bash
sudo apt update
sudo apt install -y ca-certificates curl
```

---

## 🔐 3. Agregar la clave GPG oficial de Docker

```bash
sudo install -m 0755 -d /etc/apt/keyrings
sudo curl -fsSL https://download.docker.com/linux/ubuntu/gpg -o /etc/apt/keyrings/docker.asc
sudo chmod a+r /etc/apt/keyrings/docker.asc
```

---

## 📦 4. Agregar el repositorio oficial de Docker

```bash
echo \
  "deb [arch=$(dpkg --print-architecture) signed-by=/etc/apt/keyrings/docker.asc] https://download.docker.com/linux/ubuntu \
  $(. /etc/os-release && echo "$VERSION_CODENAME") stable" | \
  sudo tee /etc/apt/sources.list.d/docker.list > /dev/null
```

---

## 📥 5. Instalar Docker

```bash
sudo apt update
sudo apt install -y docker-ce docker-ce-cli containerd.io docker-buildx-plugin docker-compose-plugin
```

---

## ✅ 6. Verificar instalación

```bash
sudo docker run hello-world
```

---

## 👤 7. Ejecutar Docker sin sudo (opcional)

```bash
sudo groupadd docker
sudo usermod -aG docker $USER
newgrp docker
```

Luego verifica nuevamente:

```bash
docker run hello-world
```

---

## 🧠 Notas importantes

- Si `groupadd docker` falla, es porque el grupo ya existe (es normal).
- Después de agregar el usuario al grupo, también puedes cerrar sesión y volver a entrar.
- `docker-compose` ahora se usa como plugin (`docker compose` en lugar de `docker-compose`).

---

## 🚀 Comando útil

```bash
docker compose version
```

---


