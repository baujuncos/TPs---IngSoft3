# ProyectoArquiSoft1 (utilzado para el TP) – CI con Azure DevOps

Este repositorio contiene un **monorepo** con:
- **Frontend**: aplicación React (carpeta `/frontend`)
- **Backend**: API en Go (carpeta `/backend`)

El proyecto implementa **Integración Continua (CI)** mediante **Azure DevOps Pipelines** ejecutándose en un **agente Self-Hosted** instalado en la máquina de desarrollo.

---

## Ejecución local

### 1. Clonar el repositorio

>bash
> git clone https://baujuncos@dev.azure.com/baujuncos/TP4_IS3/_git/TP4_IS3
> cd TP4_IS3

### 2. Ejecutar Frontend

>cd frontend
> 
> npm install    ------------   instalar dependencias
> 
> npm start    ----------- levantar en modo desarrollo

### 3. Ejecutar Backend

> cd backend
> 
> go mod download -------------- descargar dependencias
>
> go run main.go  ------------ iniciar API


### Pipeline de CI

El archivo azure-pipelines.yml define un pipeline multi-stage que:

1. Instala Node 18.x y compila el frontend (npm ci && npm run build)
2. Publica el artefacto front_drop con el contenido de /frontend/build 
3. Instala Go 1.21, descarga dependencias y compila el backend (go build)
4. Ejecuta tests del backend (go test ./... -v)
5. Publica el artefacto back_drop con el binario generado

**Trigger**: El pipeline se dispara automáticamente en cada push a la rama main.

**Artefactos generados**:
* front_drop → Carpeta estática lista para deploy en cualquier servidor web. 
* back_drop → Binario compilado del backend (backend-api.exe).

### Prerequisitos del Agente Self-Hosted

Para correr el pipeline en tu máquina local:

1. Tener Windows 10/11 o Linux/MacOS con conectividad a Azure DevOps.
2. Instalar el Agente Self-Hosted:
   *  Organization Settings → Agent Pools → New Agent
   * Descargar el paquete, descomprimir y ejecutar config.cmd
   * Instalar como servicio (svc install, svc start)
3. Asegurar que la máquina tenga:
   * Node 18.x (o el que define el pipeline)
   * Go 1.21.x
   * Acceso a Internet para descargar dependencias npm/go
