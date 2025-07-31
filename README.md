# NetBox_Docker
Instalación de Netbox, en Docker

Paso 1: Instalar docker
url: https://docs.docker.com/engine/install/ubuntu/

```bash
# Add Docker's official GPG key:
sudo apt-get update
sudo apt-get install ca-certificates curl
sudo install -m 0755 -d /etc/apt/keyrings
sudo curl -fsSL https://download.docker.com/linux/ubuntu/gpg -o /etc/apt/keyrings/docker.asc
sudo chmod a+r /etc/apt/keyrings/docker.asc

# Add the repository to Apt sources:
echo \
  "deb [arch=$(dpkg --print-architecture) signed-by=/etc/apt/keyrings/docker.asc] https://download.docker.com/linux/ubuntu \
  $(. /etc/os-release && echo "${UBUNTU_CODENAME:-$VERSION_CODENAME}") stable" | \
  sudo tee /etc/apt/sources.list.d/docker.list > /dev/null
sudo apt-get update
```

```bash

sudo apt-get install docker-ce docker-ce-cli containerd.io docker-buildx-plugin docker-compose-plugin
```

Test:

```bash
sudo docker run hello-world
```
Paso 2: (Opcional) Dar permisos al usuario para poder ejecutar comandos docker

```bash
sudo usermod -aG docker $USER
newgrp docker
```

Paso 3: Descargar NetBox Docker del repositorio

```bash
git clone -b release https://github.com/netbox-community/netbox-docker.git
cd netbox-docker
```

Paso 4: Crear un fichero docker-compose.override.yml

```bash
tee docker-compose.override.yml <<EOF
services:
  netbox:
    ports:
      - 8000:8080
EOF
```
Paso 5: Arrancar los Containers

```bash
docker compose pull
docker compose up -d
```
Opservaciones: puede fallar, se esperar que los contenedores se generen bien y volver a ejecutar el docker compose up -d

```text
✔ Container netbox-docker-redis-cache-1  Started                                                                  0.3s
✔ Container netbox-docker-postgres-1     Started                                                                  0.3s
✔ Container netbox-docker-redis-1        Started                                                                  0.2s
✘ Container netbox-docker-netbox-1       Error                                                                  125.2s
```
Una vez levantados todos los containers se puede visitar Netbox en la web, [con la ip de tu servidor] http://your-server-ip:8000:

Paso 6: Crear usuario superadministrador:

```bash
docker compose exec netbox /opt/netbox/netbox/manage.py createsuperuser
```
