# Instalación y configuración de `uv` y Git

## 1. Instalación de `uv`

### Con PowerShell

```powershell
winget install --id=astral-sh.uv -e
```

Verificar la instalación:

```powershell
uv --version
```

### Otra opción

```powershell
powershell -ExecutionPolicy ByPass -c "irm https://astral.sh/uv/install.ps1 | iex"
```

## 2. Instalación de Git

Para instalar Git en Windows:

```powershell
winget install --id Git.Git -e
```

## 3. Crear y clonar un proyecto de GitHub

1. Crear un repositorio vacío en GitHub llamado `demo-uv`.
2. Clonar el repositorio:

```powershell
git clone https://github.com/usuario/demo-uv.git
cd demo-uv
```

3. Inicializar el proyecto con `uv`:

```powershell
uv init
```

## 4. Configurar el nombre y el correo de Git

Para identificar al autor de los commits:

```powershell
git config --global user.name "Tu Nombre"
git config --global user.email "tu@email.com"
```

Para consultar el estado del repositorio:

```powershell
git status
```

## 5. Crear el entorno virtual

Crear el entorno virtual del proyecto:

```powershell
uv venv
```

El entorno virtual se almacena normalmente en `.venv/` y **no debe subirse a Git**.

### Crear el archivo `.gitignore`

Ejecutar en PowerShell:

```powershell
@"
.venv/
__pycache__/
*.pyc
"@ | Set-Content .gitignore
```

Este archivo indica a Git qué archivos y directorios debe ignorar.

## 6. Ejecutar el proyecto

Para ejecutar el archivo `main.py`:

```powershell
uv run python main.py
```

`uv run` ejecuta el programa dentro del entorno del proyecto y gestiona las dependencias declaradas.

## 7. Instalar paquetes

Para instalar la biblioteca `requests`:

```powershell
uv add requests
```

Este comando actualiza los archivos de configuración y bloqueo de dependencias del proyecto.

### `pyproject.toml`

Define información del proyecto, como:

* El nombre y los metadatos del proyecto.
* Las dependencias que necesita.
* La configuración de herramientas de Python.

### `uv.lock`

Registra la resolución de dependencias y las versiones exactas para facilitar la reproducibilidad del entorno.

Esto permite que diferentes instalaciones del proyecto utilicen las mismas versiones resueltas de los paquetes.

## 8. Crear el archivo `main.py`

Ejecutar en PowerShell:

```powershell
@"
import requests

response = requests.get("https://api.github.com")

print(response.status_code)
"@ | Set-Content main.py
```

El programa realiza una petición HTTP a la API de GitHub e imprime el código de estado de la respuesta.

### Consultar la respuesta

El contenido de la respuesta se obtiene con:

```python
response.content
```

El código de estado HTTP se obtiene con:

```python
response.status_code
```

Por ejemplo, un código `200` indica que la petición se completó correctamente.

Para ejecutar el programa:

```powershell
uv run python main.py
```

## 9. Guardar el proyecto en Git

Consultar los cambios:

```powershell
git status
```

Agregar los archivos al área de preparación:

```powershell
git add .
```

Crear el primer commit:

```powershell
git commit -m "Initial project with uv"
```

## 10. Clonar el proyecto para comprobar su reproducibilidad

Volver al directorio padre:

```powershell
cd ..
```

Clonar el proyecto localmente:

```powershell
git clone demo-uv demo-uv-copy
cd demo-uv-copy
```

Sincronizar las dependencias declaradas en `pyproject.toml` y bloqueadas en `uv.lock`:

```powershell
uv sync
```

Ejecutar el programa:

```powershell
uv run python main.py
```

Si la configuración es correcta, el proyecto clonado podrá ejecutarse sin necesidad de copiar manualmente el entorno virtual original.

---

## Resumen

* **`uv init`**: inicializa un proyecto Python.
* **`uv venv`**: crea un entorno virtual.
* **`uv add`**: agrega una dependencia al proyecto.
* **`uv sync`**: sincroniza el entorno con las dependencias declaradas.
* **`uv run`**: ejecuta un comando dentro del entorno del proyecto.
* **`pyproject.toml`**: contiene la configuración y las dependencias del proyecto.
* **`uv.lock`**: registra las versiones resueltas de las dependencias.
* **`.gitignore`**: evita que archivos como `.venv/` se incluyan en Git.
* **`git commit`**: registra los cambios en el historial del repositorio.
