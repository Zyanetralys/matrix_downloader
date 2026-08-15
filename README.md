# MATRIX DOWNLOADER

Aplicación de escritorio 100% local (Python + Tkinter) para descargar vídeo
o audio de YouTube, tanto de URLs sueltas como de playlists completas, con
selección individual de elementos.

## Nota legal

Esta herramienta no aloja ni distribuye contenido: automatiza `yt-dlp` en tu
propio equipo. Úsala solo para descargar vídeos de los que tengas los
derechos; el uso para saltarse el copyright de contenido protegido queda
fuera del propósito de este proyecto.

## Dos formas de usar la aplicación

- **Opción A — Ejecutable portable (`.exe`)**: recomendada si solo quieres
  usar la aplicación en Windows sin instalar Python ni ffmpeg por separado.
  Requiere compilarlo una vez con `MatrixDownloader_Build.bat`.
- **Opción B — Ejecutar desde el código fuente**: recomendada si quieres
  modificar el programa, o si usas macOS/Linux. Requiere instalar Python,
  ffmpeg y las dependencias manualmente.

Ambas opciones parten del mismo `main.py`; la diferencia es solo cómo se
ejecuta.

---

## OPCIÓN A — Ejecutable portable para Windows

### Qué es

`MatrixDownloader_Build.bat` es un único archivo que contiene el código
completo de la aplicación embebido en sí mismo. Al ejecutarlo:

1. Extrae `main.py` a partir de su propio contenido.
2. Crea un entorno virtual de Python e instala `yt-dlp` y `PyInstaller`.
3. Descarga una build portable de `ffmpeg`.
4. Compila `MatrixDownloader.exe`, un único ejecutable con Python, `yt-dlp`
   y `ffmpeg` empaquetados dentro.

El resultado es un `.exe` que se puede copiar a cualquier PC con Windows
y ejecutar con doble clic, sin instalar nada más.

### Requisitos (solo para compilar, una vez)

- Windows 10 u 11.
- Python 3.10 o superior instalado, con la casilla **"Add python.exe to
  PATH"** marcada durante la instalación
  (https://www.python.org/downloads/). Verifícalo con `python --version`
  en una terminal.
- Conexión a internet (se usa solo durante la compilación, para descargar
  dependencias y ffmpeg; el `.exe` final no la necesita para funcionar,
  salvo para descargar los vídeos).

### Procedimiento

1. Coloca `MatrixDownloader_Build.bat` en una carpeta cualquiera, por
   ejemplo `C:\MatrixDownloader\`.
2. Haz doble clic sobre el archivo.
3. Sigue las instrucciones en pantalla. El proceso completo tarda entre 1
   y 5 minutos según el equipo.
4. Al finalizar, `MatrixDownloader.exe` aparece en la misma carpeta.

Tras la primera ejecución, la carpeta contendrá también `main.py`,
`venv_build`, `ffmpeg.exe`, `build` y `dist`: son artefactos intermedios
del proceso de compilación. Solo es necesario conservar
`MatrixDownloader.exe`; el resto se puede eliminar, o dejar para que una
recompilación futura sea más rápida (ffmpeg y el entorno virtual no se
vuelven a descargar/crear si ya existen).

### Uso del ejecutable

- Copia únicamente `MatrixDownloader.exe` a donde quieras (no hace falta
  copiar nada más).
- Doble clic para abrirlo.
- Al arrancar, crea automáticamente una carpeta `descargas` junto al
  propio `.exe`, donde se guardan los archivos descargados (salvo que se
  elija otra carpeta desde la interfaz).

### Solución de problemas — compilación

**Windows protegió tu PC / SmartScreen bloquea el `.exe` generado**
El ejecutable no tiene firma digital (firmar código tiene coste y no es
necesario para uso personal). Pulsa "Más información" → "Ejecutar de
todas formas".

**El antivirus marca o elimina `MatrixDownloader.exe`**
Falso positivo habitual en ejecutables `--onefile` de PyInstaller, por
cómo se autoextraen al arrancar. Verifícalo en
https://www.virustotal.com si tienes dudas, o añade una excepción en tu
antivirus para el archivo o la carpeta.

**Control inteligente de aplicaciones (Smart App Control) bloquea el
proceso de compilación**
Es una función de seguridad de Windows 11 que bloquea ejecutables sin
reputación conocida (puede afectar a `python.exe`, `pip.exe` o
`pyinstaller.exe` recién creados en el entorno virtual). Pasos:

1. Abre Seguridad de Windows → Control de aplicaciones y del explorador →
   Historial de protección. Busca la entrada del bloqueo y, si aparece,
   usa "Acciones" → "Permitir en el dispositivo". Es la opción menos
   drástica.
2. Si el bloqueo persiste, en Seguridad de Windows → Control de
   aplicaciones y del explorador → Configuración de Control inteligente
   de aplicaciones, cámbialo a "Desactivado".
3. Importante: una vez desactivado, Windows no permite reactivarlo salvo
   reinstalando el sistema desde cero. Usa esta opción solo si el paso 1
   no funciona.
4. Reinicia el equipo tras cualquiera de los dos cambios y vuelve a
   ejecutar el script.

**`ConnectionResetError` o fallos de conexión al instalar `yt-dlp` /
`pyinstaller`**
Suele deberse a software que intercepta el tráfico HTTPS hacia los
repositorios de paquetes Python (PyPI), no a un problema del script.
Causas más frecuentes, en orden de probabilidad:

1. Antivirus con inspección HTTPS (Avast, AVG, Kaspersky, Bitdefender,
   ESET...): desactiva temporalmente la protección web/HTTPS mientras
   corre el paso de instalación de dependencias.
2. VPN activa: pruébalo con la VPN desconectada.
3. Firewall de red (router, red corporativa): prueba con otra red, por
   ejemplo compartiendo datos móviles desde el teléfono.
4. Corte de red puntual: vuelve a ejecutar el script; no hace falta
   borrar nada si el entorno virtual ya se creó.

**Falla la descarga automática de `ffmpeg`**
El script lo indicará explícitamente. Solución manual:

1. Ve a https://www.gyan.dev/ffmpeg/builds/ y descarga el build
   "essentials" (`ffmpeg-release-essentials.zip`).
2. Extrae el archivo `bin\ffmpeg.exe`.
3. Cópialo a la misma carpeta que `MatrixDownloader_Build.bat`.
4. Vuelve a ejecutar el script; al detectar que `ffmpeg.exe` ya existe,
   no lo descarga de nuevo.

**No se encuentra Python**
Reinstala Python marcando "Add python.exe to PATH", o añade la carpeta de
instalación al PATH manualmente.

---

## OPCIÓN B — Ejecutar desde el código fuente

Requiere tres componentes en el sistema: Python 3.9 o superior, `pip`
(incluido con Python) y `ffmpeg`. `Tkinter` (la interfaz gráfica) viene
incluido con Python en Windows y macOS; en Linux a veces requiere
instalación aparte, indicada más abajo.

### Windows

**1. Instalar Python**

1. Ve a https://www.python.org/downloads/ y descarga la última versión
   de Python 3.
2. Ejecuta el instalador. En la primera pantalla, marca la casilla
   **"Add python.exe to PATH"** antes de pulsar "Install Now". Si no se
   marca, Windows no reconocerá el comando `python`.
3. Termina la instalación y abre una terminal nueva.
4. Verifica con:
   ```powershell
   python --version
   ```
   Debe mostrar algo como `Python 3.12.x`.

**2. Instalar ffmpeg**

Opción recomendada, usando winget (incluido en Windows 10/11):
```powershell
winget install ffmpeg
```
Cierra y vuelve a abrir la terminal después de instalarlo.

Opción manual, si winget no funciona:
1. Ve a https://www.gyan.dev/ffmpeg/builds/ y descarga
   `ffmpeg-release-essentials.zip`.
2. Descomprime el ZIP, por ejemplo en `C:\ffmpeg`.
3. Localiza la carpeta `bin` (ej. `C:\ffmpeg\bin`), que contiene
   `ffmpeg.exe`.
4. Añade esa ruta al PATH del sistema: menú Inicio → "Variables de
   entorno" → "Editar las variables de entorno del sistema" → "Variables
   de entorno..." → selecciona `Path` en "Variables del sistema" →
   "Editar" → "Nuevo" → pega la ruta a `bin`. Acepta todas las ventanas.
5. Abre una terminal nueva y verifica:
   ```powershell
   ffmpeg -version
   ```

**3. Colocar los archivos del proyecto**

Copia la carpeta `matrix_downloader` (con `main.py`, `requirements.txt` y
este `README.md`) a donde quieras, por ejemplo
`C:\Users\TuUsuario\Desktop\matrix_downloader`.

**4. Crear entorno virtual e instalar dependencias**

```powershell
cd C:\Users\TuUsuario\Desktop\matrix_downloader
python -m venv venv
venv\Scripts\activate
pip install -r requirements.txt
```
Debe aparecer `(venv)` al principio de la línea de comandos, y `pip`
habrá descargado `yt-dlp`.

**5. Ejecutar**

```powershell
python main.py
```

Para futuras ejecuciones, solo hace falta repetir los pasos 4
(`venv\Scripts\activate`) y 5 (`python main.py`).

### macOS

**1. Instalar Homebrew** (si no está instalado)

```bash
/bin/bash -c "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/HEAD/install.sh)"
```
Sigue las instrucciones en pantalla (puede pedir la contraseña de
usuario).

**2. Instalar Python**

```bash
brew install python
python3 --version
```

**3. Instalar ffmpeg**

```bash
brew install ffmpeg
ffmpeg -version
```

**4. Instalar Tkinter (solo si diera error al arrancar la app)**

```bash
brew install python-tk
```

**5. Colocar los archivos del proyecto**

Copia la carpeta `matrix_downloader` a donde quieras, por ejemplo a la
carpeta de Escritorio.

**6. Crear entorno virtual e instalar dependencias**

```bash
cd ~/Desktop/matrix_downloader
python3 -m venv venv
source venv/bin/activate
pip install -r requirements.txt
```

**7. Ejecutar**

```bash
python3 main.py
```

Para futuras ejecuciones, solo hace falta repetir
`source venv/bin/activate` y `python3 main.py`.

### Linux (Debian/Ubuntu y derivados)

**1. Instalar Python, pip, Tkinter y ffmpeg**

```bash
sudo apt update
sudo apt install python3 python3-pip python3-venv python3-tk ffmpeg -y
```

**2. Verificar la instalación**

```bash
python3 --version
pip3 --version
ffmpeg -version
```
Los tres comandos deben devolver una versión, sin errores.

**3. Colocar los archivos del proyecto**

Copia la carpeta `matrix_downloader` a la carpeta personal, por ejemplo
`~/matrix_downloader`.

**4. Crear entorno virtual e instalar dependencias**

```bash
cd ~/matrix_downloader
python3 -m venv venv
source venv/bin/activate
pip install -r requirements.txt
```

**5. Ejecutar**

```bash
python3 main.py
```

Para futuras ejecuciones, solo hace falta repetir
`source venv/bin/activate` y `python3 main.py`.

---

## Uso de la aplicación

1. Pega una URL de YouTube (vídeo suelto o playlist) en el campo `URL >`.
2. Pulsa **ANALIZAR**. Si es una playlist, se listan todos sus vídeos con
   casillas marcadas por defecto.
3. Haz clic sobre la columna de la casilla en cada fila para marcar o
   desmarcar elementos individuales, o usa **Seleccionar todos** /
   **Deseleccionar todos**.
4. Elige **Formato** (vídeo MP4/WEBM o audio MP3/M4A/WAV/FLAC) y
   **Calidad** (resolución para vídeo, bitrate para audio).
5. Elige la carpeta de salida (por defecto `./descargas`, o la carpeta
   junto al `.exe` si se usa el ejecutable portable).
6. Pulsa **DESCARGAR**. El progreso y los logs se muestran en tiempo
   real. Se puede cancelar en cualquier momento con **CANCELAR**, que
   termina el elemento en curso y detiene la cola.

---

## Solución de problemas — uso de la aplicación

Esta sección aplica tanto si se ejecuta desde código fuente como desde el
ejecutable compilado.

**`'python' no se reconoce como un comando...` (Windows, solo desde
código fuente)**
No se marcó "Add python.exe to PATH" al instalar. Reinstala Python
marcando esa casilla, o añade manualmente la carpeta de Python al PATH.

**`ModuleNotFoundError: No module named 'yt_dlp'` (solo desde código
fuente)**
No se activó el entorno virtual, o no se ejecutó
`pip install -r requirements.txt` dentro de él. Debe verse `(venv)` al
principio de la terminal antes de ejecutar `python main.py`.

**`ModuleNotFoundError: No module named 'tkinter'` (solo desde código
fuente)**
En Linux, instala `python3-tk`. En macOS, instala `python-tk` con
Homebrew. En Windows no debería ocurrir si se usó el instalador oficial
de python.org.

**"ffmpeg no detectado en el PATH" al abrir la aplicación**
- Ejecutando desde código fuente: ffmpeg no está instalado o no está en
  el PATH. Repite el paso de instalación de ffmpeg del sistema operativo
  correspondiente y abre una terminal nueva (los cambios de PATH no se
  aplican a terminales ya abiertas).
- Ejecutando el `.exe`: no debería aparecer, ya que ffmpeg va embebido en
  el ejecutable. Si aparece, el `.exe` se compiló sin que `ffmpeg.exe`
  estuviera presente durante la compilación; vuelve a compilarlo
  siguiendo la Opción A.

**La descarga falla con errores relacionados con formatos**
YouTube puede haber cambiado algo en su web. Actualiza `yt-dlp`:
```bash
pip install --upgrade yt-dlp
```
(Si se usa el `.exe`, esto requiere recompilarlo con la versión de
`yt-dlp` actualizada, siguiendo la Opción A.)

**El vídeo se descarga pero sin audio, o viceversa**
Comprueba que ffmpeg esté correctamente disponible. Ejecutando desde
código fuente, `ffmpeg -version` debe funcionar en la terminal; es el
encargado de unir las pistas de vídeo y audio.

---

## Estructura del proyecto

```
matrix_downloader/
├── main.py                      # Aplicación completa (GUI + lógica de descarga)
├── requirements.txt             # Dependencias para ejecución desde código fuente (yt-dlp)
├── MatrixDownloader_Build.bat   # Compilador todo-en-uno del ejecutable portable
├── MatrixDownloader.exe         # Ejecutable portable (generado al compilar, no incluido de origen)
├── README.md
└── descargas/                   # Carpeta de salida por defecto (se crea sola)
```

## Notas técnicas

- La extracción de metadatos de playlist usa `extract_flat` para ser
  rápida incluso con listas largas.
- Las descargas corren en un hilo (`threading`) para no bloquear la GUI;
  la comunicación con la interfaz se hace vía `queue.Queue` + `after()`,
  el patrón correcto en Tkinter para actualizar widgets desde otro hilo.
- Para vídeo se descarga `bestvideo+bestaudio` y se remuxa con ffmpeg al
  contenedor elegido (mp4/webm). Para audio se extrae con
  `FFmpegExtractAudio` al códec y bitrate elegidos.
- Si `ffmpeg` no está instalado (o no va embebido, en el caso del
  `.exe`), la app avisa pero permite continuar; algunas conversiones
  fallarán y se reportará en el log.
- `main.py` incluye una capa de compatibilidad para funcionar tanto
  ejecutado como script como compilado con PyInstaller:
  - `app_base_dir()` determina la carpeta base de la aplicación: la
    carpeta del propio `.exe` cuando corre compilado (`sys.frozen`), o
    la carpeta de `main.py` cuando corre como script. Se usa para ubicar
    la carpeta `descargas`.
  - `resource_path()` localiza recursos embebidos en el ejecutable
    (concretamente `ffmpeg.exe`) usando `sys._MEIPASS`, el directorio
    temporal donde PyInstaller descomprime los binarios incluidos con
    `--add-binary`. Si no encuentra un `ffmpeg.exe` embebido, recurre al
    PATH del sistema, de forma que el comportamiento es idéntico al
    ejecutar `main.py` directamente.
