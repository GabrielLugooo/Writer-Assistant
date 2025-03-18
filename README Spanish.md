<img align="center" src="https://media.licdn.com/dms/image/v2/D4D16AQGUNxQ7NSC05A/profile-displaybackgroundimage-shrink_350_1400/profile-displaybackgroundimage-shrink_350_1400/0/1738695150340?e=1744243200&v=beta&t=oXX-ixT9bR3dJcYCLv4KBs5wjKFoeP0524kFGHQMYmQ" alt="gabriellugo" />

# ASISTENTE DE ESCRITURA

<a href="https://github.com/GabrielLugooo/Writer-Assistant/blob/main/README%20Spanish.md" target="_blank" rel="noreferrer noopener"> <img align="center" src="https://img.shields.io/badge/Asistente%20Escritura%20Español-000000" alt="Asistente Escritura Español" /></a>
<a href="https://github.com/GabrielLugooo/Writer-Assistant" target="_blank" rel="noreferrer noopener"> <img align="center" src="https://img.shields.io/badge/Asistente%20Escritura%20Inglés-green" alt="Asistente Escritura Inglés" /></a>

### Objetivos

Assistente de Escritura es un asistente de escritura basado en voz diseñado para transcribir palabras habladas a texto de manera eficiente mediante asistencia automatizada. Su propósito es facilitar la creación de textos organizados y bien estructurados, proporcionando una interfaz intuitiva para mejorar la experiencia de escritura.

Ofrece una experiencia de usuario fluida con reconocimiento de voz en tiempo real, guardado de texto e integración con `Google Docs`, lo que lo convierte en una herramienta ideal para Su implementación busca combinar funcionalidad y facilidad de uso para escritores, estudiantes, profesionales, programadores y cualquier persona que necesitan tomar notas sin usar las manos e interesada en mejorar su productividad.

La aplicación cuenta con una interfaz gráfica moderna desarrollada con `Tkinter` e integra reconocimiento de voz `SpeechRecognition`, síntesis de texto a voz `text-to-speech` y funciones de administración del portapapeles. Al automatizar la entrada de texto mediante comandos de voz, mejora la productividad y la accesibilidad.

Además, este proyecto permite explorar conceptos clave en el desarrollo de software, incluyendo la gestión de entornos virtuales, la creación de ejecutables y la optimización del flujo de trabajo en Python.

### Habilidades Aprendidas

- Implementación de reconocimiento de voz con Python.
- Creación de interfaces gráficas con `Tkinter`.
- Gestión de multihilos para aplicaciones responsivas.
- Integración de funcionalidad de síntesis de voz (TTS).
- Manejo de archivos para guardar texto.
- Automatización de navegador web para integración con Google Docs.
- Implementación de gestión del portapapeles en Python.
- Integración de bibliotecas externas en un entorno Python.
- Creación y gestión de entornos virtuales con `venv`.
- Configuración y empaquetado de aplicaciones Python en ejecutables `.exe`.

### Herramientas Usadas

![Static Badge](https://img.shields.io/badge/Python-000000?logo=python&logoSize=auto)
![Static Badge](https://img.shields.io/badge/TKinter-000000?logo=tkinter&logoSize=auto)
![Static Badge](https://img.shields.io/badge/Speech%20Recognition-000000?logo=googletranslate&logoSize=auto)
![Static Badge](https://img.shields.io/badge/Pyttsx3-000000?logo=pyttsx3&logoSize=auto)
![Static Badge](https://img.shields.io/badge/Threading-000000?logo=threading&logoSize=auto)
![Static Badge](https://img.shields.io/badge/Webbrowser-000000?logo=webbrowser&logoSize=auto)
![Static Badge](https://img.shields.io/badge/Filedialog-000000?logo=filedialog&logoSize=auto)
![Static Badge](https://img.shields.io/badge/Google%20Docs-000000?logo=googledocs&logoSize=auto)
![Static Badge](https://img.shields.io/badge/MessageBox-000000?logo=mesagebox&logoSize=auto)
![Static Badge](https://img.shields.io/badge/PyInstaller-000000?logo=pyinstaller&logoSize=auto)
![Static Badge](https://img.shields.io/badge/venv-000000?logo=venv&logoSize=auto)
![Static Badge](https://img.shields.io/badge/.exe-000000?logo=dotexe&logoSize=auto)

- Python
- `Tkinter` (desarrollo de GUI)
- `SpeechRecognition` (procesamiento de entrada de voz)
- `Pyttsx3` (síntesis de texto a voz)
- `Threading` (gestión de tareas concurrentes)
- `Webbrowser` (integración con Google Docs)
- `FileDialog` y `MessageBox` (manejo de archivos y alertas)
- `venv` entorno virtual (virtual environment)
- `PyInstaller` para generar ejecutables `.exe` (paquetería y ejecución)

### Proyecto

#### Vista Previa

<img align="center" src="https://i.imgur.com/HJdRkEd.jpeg" alt="WritterAssistant_01" />
<img align="center" src="https://i.imgur.com/cNq3MCB.jpeg" alt="WritterAssistant_02" />
<img align="center" src="https://i.imgur.com/xuizL4G.jpeg" alt="WritterAssistant_03" />

#### Código con Comentarios (Español)

```python
# Writer Assistant

# Importar la librerías necesarias
import sys
import os
import tkinter as tk
from tkinter import scrolledtext, filedialog, messagebox
import speech_recognition as sr
import pyttsx3
import threading
import webbrowser

# Variable global para controlar la escucha
listening = False

# Función para iniciar la escucha del micrófono en un hilo separado
def start_listening():
    global listening
    listening = True
    threading.Thread(target=listen, daemon=True).start()

# Función para detener la escucha
def stop_listening():
    global listening
    listening = False

# Función para pausar y reanudar la escucha
def toggle_listening():
    global listening
    if listening:
        stop_listening()
        play_pause_button.config(text="▶ Play")
    else:
        start_listening()
        play_pause_button.config(text="⏸ Pause")

# Función que captura la voz y la convierte en texto
def listen():
    global listening
    recognizer = sr.Recognizer()  # Definir el objeto recognizer
    recognizer.pause_threshold = 1.5  # Ajusta el tiempo de espera antes de procesar la voz
    mic = sr.Microphone()

    with mic as source:
        recognizer.adjust_for_ambient_noise(source)  # Ajusta para reducir ruido ambiental
        while listening:
            try:
                print("Escuchando...")
                audio = recognizer.listen(source)  # Escucha sin un límite de tiempo
                text = recognizer.recognize_google(audio, language='es')  # Convierte el audio en texto
                text_area.insert(tk.END, text + " \n")  # Agrega el texto al área de texto
                text_area.see(tk.END)  # Desplaza automáticamente hacia abajo
            except sr.UnknownValueError:
                pass  # Si no entiende la voz, sigue esperando
            except sr.RequestError:
                messagebox.showerror("Error", "No se pudo conectar con el servicio de reconocimiento de voz.")
            except Exception as e:
                print(f"Error: {e}")

# Función para guardar el texto en un archivo de texto
def save_text():
    file_path = filedialog.asksaveasfilename(defaultextension=".txt", filetypes=[("Archivos de texto", "*.txt")])
    if file_path:
        with open(file_path, "w", encoding="utf-8") as file:
            file.write(text_area.get("1.0", tk.END))  # Guarda el contenido del área de texto
        messagebox.showinfo("Guardado", "Texto guardado con éxito.")

# Función para abrir un nuevo documento en Google Docs
def open_google_docs():
    webbrowser.open("https://docs.google.com/document/create")

# Función para copiar el texto al portapapeles y pegarlo en Google Docs
def copy_to_google_docs():
    root.clipboard_clear()
    root.clipboard_append(text_area.get("1.0", tk.END))  # Copia el contenido al portapapeles
    root.update()
    messagebox.showinfo("Copiado", "Texto copiado al portapapeles. Pega en Google Docs.")

# Configuración de la voz femenina para Asistente
def setup_voice():
    global engine
    engine = pyttsx3.init()
    voices = engine.getProperty('voices')
    for voice in voices:
        if "female" in voice.name.lower() or "mujer" in voice.name.lower():  # Busca una voz femenina
            engine.setProperty('voice', voice.id)
            break

# Función para dar la bienvenida al usuario en el hilo principal
def welcome_message():
    message = "Hola! soy tu asistente de escritura, por favor comienza a hablar y yo tomare nota de todo, para que puedas utilizarlo luego."
    threading.Thread(target=speak, args=(message,), daemon=True).start()

# Función para que la asistente hable sin bloquear la interfaz gráfica
def speak(message):
    engine.say(message)
    engine.runAndWait()

# Verificar si el script está ejecutándose como un archivo empaquetado
if getattr(sys, 'frozen', False):
    # Si estamos ejecutando desde el .exe empaquetado, la ruta será diferente
    icon_path = os.path.join(sys._MEIPASS, 'assets', 'writerassist.ico')
else:
    # Si estamos ejecutando el script en el entorno de desarrollo
    icon_path = 'assets/writerassist.ico'

# Configuración de la ventana principal de la aplicación
root = tk.Tk()
root.title("Writter Assistant")
root.iconbitmap(icon_path)
root.geometry("900x600")
root.minsize(800, 500)  # Tamaño mínimo
root.config(bg="#e0e0e0")

# Centrar la ventana
window_width = 900
window_height = 600
screen_width = root.winfo_screenwidth()
screen_height = root.winfo_screenheight()
position_top = int(screen_height / 2 - window_height / 2)
position_right = int(screen_width / 2 - window_width / 2)
root.geometry(f'{window_width}x{window_height}+{position_right}+{position_top}')

# Área de texto que se adapta al tamaño de la ventana
text_area = scrolledtext.ScrolledText(root, wrap=tk.WORD, font=("Segoe UI", 12), bg="#f9f9fe", fg="#000000", bd=0, relief="solid", highlightthickness=2)
text_area.grid(row=0, column=0, columnspan=5, sticky="nsew", padx=20, pady=20)

# Configuración de las filas y columnas para que se expandan correctamente
root.grid_rowconfigure(0, weight=1)  # Deja que la fila 0 (text_area) se expanda
root.grid_columnconfigure(0, weight=1)  # Deja que la columna 0 se expanda

# Contenedor de botones
frame_buttons = tk.Frame(root, bg="#e0e0e0")  # Creamos un frame para contener los botones
frame_buttons.grid(row=1, column=0, padx=20, pady=10, sticky="ew")  # Se coloca en una nueva fila

# Configuración común para los botones (tamaño, colores, fuentes)
button_config = {
    'bg': "#f8f8ff",
    'fg': "#000000",
    'font': ("Segoe UI", 12),
    'relief': "solid",
    'width': 12,
    'height': 2,
    'bd': 0,
    'borderwidth': 0,
    'highlightthickness': 2
}

# Botón de Play/Pause
play_pause_button = tk.Button(frame_buttons, text="⏸ Pause", command=toggle_listening, **button_config, activebackground="#388E3C", activeforeground="white")
play_pause_button.grid(row=0, column=0, padx=10, pady=5, sticky="ew")

# Botón de Detener
btn_stop = tk.Button(frame_buttons, text="🛑 Detener", command=stop_listening, **button_config, activebackground="#D32F2F", activeforeground="white")
btn_stop.grid(row=0, column=1, padx=10, pady=5, sticky="ew")

# Botón para guardar el texto
btn_save = tk.Button(frame_buttons, text="💾 Guardar", command=save_text, **button_config, activebackground="#1976D2", activeforeground="white")
btn_save.grid(row=0, column=2, padx=10, pady=5, sticky="ew")

# Botón para abrir un nuevo documento en Google Docs
btn_docs = tk.Button(frame_buttons, text="📄 Nuevo Docs", command=open_google_docs, **button_config, activebackground="#F57C00", activeforeground="white")
btn_docs.grid(row=0, column=3, padx=10, pady=5, sticky="ew")

# Botón para copiar a Google Docs (este botón se ajusta para ocupar más espacio horizontal)
btn_copy_docs = tk.Button(frame_buttons, text="📋 Copiar a Docs", command=copy_to_google_docs, **button_config, activebackground="#1976D2", activeforeground="white")
btn_copy_docs.grid(row=0, column=4, padx=10, pady=5, sticky="ew", columnspan=2)  # Ocupa más espacio horizontal

# Configuración para las columnas de la grilla, se asegura de que los botones se ajusten bien
frame_buttons.grid_columnconfigure(0, weight=1)
frame_buttons.grid_columnconfigure(1, weight=1)
frame_buttons.grid_columnconfigure(2, weight=1)
frame_buttons.grid_columnconfigure(3, weight=1)
frame_buttons.grid_columnconfigure(4, weight=2)  # Para el botón de copiar, con más peso

# Configuración de la voz de Asistente
setup_voice()

# Dar la bienvenida al usuario después de abrir la ventana
root.after(1000, welcome_message)

# Iniciar la escucha automáticamente después del saludo
root.after(3000, start_listening)

# Iniciar la aplicación
root.mainloop()
```

#### Creación del Entorno Virtual (`venv`)

Para aislar las dependencias del proyecto, se recomienda crear un entorno virtual. Sigue estos pasos:

1. Abrir una terminal en la carpeta del proyecto y ejecutar:

```sh
python -m venv venv
```

Esto te genera la carpeta del entorno virtual `venv`

2. Activar el entorno virtual:

- En Windows (Bash CMD):

```sh
venv\Scripts\activate
```

- En macOS/Linux (También en Git Bash en VSCode dentro de Win10):

```sh
source venv/bin/activate
```

3. Instalar las dependencias necesarias:

Antes, asegurarse de haber creado el archivo `requirements.txt` con la lista de librerias/dependecias
necesarias para que el proyecto se ejecute con su respectiva versión, por ej.: dentro del archivo:

```txt
SpeechRecognition==3.14.1
pyttsx3==2.98
pyaudio==0.2.14
pyinstaller==6.12.0
```

Luego:

```sh
pip install -r requirements.txt
```

**IMPORTANTE:** POR SEGURIDAD en este repositorio se borró la `API KEY` DE GOOGLE `SpeechRecognition` en : `venv/Lib/site-packages/speech_recognition/recognizers/google.py`, ya que solamente es de muestra de codigo funcionando correctamente con proposito educacional, para que funcione en tu `PC` deberias iniciar tu propio `venv`.

#### Creación del Ejecutable (`.exe`)

Para empaquetar el proyecto en un ejecutable de Windows, sigue estos pasos:

1. Asegurarse de tener instalado `pyinstaller`:

```sh
pip install pyinstaller
```

2. Ejecutar el siguiente comando para generar el `.exe`:

```sh
pyinstaller --onefile --windowed main.py
```

- `--onefile`: Crea un solo archivo ejecutable.

- `--windowed`: Evita que se abra una consola de terminal al ejecutar.

3. Al ejecutar PyInstaller, se generan varios archivos y carpetas adicionales:

La carpeta `build/` contiene archivos temporales utilizados durante el proceso de empaquetado.

El archivo `.spec` se genera automáticamente con las especificaciones del proyecto, lo podés personalizar si tenes el conocimiento (busca un tutorial de youtube).

La carpeta `dist/` es donde se encuentra el ejecutable final `.exe`, listo para su distribución y uso.

Solo el archivo dentro de `dist/` es necesario para ejecutar la aplicación.

**IMPORTANTE:** Dentro de la carpeta `dist/` se encuentra un ejecutable `writerassist.exe` totalmente funcional.

### Limitaciones

El Asistente de Escritura es solo para fines educativos bajo la licencia MIT.

---

<h3 align="left">Conecta Conmigo</h3>

<p align="left">
<a href="https://www.youtube.com/@gabriellugooo" target="_blank" rel="noreferrer noopener"> <img align="center" src="https://img.icons8.com/?size=50&id=55200&format=png" alt="@gabriellugooo" height="40" width="40" /></a>
<a href="http://www.tiktok.com/@gabriellugooo" target="_blank" rel="noreferrer noopener"> <img align="center" src="https://img.icons8.com/?size=50&id=118638&format=png" alt="@gabriellugooo" height="40" width="40" /></a>
<a href="https://instagram.com/lugooogabriel" target="_blank" rel="noreferrer noopener"> <img align="center" src="https://img.icons8.com/?size=50&id=32309&format=png" alt="lugooogabriel" height="40" width="40" /></a>
<a href="https://twitter.com/gabriellugo__" target="_blank" rel="noreferrer noopener"> <img align="center" src="https://img.icons8.com/?size=50&id=phOKFKYpe00C&format=png" alt="gabriellugo__" height="40" width="40" /></a>
<a href="https://www.linkedin.com/in/hernando-gabriel-lugo" target="_blank" rel="noreferrer noopener"> <img align="center" src="https://img.icons8.com/?size=50&id=8808&format=png" alt="hernando-gabriel-lugo" height="40" width="40" /></a>
<a href="https://github.com/GabrielLugooo" target="_blank" rel="noreferrer noopener"> <img align="center" src="https://img.icons8.com/?size=80&id=AngkmzgE6d3E&format=png" alt="gabriellugooo" height="34" width="34" /></a>
<a href="mailto:lugohernandogabriel@gmail.com"> <img align="center" src="https://img.icons8.com/?size=50&id=38036&format=png" alt="lugohernandogabriel@gmail.com" height="40" width="40" /></a>
<a href="https://linktr.ee/gabriellugooo" target="_blank" rel="noreferrer noopener"> <img align="center" src="https://simpleicons.org/icons/linktree.svg" alt="gabriellugooo" height="40" width="40" /></a>
</p>

<p align="left">
<a href="https://github.com/GabrielLugooo/GabrielLugooo/blob/main/Readme%20Spanish.md" target="_blank" rel="noreferrer noopener"> <img align="center" src="https://img.shields.io/badge/Versión%20Español-000000" alt="Versión Español" /></a>
<a href="https://github.com/GabrielLugooo/GabrielLugooo/blob/main/README.md" target="_blank" rel="noreferrer noopener"> <img align="center" src="https://img.shields.io/badge/Versión%20Inglés-Green" alt="Versión Inglés" /></a>

</p>

<a href="https://linktr.ee/gabriellugooo" target="_blank" rel="noreferrer noopener"> <img align="center" src="https://img.shields.io/badge/Créditos-Gabriel%20Lugo-green" alt="Créditos" /></a>
<img align="center" src="https://komarev.com/ghpvc/?username=GabrielLugoo&label=Vistas%20del%20Perfil&color=green&base=2000" alt="GabrielLugooo" />
<a href="" target="_blank" rel="noreferrer noopener"> <img align="center" src="https://img.shields.io/badge/License-MIT-green" alt="MIT License" /></a>
