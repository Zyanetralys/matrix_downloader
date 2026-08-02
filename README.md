# MATRIX DOWNLOADER // 動画抽出システム

Aplicación de escritorio 100% local (Python + Tkinter) para descargar vídeos o
audio de YouTube, tanto de URLs sueltas como de playlists completas con
selección individual de elementos.

## ⚠️ Nota legal

Esta herramienta **no aloja ni distribuye contenido**: simplemente automatiza
`yt-dlp` en tu propio equipo. Úsala solo para descargar vídeos de los que
tengas los derechos. El uso para saltarse el copyright de
contenido protegido queda fuera del propósito de este proyecto académico.

## Requisitos

1. **Python 3.9+**
2. **ffmpeg** instalado y accesible desde el PATH (necesario para unir
   vídeo+audio y para convertir a MP3/WAV/FLAC/M4A):
   - Windows: `winget install ffmpeg` o descarga desde https://ffmpeg.org/download.html
     y añade la carpeta `bin` al PATH.
   - macOS: `brew install ffmpeg`
   - Linux (Debian/Ubuntu): `sudo apt install ffmpeg`

## Instalación

```bash
cd matrix_downloader
python3 -m venv venv
source venv/bin/activate      # En Windows: venv\Scripts\activate
pip install -r requirements.txt
```

## Ejecución

```bash
python3 main.py
```

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
