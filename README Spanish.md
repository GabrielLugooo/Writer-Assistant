<img align="center" src="https://media.licdn.com/dms/image/v2/D4D16AQGUNxQ7NSC05A/profile-displaybackgroundimage-shrink_350_1400/profile-displaybackgroundimage-shrink_350_1400/0/1738695150340?e=1744243200&v=beta&t=oXX-ixT9bR3dJcYCLv4KBs5wjKFoeP0524kFGHQMYmQ" alt="gabriellugo" />

# ASISTENTE DE VOZ

<a href="https://github.com/GabrielLugooo/Writter-Assistant/blob/main/README%20Spanish.md" target="_blank" rel="noreferrer noopener"> <img align="center" src="https://img.shields.io/badge/Asistente%20Escritura%20Español-000000" alt="Asistente Escritura Español" /></a>
<a href="https://github.com/GabrielLugooo/Writter-Assistant" target="_blank" rel="noreferrer noopener"> <img align="center" src="https://img.shields.io/badge/Asistente%20Escritura%20Inglés-green" alt="Asistente Escritura Inglés" /></a>

### Objetivos

Assistente de Escritura es un asistente de escritura basado en voz diseñado para transcribir palabras habladas a texto de manera eficiente. Ofrece una experiencia de usuario fluida con reconocimiento de voz en tiempo real, guardado de texto e integración con Google Docs, lo que lo convierte en una herramienta ideal para escritores, estudiantes y profesionales que necesitan tomar notas sin usar las manos.

La aplicación cuenta con una interfaz gráfica moderna desarrollada con Tkinter e integra reconocimiento de voz, síntesis de texto a voz y funciones de administración del portapapeles. Al automatizar la entrada de texto mediante comandos de voz, mejora la productividad y la accesibilidad.

### Habilidades Aprendidas

- Implementación de reconocimiento de voz con Python
- Creación de interfaces gráficas con `Tkinter`
- Gestión de multihilos para aplicaciones responsivas
- Integración de funcionalidad de síntesis de voz (TTS)
- Manejo de archivos para guardar texto
- Automatización de navegador web para integración con Google Docs
- Implementación de gestión del portapapeles en Python

### Herramientas Usadas

![Static Badge](https://img.shields.io/badge/Python-000000?logo=python&logoSize=auto)
![Static Badge](https://img.shields.io/badge/-000000?logo=tkinter&logoSize=auto)
![Static Badge](https://img.shields.io/badge/-000000?logo=googletranslate&logoSize=auto)
![Static Badge](https://img.shields.io/badge/-000000?logo=pyttsx3&logoSize=auto)
![Static Badge](https://img.shields.io/badge/-000000?logo=threading&logoSize=auto)
![Static Badge](https://img.shields.io/badge/-000000?logo=webbrowser&logoSize=auto)
![Static Badge](https://img.shields.io/badge/-000000?logo=filedialog&logoSize=auto)
![Static Badge](https://img.shields.io/badge/-000000?logo=googledocs&logoSize=auto)
![Static Badge](https://img.shields.io/badge/-000000?logo=mesagebox&logoSize=auto)

- Python
- `Tkinter` (desarrollo de GUI)
- `SpeechRecognition` (procesamiento de entrada de voz)
- `Pyttsx3` (síntesis de texto a voz)
- `Threading` (gestión de tareas concurrentes)
- `Webbrowser` (integración con Google Docs)
- `FileDialog` y `MessageBox` (manejo de archivos y alertas)

### Proyecto

#### Vista Previa

<img align="center" src="https://i.imgur.com/HJdRkEd.jpeg" alt="WritterAssistant_01" />
<img align="center" src="https://i.imgur.com/cNq3MCB.jpeg" alt="WritterAssistant_02" />
<img align="center" src="https://i.imgur.com/xuizL4G.jpeg" alt="WritterAssistant_03" />

#### Código con Comentarios (Español)

```python
#WritterAssistant

# Importar las bibliotecas necesarias
import tkinter as tk
from tkinter import scrolledtext, filedialog, messagebox
import speech_recognition as sr
import pyttsx3
import threading
import webbrowser

# Variable global para controlar la escucha
hearing = False

# Función para comenzar a escuchar el micrófono en un hilo separado
def start_listening():
escucha global
listening = True
threading.Thread(target=listen, daemon=True).start()

# Función para dejar de escuchar
def stop_listening():
escucha global
hearing = False

# Función para pausar y reanudar la escucha
def toggle_listening():
escucha global
if listening:
stop_listening()
play_pause_button.config(text="▶ Reproducir")
else:
start_listening()
play_pause_button.config(text="⏸ Pausa")

# Función que captura la voz y la convierte en texto
def listen():
escucha global
recognizer = mr. Recognizer() # Define el objeto reconocedor
recognizer. pause_threshold = 1.5 # Ajusta el tiempo de espera antes de procesar la voz
mic = mr. Microphone()

with mic as source:
recognizer. adjust_for_ambient_noise(source) # Ajusta para reducir el ruido ambiental
while listening:
try:
print("Escuchando...")
audio = perceiver. listen(source) # Escucha sin límite de tiempo
text = perceiver. perceive_google(audio, language='en') # Convierte audio en texto
text_area. insert(tk. END, text + " \n") # Agrega el texto al área de texto
text_area. see(tk. END) # Desplazarse hacia abajo automáticamente
except sr.UnknownValueError:
pass # Si no entiende la voz, sigue esperando
except sr.RequestError:
messagebox.showerror("Error", "No se pudo conectar al servicio de reconocimiento de voz.")
except Exception as e:
print(f"Error: {e}")

# Función para guardar texto en un archivo de texto
def save_text():
file_path = filedialog.asksaveasfilename(defaultextension=".txt", filetypes=[("Archivos de texto", "*.txt")])
if file_path:
with open(file_path, "w", encoding="utf-8") as file:
file.write(text_area.get("1.0", tk.END)) # Guardar el contenido del área de texto
messagebox.showinfo("Guardado", "Texto guardado correctamente.")

# Función para abrir un nuevo documento en Google Docs
def open_google_docs():
webbrowser.open("https://docs.google.com/document/create")

# Función para copiar texto al portapapeles y pegarlo en Google Docs
def copy_to_google_docs():
root.clipboard_clear()
root.clipboard_append(text_area.get("1.0", tk.END)) # Copiar contenido al portapapeles
root.update()
messagebox.showinfo("Copiado", "Texto copiado al portapapeles. Pegar en Google Docs.")

# Configuración de voz femenina para el Asistente
def setup_voice():
global engine
engine = pyttsx3.init()
voices = engine.getProperty('voices')
for voice in voices:
if "female" in voice.name.lower() or "woman" in voice.name.lower(): # Encontrar una voz femenina
engine.setProperty('voice', voice.id)
break

# Función para dar la bienvenida al usuario en el hilo principal
def welcome_message():
message = "¡Hola! Soy tu asistente de escritura, por favor empieza a hablar y tomaré notas de todo, para que puedas usarlo después."
threading.Thread(target=speak, args=(message,), daemon=True).start()

# Función para que el asistente hable sin bloquear la interfaz gráfica
def speak(message):
engine.say(message)
engine.runAndWait()

# Configuración de la ventana principal de la aplicación
root = tk.Tk()
root.title("Asistente de escritura")
root.geometry("900x600")
root.minsize(800, 500) # Tamaño mínimo
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

# Configuración de filas y columnas para expandir correctamente
root.grid_rowconfigure(0, weight=1) # Permite que la fila 0 (text_area) se expanda
root.grid_columnconfigure(0, weight=1) # Permite que la columna 0 se expanda

# Contenedor de botones
frame_buttons = tk.Frame(root, bg="#e0e0e0") # Crea un marco para contener los botones
frame_buttons.grid(row=1, column=0, padx=20, pady=10, sticky="ew") # Lo coloca en una nueva fila

# Configuraciones comunes para botones
```

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
