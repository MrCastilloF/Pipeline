# 🚀 Plan DevOps en 1 Día

> **Requisitos previos:** Experiencia en programación + cuenta GitHub

---

## 🌅 Hora 1-2 — Git avanzado + GitHub Actions

Crea un repositorio de práctica y configura tu primer pipeline CI/CD.

Crea el archivo `.github/workflows/mi-primer-pipeline.yml`:

```yaml
name: Mi primer pipeline DevOps

on:
  push:
    branches: [ main ]

jobs:
  build-and-test:
    runs-on: ubuntu-latest
    
    steps:
      - name: Clonar repositorio
        uses: actions/checkout@v3
      
      - name: Configurar Node.js
        uses: actions/setup-node@v3
        with:
          node-version: '18'
      
      - name: Instalar dependencias
        run: npm install
      
      - name: Ejecutar tests
        run: npm test
      
      - name: Mensaje de éxito
        run: echo "✅ Pipeline completado exitosamente"
```

> Cada vez que hagas `push`, GitHub ejecutará esto automáticamente. **Eso ya es CI/CD real.**

---

## 🐳 Hora 3-4 — Docker (contenedores)

Instala Docker Desktop y crea un `Dockerfile` básico:

```dockerfile
FROM node:18-alpine

WORKDIR /app

COPY package*.json ./
RUN npm install

COPY . .

EXPOSE 3000

CMD ["node", "index.js"]
```

### Comandos clave

```bash
# Construir imagen
docker build -t mi-app .

# Correr contenedor
docker run -p 3000:3000 mi-app

# Ver contenedores activos
docker ps

# Ver imágenes descargadas
docker images
```

---

## 🔗 Hora 5-6 — Docker + GitHub Actions juntos

Actualiza tu pipeline para publicar tu imagen en GitHub Container Registry:

```yaml
name: Build y Push Docker

on:
  push:
    branches: [ main ]

jobs:
  docker:
    runs-on: ubuntu-latest
    
    steps:
      - uses: actions/checkout@v3
      
      - name: Login a GitHub Container Registry
        uses: docker/login-action@v2
        with:
          registry: ghcr.io
          username: ${{ github.actor }}
          password: ${{ secrets.GITHUB_TOKEN }}
      
      - name: Build y Push imagen
        uses: docker/build-push-action@v4
        with:
          push: true
          tags: ghcr.io/${{ github.repository }}/mi-app:latest
```

---

## ☁️ Hora 7-8 — Infraestructura como código (IaC) con Terraform

Instala Terraform y crea `main.tf`:

```hcl
terraform {
  required_providers {
    docker = {
      source  = "kreuzwerker/docker"
      version = "~> 3.0"
    }
  }
}

provider "docker" {}

resource "docker_image" "nginx" {
  name = "nginx:latest"
}

resource "docker_container" "servidor" {
  image = docker_image.nginx.image_id
  name  = "mi-servidor"
  
  ports {
    internal = 80
    external = 8080
  }
}
```

### Comandos Terraform

```bash
terraform init    # Inicializar
terraform plan    # Ver qué va a crear
terraform apply   # Crear la infraestructura
terraform destroy # Destruir todo
```

---

## 📊 Hora 9 — Monitoreo básico con Docker Compose

Crea `docker-compose.yml`:

```yaml
version: '3'
services:
  app:
    build: .
    ports:
      - "3000:3000"
  
  prometheus:
    image: prom/prometheus
    ports:
      - "9090:9090"
  
  grafana:
    image: grafana/grafana
    ports:
      - "3001:3001"
```

```bash
docker-compose up -d
```

> Entra a `localhost:3001` para ver **Grafana** corriendo.

---

## 🗺️ Hora 10 — Hoja de ruta siguiente

| Período | Tema |
|---|---|
| Semana 1-2 | Docker + Docker Compose (profundidad) |
| Semana 3-4 | GitHub Actions avanzado |
| Mes 2 | Kubernetes básico (K8s) |
| Mes 3 | AWS/GCP + Terraform |
| Mes 4 | Seguridad DevSecOps |
| Mes 5-6 | Observabilidad (Prometheus + Grafana) |

---

## ✅ Al final del día tendrás

- Un pipeline CI/CD **real funcionando** en GitHub
- Tu primera imagen Docker **publicada**
- Infraestructura definida como código con Terraform
- Monitoreo básico con Grafana

---

## 📚 Recursos recomendados

| Recurso | Para qué |
|---|---|
| [roadmap.sh/devops](https://roadmap.sh/devops) | Ver el mapa completo |
| [Play with Docker](https://labs.play-with-docker.com) | Practicar Docker gratis en el navegador |
| [GitHub Actions docs](https://docs.github.com/actions) | Pipeline real en minutos |
| TechWorld with Nana (YouTube) | Explicaciones visuales y claras |
