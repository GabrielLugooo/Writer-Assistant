<img align="center" src="https://media.licdn.com/dms/image/v2/D4D16AQGUNxQ7NSC05A/profile-displaybackgroundimage-shrink_350_1400/profile-displaybackgroundimage-shrink_350_1400/0/1738695150340?e=1744243200&v=beta&t=oXX-ixT9bR3dJcYCLv4KBs5wjKFoeP0524kFGHQMYmQ" alt="gabriellugo" />

# WRITTER ASSISTANT

<a href="https://github.com/GabrielLugooo/Writter-Assistant" target="_blank" rel="noreferrer noopener"> <img align="center" src="https://img.shields.io/badge/English%20Writter%20Assistant-000000" alt="English Writter Assistant" /></a>
<a href="https://github.com/GabrielLugooo/Writter-Assistant/blob/main/README%20Spanish.md" target="_blank" rel="noreferrer noopener"> <img align="center" src="https://img.shields.io/badge/Spanish%20Writter%20Assistant-green" alt="Spanish Writter Assistant" /></a>

### Objective

WritterAssistant is a voice-powered writing assistant designed to transcribe spoken words into text efficiently. It provides a seamless user experience with real-time voice recognition, text saving, and integration with Google Docs, making it an ideal tool for writers, students, and professionals who need hands-free note-taking.

The application features a modern graphical user interface (GUI) built with Tkinter and integrates speech recognition, text-to-speech, and clipboard management functionalities. By automating text input through voice commands, it enhances productivity and accessibility.

### Skills Learned

- Implementing speech recognition using Python.
- Creating graphical user interfaces with `Tkinter`.
- Managing multithreading for responsive applications.
- Integrating text-to-speech (TTS) functionality.
- Handling file operations for text saving.
- Utilizing web browser automation for Google Docs integration.
- Implementing clipboard management in Python.

### Tools Used

![Static Badge](https://img.shields.io/badge/Python-000000?logo=python&logoSize=auto)

- Python
- `Tkinter` (GUI development)
- `SpeechRecognition` (voice input processing)
- `Pyttsx3` (text-to-speech synthesis)
- `Threading` (concurrent task management)
- `Webbrowser` (Google Docs integration)
- `FileDialog` and `MessageBox` (file handling and alerts)

FileDialog and MessageBox (file handling and alerts)

### Project

#### Code with Comments (English)

```python
# WritterAssistant

# Importar la librerías necesarias
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

# Configuración de la ventana principal de la aplicación
root = tk.Tk()
root.title("Asistente de Escritura")
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

### Limitations

Witter Assistant it's just for educational purpose under the MIT License.

---

<h3 align="left">Connect with me</h3>

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
<a href="https://github.com/GabrielLugooo/GabrielLugooo/blob/main/README.md" target="_blank" rel="noreferrer noopener"> <img align="center" src="https://img.shields.io/badge/English%20Version-000000" alt="English Version" /></a>
<a href="https://github.com/GabrielLugooo/GabrielLugooo/blob/main/Readme%20Spanish.md" target="_blank" rel="noreferrer noopener"> <img align="center" src="https://img.shields.io/badge/Spanish%20Version-Green" alt="Spanish Version" /></a>
</p>

<a href="https://linktr.ee/gabriellugooo" target="_blank" rel="noreferrer noopener"> <img align="center" src="https://img.shields.io/badge/Credits-Gabriel%20Lugo-green" alt="Credits" /></a>
<img align="center" src="https://komarev.com/ghpvc/?username=GabrielLugoo&label=Profile%20views&color=green&base=2000" alt="GabrielLugooo" />
<a href="" target="_blank" rel="noreferrer noopener"> <img align="center" src="https://img.shields.io/badge/License-MIT-green" alt="MIT License" /></a>
