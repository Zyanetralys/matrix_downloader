# MATRIX DOWNLOADER // 動画抽出システム

Aplicación de escritorio 100% local (Python + Tkinter) para descargar vídeos o
audio de YouTube, tanto de URLs sueltas como de playlists completas con
selección individual de elementos.

## ⚠️ Nota legal (importante)

Esta herramienta **no aloja ni distribuye contenido**: simplemente automatiza
`yt-dlp` en tu propio equipo. Úsala solo para descargar vídeos de los que
tengas los derechos, el uso para saltarse el copyright de
contenido protegido queda fuera del propósito de este proyecto.

## Qué necesitas antes de empezar

Para que la app funcione hacen falta **tres cosas** en tu ordenador:

1. **Python 3.9 o superior** (el lenguaje en el que está escrito el programa).
2. **ffmpeg** (un programa externo que une el vídeo y el audio, y convierte
   a MP3/WAV/FLAC/M4A). Sin esto la app abre igual, pero las descargas
   fallarán o saldrán incompletas.
3. **pip** (el gestor de paquetes de Python, viene incluido con Python).

**Tkinter** (la librería de la interfaz gráfica) también es necesaria, pero en
Windows y macOS viene ya incluida con Python. En Linux a veces hay que
instalarla aparte (se indica más abajo).

A continuación tienes la guía completa, separada por sistema operativo.
Sigue solo la sección de tu sistema.

---

## 🪟 WINDOWS

### 1. Instalar Python

1. Ve a https://www.python.org/downloads/
2. Descarga la última versión de Python 3 (botón amarillo grande).
3. Ejecuta el instalador descargado.
4. **MUY IMPORTANTE**: en la primera pantalla del instalador, marca la
   casilla **"Add python.exe to PATH"** (abajo del todo) antes de pulsar
   "Install Now". Si no la marcas, luego Windows no encontrará el comando
   `python`.
5. Termina la instalación y reinicia el símbolo del sistema (CMD/PowerShell)
   si lo tenías abierto.
6. Verifica que se instaló correctamente abriendo **PowerShell** (busca
   "PowerShell" en el menú Inicio) y escribiendo:
   ```powershell
   python --version
   ```
   Debe mostrar algo como `Python 3.12.x`. Si sale error, reinicia el PC y
   vuelve a probar.

### 2. Instalar ffmpeg

**Opción A (recomendada, usando winget, ya viene en Windows 10/11):**
```powershell
winget install ffmpeg
```
Cierra y vuelve a abrir PowerShell después de instalarlo.

**Opción B (manual, si winget no funciona):**
1. Ve a https://www.gyan.dev/ffmpeg/builds/ y descarga el archivo
   **"ffmpeg-release-essentials.zip"**.
2. Descomprime el ZIP, por ejemplo en `C:\ffmpeg`.
3. Dentro encontrarás una carpeta `bin` (ej. `C:\ffmpeg\bin`) que contiene
   `ffmpeg.exe`. Copia esa ruta.
4. Añádela al PATH del sistema:
   - Busca en el menú Inicio "Variables de entorno" y abre
     "Editar las variables de entorno del sistema".
   - Pulsa "Variables de entorno...".
   - En la lista inferior ("Variables del sistema"), selecciona `Path` y
     pulsa "Editar".
   - Pulsa "Nuevo" y pega la ruta a la carpeta `bin` (ej. `C:\ffmpeg\bin`).
   - Acepta todas las ventanas.
5. Abre una **nueva** ventana de PowerShell y comprueba:
   ```powershell
   ffmpeg -version
   ```
   Debe mostrar la versión de ffmpeg, no un error.

### 3. Descargar/colocar los archivos del proyecto

Copia la carpeta `matrix_downloader` (con `main.py`, `requirements.txt` y
este `README.md`) a donde quieras, por ejemplo en `C:\Users\TuUsuario\Desktop\matrix_downloader`.

### 4. Crear un entorno virtual e instalar dependencias

Abre PowerShell, navega hasta la carpeta del proyecto y ejecuta:
```powershell
cd C:\Users\TuUsuario\Desktop\matrix_downloader
python -m venv venv
venv\Scripts\activate
pip install -r requirements.txt
```
Si todo va bien, verás `(venv)` al principio de la línea de comandos, y
`pip` habrá descargado `yt-dlp`.

### 5. Ejecutar la aplicación

```powershell
python main.py
```
Debería abrirse la ventana de la app con el tema Matrix.

*(Cada vez que quieras volver a abrir la app en el futuro, solo necesitas
repetir los pasos 4 (`venv\Scripts\activate`) y 5 (`python main.py`),
no hace falta reinstalar nada.)*

---

## 🍎 macOS — Paso a paso desde cero

### 1. Instalar Homebrew (gestor de paquetes, si no lo tienes)

Abre la app **Terminal** y ejecuta:
```bash
/bin/bash -c "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/HEAD/install.sh)"
```
Sigue las instrucciones que aparezcan en pantalla (puede pedirte tu
contraseña de usuario).

### 2. Instalar Python

macOS trae una versión de Python antigua preinstalada; instala una versión
moderna con Homebrew:
```bash
brew install python
```
Verifica:
```bash
python3 --version
```

### 3. Instalar ffmpeg

```bash
brew install ffmpeg
```
Verifica:
```bash
ffmpeg -version
```

### 4. Instalar Tkinter (si diera error al arrancar la app)

Normalmente ya viene incluido, pero si al ejecutar `main.py` ves un error
de `tkinter`, instala:
```bash
brew install python-tk
```

### 5. Colocar los archivos del proyecto

Copia la carpeta `matrix_downloader` a donde quieras, por ejemplo a tu
carpeta de Escritorio.

### 6. Crear entorno virtual e instalar dependencias

```bash
cd ~/Desktop/matrix_downloader
python3 -m venv venv
source venv/bin/activate
pip install -r requirements.txt
```

### 7. Ejecutar

```bash
python3 main.py
```

*(La próxima vez, solo repite `source venv/bin/activate` y `python3 main.py`.)*

---

## 🐧 LINUX (Debian/Ubuntu y derivados) — Paso a paso desde cero

### 1. Actualizar el sistema e instalar Python, pip, Tkinter y ffmpeg

Abre una terminal y ejecuta:
```bash
sudo apt update
sudo apt install python3 python3-pip python3-venv python3-tk ffmpeg -y
```

### 2. Verificar que todo está instalado

```bash
python3 --version
pip3 --version
ffmpeg -version
```
Los tres comandos deben devolver una versión, sin errores.

### 3. Colocar los archivos del proyecto

Copia la carpeta `matrix_downloader` a tu carpeta personal, por ejemplo
`~/matrix_downloader`.

### 4. Crear entorno virtual e instalar dependencias

```bash
cd ~/matrix_downloader
python3 -m venv venv
source venv/bin/activate
pip install -r requirements.txt
```

### 5. Ejecutar

```bash
python3 main.py
```

*(La próxima vez, solo repite `source venv/bin/activate` y `python3 main.py`.)*

## Uso

1. Pega una URL de YouTube (vídeo suelto o playlist) en el campo `URL >`.
2. Pulsa **解析 ANALIZAR**. Si es una playlist, se listarán todos sus vídeos
   con casillas (☑/☐) marcadas por defecto.
3. Haz clic sobre la columna `✓` de cada fila para marcar/desmarcar
   elementos individuales, o usa **全選択 / 全解除** (seleccionar todos /
   deseleccionar todos).
4. Elige **形式 Formato** (vídeo MP4/WEBM o audio MP3/M4A/WAV/FLAC) y
   **品質 Calidad** (resolución para vídeo, bitrate para audio).
5. Elige la carpeta de salida (por defecto `./descargas`).
6. Pulsa **▶ DESCARGAR**. El progreso y los logs se muestran en tiempo real;
   puedes cancelar en cualquier momento con **■ CANCELAR** (termina el
   elemento en curso y detiene la cola).

---

## 🛠️ Solución de problemas comunes

**`'python' no se reconoce como un comando...` (Windows)**
No marcaste "Add python.exe to PATH" al instalar. Reinstala Python y marca
esa casilla, o añade manualmente la carpeta de Python al PATH (igual que
se hace con ffmpeg en el paso 2 de Windows).

**`ModuleNotFoundError: No module named 'yt_dlp'`**
No activaste el entorno virtual o no ejecutaste `pip install -r requirements.txt`
dentro de él. Asegúrate de ver `(venv)` al principio de la terminal antes
de ejecutar `python main.py`.

**`ModuleNotFoundError: No module named 'tkinter'`**
En Linux, instala `python3-tk` (paso 1 de Linux). En macOS, instala
`python-tk` con Homebrew (paso 4 de macOS). En Windows no debería pasar si
usaste el instalador oficial de python.org.

**"ffmpeg no detectado en el PATH" al abrir la app**
ffmpeg no está instalado o no está en el PATH. Repite el paso de
instalación de ffmpeg de tu sistema operativo y abre una terminal nueva
(los cambios de PATH no se aplican a terminales ya abiertas).

**La descarga falla con errores relacionados con formatos**
Es posible que YouTube haya cambiado algo en su web. Actualiza `yt-dlp` a
la última versión con:
```bash
pip install --upgrade yt-dlp
```

**El vídeo se descarga pero sin audio, o viceversa**
Comprueba que ffmpeg esté correctamente instalado (`ffmpeg -version` debe
funcionar); es el encargado de unir las pistas de vídeo y audio.

---

## Estructura del proyecto

```
matrix_downloader/
├── main.py            # Aplicación completa (GUI + lógica de descarga)
├── requirements.txt    # Dependencias (yt-dlp)
├── README.md
└── descargas/          # Carpeta de salida por defecto (se crea sola)
```

## Notas técnicas

- La extracción de metadatos de playlist usa `extract_flat` para ser rápida
  incluso con listas largas.
- Las descargas corren en un hilo (`threading`) para no bloquear la GUI;
  la comunicación con la interfaz se hace vía `queue.Queue` + `after()`,
  el patrón correcto en Tkinter para actualizar widgets desde otro hilo.
- Para vídeo se descarga `bestvideo+bestaudio` y se remuxa con ffmpeg al
  contenedor elegido (mp4/webm). Para audio se extrae con
  `FFmpegExtractAudio` al códec y bitrate elegidos.
- Si `ffmpeg` no está instalado, la app avisa pero permite continuar
  (algunas conversiones fallarán y se reportará en el log).
