# 🐳 Instalación de Docker en Ubuntu

Este documento describe los pasos para instalar Docker correctamente en Ubuntu desde el repositorio oficial.

---

## 📋 Prerrequisitos

Antes de comenzar, asegúrate de cumplir con los siguientes requisitos:

### 🖥️ Sistema
| Componente | Requerido | Tu sistema |
|---|---|---|
| **OS** | Ubuntu 20.04 / 22.04 / 24.04 LTS (64-bit) | Linux Mint 22.1 (base Ubuntu 24.04 Noble) |
| **Arquitectura** | x86_64 / amd64 | x86_64 ✅ |
| **RAM** | Mínimo 2 GiB | 16 GiB ✅ |
| **Almacenamiento libre** | Mínimo 10 GiB | ~362 GiB libres ✅ |
| **Kernel** | 3.10 o superior | 6.8.0-107-generic ✅ |

### ⚙️ Requisitos de software
- Acceso a terminal con privilegios `sudo`
- Conexión a internet activa
- `curl` y `ca-certificates` disponibles (se instalan en el paso 2)

### ⚠️ Consideraciones previas
- Este sistema usa **Linux Mint**, que es compatible con los repositorios de Ubuntu.  
  Asegúrate de que `VERSION_CODENAME` en `/etc/os-release` retorne `noble` y no `xia`.  
  Puedes verificarlo con:
```bash
  . /etc/os-release && echo "$VERSION_CODENAME"
```
  Si retorna `xia` (el codename de Mint), deberás reemplazarlo manualmente por `noble` en el paso 4.

- El sistema **no tiene swap configurado**. Docker puede funcionar sin swap, pero se recomienda
  para entornos con múltiples contenedores. Puedes omitirlo para uso básico.

- Se detectaron **dos GPUs** (Intel HD 4000 + NVIDIA NVS 5200M). Docker no requiere configuración
  adicional de GPU para uso general.

---

## 🔧 1. Eliminar versiones antiguas (opcional pero recomendado)

```bash
sudo apt purge -y docker-ce docker-ce-cli containerd.io docker-buildx-plugin docker-compose-plugin docker-ce-rootless-extras 
```

![Eliminar versiones antiguas](./screenshots/Eliminar.png)

---

## 🔄 2. Actualizar paquetes e instalar dependencias

```bash
sudo apt update
sudo apt install -y ca-certificates curl
```

![Instalar certificados](./screenshots/Certificado.png)

---

## 🔐 3. Agregar la clave GPG oficial de Docker

```bash
sudo install -m 0755 -d /etc/apt/keyrings
sudo curl -fsSL https://download.docker.com/linux/ubuntu/gpg -o /etc/apt/keyrings/docker.asc
sudo chmod a+r /etc/apt/keyrings/docker.asc
```

![Agregar clave GPG](./screenshots/Clave.png)

---

## 📦 4. Agregar el repositorio oficial de Docker

```bash
echo \
  "deb [arch=$(dpkg --print-architecture) signed-by=/etc/apt/keyrings/docker.asc] https://download.docker.com/linux/ubuntu \
  $(. /etc/os-release && echo "$VERSION_CODENAME") stable" | \
  sudo tee /etc/apt/sources.list.d/docker.list > /dev/null
```

> ⚠️ **Nota para Linux Mint:** Si el comando anterior no funciona, reemplaza
> `$(. /etc/os-release && echo "$VERSION_CODENAME")` por `noble` directamente:
> ```bash
> echo \
>   "deb [arch=$(dpkg --print-architecture) signed-by=/etc/apt/keyrings/docker.asc] https://download.docker.com/linux/ubuntu noble stable" | \
>   sudo tee /etc/apt/sources.list.d/docker.list > /dev/null
> ```

![Agregar repositorio Docker](./screenshots/Repositorio.png)

---

## 📥 5. Instalar Docker

```bash
sudo apt update
sudo apt install -y docker-ce docker-ce-cli containerd.io docker-buildx-plugin docker-compose-plugin
```

![Instalación de Docker](./screenshots/Instalacion.png)

---

## ✅ 6. Verificar instalación

```bash
sudo docker run hello-world
```

![Primera verificación con sudo](./screenshots/Primera.png)

---

## 👤 7. Ejecutar Docker sin sudo (opcional)

```bash
sudo groupadd docker
sudo usermod -aG docker $USER
newgrp docker
```

![Configurar grupo Docker](./screenshots/Grupo.png)

Luego verifica nuevamente:

```bash
docker run hello-world
```

![Segunda verificación sin sudo](./screenshots/Segunda.png)

---

## 🧠 Notas importantes

- Si `groupadd docker` falla, es porque el grupo ya existe (es normal).
- Después de agregar el usuario al grupo, también puedes cerrar sesión y volver a entrar.
- `docker-compose` ahora se usa como plugin (`docker compose` en lugar de `docker-compose`).

---

