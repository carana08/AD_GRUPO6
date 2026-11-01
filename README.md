# AD_GRUPO6

## Guía de configuración del entorno

Esta guía describe cómo preparar un entorno Python con [uv](https://docs.astral.sh/uv/), fijar Python 3.12.15 y configurar Jupyter para trabajar con notebooks. Los comandos de proyecto se ejecutan desde la carpeta raíz, donde se encuentran `pyproject.toml` y `uv.lock`.

### 1. Instalar uv

Instala `uv` con el instalador oficial correspondiente al sistema operativo.

**Windows (PowerShell):**

```powershell
powershell -ExecutionPolicy ByPass -c "irm https://astral.sh/uv/install.ps1 | iex"
```

**Linux y macOS (terminal):**

```bash
curl -LsSf https://astral.sh/uv/install.sh | sh
```

Abre una terminal nueva y comprueba que la instalación esté disponible:

```bash
uv --version
```

### 2. Instalar y fijar Python 3.12.15

```bash
uv python install 3.12.15
uv python pin 3.12.15
```

El segundo comando crea o actualiza `.python-version` en la raíz del proyecto. Mantén ese archivo en el repositorio para compartir la versión de Python fijada.

### 3. Crear y sincronizar el entorno

Con `uv.lock` disponible, crea el entorno virtual e instala las versiones registradas:

```bash
uv sync --locked
```

`uv` crea `.venv` si todavía no existe y lo sincroniza con `pyproject.toml` y `uv.lock`. No es necesario activar el entorno para ejecutar comandos mediante `uv`; por ejemplo:

```bash
uv run python --version
```

También se puede activar el entorno manualmente:

```bash
# Linux y macOS (bash/zsh)
source .venv/bin/activate
```

```powershell
# Windows (PowerShell)
.venv\Scripts\Activate.ps1
```

### 4. Añadir dependencias al proyecto

Las dependencias directas del proyecto se declaran en `pyproject.toml`.

Para añadir una dependencia, desde la raíz del proyecto ejecuta:

```bash
uv add nombre-paquete
```

`uv add` registra la dependencia en `pyproject.toml` y actualiza `uv.lock`. Comparte ambos archivos en el repositorio. Después de obtener los cambios, sincroniza el entorno local con:

```bash
uv sync --locked
```

### 5. Configurar Jupyter y VS Code

`ipykernel` debe estar declarado como dependencia del proyecto. Después de ejecutar `uv sync --locked`, registra el entorno como kernel de Jupyter:

```bash
uv run python -m ipykernel install --user --name ad-proyecto --display-name "Python (AD Proyecto)"
```

En Windows, Linux o macOS, el comando de registro es el mismo. En VS Code, instala o habilita las extensiones **Python** y **Jupyter**, abre el notebook y selecciona **Select Kernel** → **Python (AD Proyecto)**. Si el kernel no aparece, selecciona manualmente el intérprete del entorno:

| Sistema operativo | Intérprete |
| --- | --- |
| Windows | `.venv\\Scripts\\python.exe` |
| Linux y macOS | `.venv/bin/python` |

Para retirar el kernel registrado, ejecuta:

```bash
uv run jupyter kernelspec uninstall ad-proyecto
```

Después de clonar el proyecto o actualizar sus dependencias, sincroniza el entorno y registra el kernel con:

```bash
uv sync --locked
uv run python -m ipykernel install --user --name ad-proyecto --display-name "Python (AD Proyecto)"
```
