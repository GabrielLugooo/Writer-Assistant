<img align="center" src="https://media.licdn.com/dms/image/v2/D4D16AQGUNxQ7NSC05A/profile-displaybackgroundimage-shrink_350_1400/profile-displaybackgroundimage-shrink_350_1400/0/1738695150340?e=1744243200&v=beta&t=oXX-ixT9bR3dJcYCLv4KBs5wjKFoeP0524kFGHQMYmQ" alt="gabriellugo" />

# WRITER ASSISTANT

<a href="https://github.com/GabrielLugooo/Writer-Assistant-Public" target="_blank" rel="noreferrer noopener"> <img align="center" src="https://img.shields.io/badge/English%20Writer%20Assistant-000000" alt="English Writer Assistant" /></a>
<a href="https://github.com/GabrielLugooo/Writer-Assistant-Public/blob/main/README%20Spanish.md" target="_blank" rel="noreferrer noopener"> <img align="center" src="https://img.shields.io/badge/Spanish%20Writer%20Assistant-green" alt="Spanish Writer Assistant" /></a>

### Objective

Writing Assistant is a voice-based writing assistant designed to efficiently transcribe spoken words into text through automation assistance. Its purpose is to facilitate the creation of organized and well-structured texts, providing an intuitive interface to improve the writing experience.

It offers a fluid user experience with real-time speech recognition, text saving, and integration with `Google Docs`, making it an ideal tool for writers, students, professionals, programmers, and anyone who needs to take hands-free notes and is interested in improving their productivity.

The application features a modern graphical interface developed with `Tkinter` and integrates `SpeechRecognition`, `text-to-speech` synthesis, and clipboard management functions. By automating text entry through voice commands, it improves productivity and accessibility.

Furthermore, this project explores key concepts in software development, including virtual environment management, executable creation, and workflow optimization in Python.

### Skills Learned

- Implementing speech recognition using Python.
- Creating graphical user interfaces with `Tkinter`.
- Managing multithreading for responsive applications.
- Integrating text-to-speech (TTS) functionality.
- Handling file operations for text saving.
- Utilizing web browser automation for Google Docs integration.
- Implementing clipboard management in Python.
- Integration of external libraries into a Python environment.
- Creation and management of virtual environments with `venv`.
- Configuration and packaging of Python applications into `.exe` executables.

### Tools Used

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
- `Tkinter` (GUI development)
- `SpeechRecognition` (voice input processing)
- `Pyttsx3` (text-to-speech synthesis)
- `Threading` (concurrent task management)
- `Webbrowser` (Google Docs integration)
- `FileDialog` and `MessageBox` (file handling and alerts)
- `venv` virtual environment
- `PyInstaller` to generate `.exe` executables (packaging and execution)

### Project

#### Preview

<img align="center" src="https://i.imgur.com/HJdRkEd.jpeg" alt="WritterAssistant_01" />
<img align="center" src="https://i.imgur.com/cNq3MCB.jpeg" alt="WritterAssistant_02" />
<img align="center" src="https://i.imgur.com/xuizL4G.jpeg" alt="WritterAssistant_03" />

#### Code with Comments (English)

```python
# Writer Assistant

# Import the necessary libraries
import sys
import os
import tkinter as tk
from tkinter import scrolledtext, filedialog, messagebox
import speech_recognition as sr
import pyttsx3
import threading
import webbrowser

# Global variable to control listening
listening = False

# Function to start microphone listening in a separate thread
def start_listening():
    global listening
    listening = True
    threading.Thread(target=listen, daemon=True).start()

# Function to stop listening
def stop_listening():
    global listening
    listening = False

# Pause and resume listening function
def toggle_listening():
    global listening
    if listening:
        stop_listening()
        play_pause_button.config(text="▶ Play")
    else:
        start_listening()
        play_pause_button.config(text="⏸ Pause")

# Function that captures voice and converts it into text
def listen():
    global listening
    recognizer = sr.Recognizer()  # Define the recognizer object
    recognizer.pause_threshold = 1.5  # Adjusts the wait time before processing the voice
    mic = sr.Microphone()

    with mic as source:
        recognizer.adjust_for_ambient_noise(source)  # Adjust to reduce ambient noise
        while listening:
            try:
                print("Escuchando...") # Change to "Listening" for 'en' language
                audio = recognizer.listen(source)  # Listen without a time limit
                text = recognizer.recognize_google(audio, language='es')  # Convert audio to text (change the language to 'en')
                text_area.insert(tk.END, text + " \n")  # Add the text to the text area
                text_area.see(tk.END)  # Automatically scroll down
            except sr.UnknownValueError:
                pass  # If don't understand the voice, keep waiting
            except sr.RequestError:
                messagebox.showerror("Error", "No se pudo conectar con el servicio de reconocimiento de voz.") # Change for "Could not connect to the speech recognition service"
            except Exception as e:
                print(f"Error: {e}")

# Function to save the text to a text file
def save_text():
    file_path = filedialog.asksaveasfilename(defaultextension=".txt", filetypes=[("Archivos de texto", "*.txt")]) # Change for "Text Files"
    if file_path:
        with open(file_path, "w", encoding="utf-8") as file:
            file.write(text_area.get("1.0", tk.END))  # Save the contents of the text area
        messagebox.showinfo("Guardado", "Texto guardado con éxito.") # Change for "Saved", "Text saved successfully.")

# Function to open a new document in Google Docs
def open_google_docs():
    webbrowser.open("https://docs.google.com/document/create")

# Function to copy text to the clipboard and paste it into Google Docs
def copy_to_google_docs():
    root.clipboard_clear()
    root.clipboard_append(text_area.get("1.0", tk.END)) # Copy the contents to the clipboard
    root.update()
    messagebox.showinfo("Copiado", "Texto copiado al portapapeles. Pega en Google Docs.") # Change for "Copied", "Text copied to clipboard. Paste into Google Docs."

# Setting the female voice for Assistant
def setup_voice():
    global engine
    engine = pyttsx3.init()
    voices = engine.getProperty('voices')
    for voice in voices:
        if "female" in voice.name.lower() or "mujer" in voice.name.lower(): # Look for a female voice
            engine.setProperty('voice', voice.id)
            break

# Function to welcome the user to the main thread
def welcome_message():
    message = "Hola! soy tu asistente de escritura, por favor comienza a hablar y yo tomare nota de todo, para que puedas utilizarlo luego." # Change for "Hi! I'm your writing assistant. Please start talking, and I'll take notes for you to use later."
    threading.Thread(target=speak, args=(message,), daemon=True).start()

# Function for the assistant to speak without blocking the graphical interface
def speak(message):
    engine.say(message)
    engine.runAndWait()

# Check if the script is running as a packed file
if getattr(sys, 'frozen', False):
    # If we are running from the packaged .exe, the path will be different
    icon_path = os.path.join(sys._MEIPASS, 'assets', 'writerassist.ico')
else:
    # If we are running the script in the development environment
    icon_path = 'assets/writerassist.ico'

# Configuring the main application window
root = tk.Tk()
root.title("Writter Assistant")
root.iconbitmap(icon_path)
root.geometry("900x600")
root.minsize(800, 500) # Minimum size
root.config(bg="#e0e0e0")

# Center the window
window_width = 900
window_height = 600
screen_width = root.winfo_screenwidth()
screen_height = root.winfo_screenheight()
position_top = int(screen_height / 2 - window_height / 2)
position_right = int(screen_width / 2 - window_width / 2)
root.geometry(f'{window_width}x{window_height}+{position_right}+{position_top}')

# Text area that adapts to the size of the window
text_area = scrolledtext.ScrolledText(root, wrap=tk.WORD, font=("Segoe UI", 12), bg="#f9f9fe", fg="#000000", bd=0, relief="solid", highlightthickness=2)
text_area.grid(row=0, column=0, columnspan=5, sticky="nsew", padx=20, pady=20)

# Setting rows and columns to expand correctly
root.grid_rowconfigure(0, weight=1)  # Let row 0 (text_area) expand
root.grid_columnconfigure(0, weight=1)  # Deja que la columna 0 se expanda

# Button container
frame_buttons = tk.Frame(root, bg="#e0e0e0")  # We create a frame to contain the buttons
frame_buttons.grid(row=1, column=0, padx=20, pady=10, sticky="ew")  # Se coloca en una nueva fila

# Common settings for buttons (size, colors, fonts)
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

# Play/Pause button
play_pause_button = tk.Button(frame_buttons, text="⏸ Pause", command=toggle_listening, **button_config, activebackground="#388E3C", activeforeground="white")
play_pause_button.grid(row=0, column=0, padx=10, pady=5, sticky="ew")

# Stop button
btn_stop = tk.Button(frame_buttons, text="🛑 Detener", command=stop_listening, **button_config, activebackground="#D32F2F", activeforeground="white")
btn_stop.grid(row=0, column=1, padx=10, pady=5, sticky="ew")

# Save text button
btn_save = tk.Button(frame_buttons, text="💾 Guardar", command=save_text, **button_config, activebackground="#1976D2", activeforeground="white")
btn_save.grid(row=0, column=2, padx=10, pady=5, sticky="ew")

# Open a new Google Docs button
btn_docs = tk.Button(frame_buttons, text="📄 Nuevo Docs", command=open_google_docs, **button_config, activebackground="#F57C00", activeforeground="white")
btn_docs.grid(row=0, column=3, padx=10, pady=5, sticky="ew")

# Copy to Google Docs button (this button adjusts to take up more horizontal space)
btn_copy_docs = tk.Button(frame_buttons, text="📋 Copiar a Docs", command=copy_to_google_docs, **button_config, activebackground="#1976D2", activeforeground="white")
btn_copy_docs.grid(row=0, column=4, padx=10, pady=5, sticky="ew", columnspan=2) # Takes up more horizontal space

# Configuration for grid columns, makes sure buttons fit well
frame_buttons.grid_columnconfigure(0, weight=1)
frame_buttons.grid_columnconfigure(1, weight=1)
frame_buttons.grid_columnconfigure(2, weight=1)
frame_buttons.grid_columnconfigure(3, weight=1)
frame_buttons.grid_columnconfigure(4, weight=2)  # For the copy button, with more weight

# Assistant Voice Settings
setup_voice()

# Welcome the user after opening the window
root.after(1000, welcome_message)

# Start listening automatically after greeting
root.after(3000, start_listening)

# Start the application
root.mainloop()
```

#### Creating the Virtual Environment (`venv`)

To isolate project dependencies, it is recommended to create a virtual environment. Follow these steps:

1. Open a terminal in the project folder and run:

   ```sh
   python -m venv venv
   ```

   This will create the `venv` virtual environment folder.

2. Activate the virtual environment:

   _On Windows_ (Bash CMD):

   ```sh
   venv\Scripts\activate
   ```

   _On macOS/Linux (also in Git Bash in VSCode within Win10):_

   ```sh
   source venv/bin/activate
   ```

3. Install the necessary dependencies:

   First, make sure you have created the `requirements.txt` file with the list of libraries/dependencies required for the project to run with its respective version, e.g., within the file:

   ```txt
   SpeechRecognition==3.14.1
   pyttsx3==2.98
   pyaudio==0.2.14
   pyinstaller==6.12.0
   ```

   Then:

   ```sh
   pip install -r requirements.txt
   ```

**IMPORTANT:** FOR SECURITY, the GOOGLE API KEY `SpeechRecognition` has been deleted from this repository in: `venv/Lib/site-packages/speech_recognition/recognizers/google.py`, since it is only a sample of code working correctly for educational purposes. For it to work on your `PC` you should start your own `venv`.

#### Creating the Executable (`.exe`)

To package the project into a Windows executable, follow these steps:

1. Make sure you have `pyinstaller` installed:

   ```sh
   pip install pyinstaller
   ```

2. Run the following command to generate the `.exe`:

   ```sh
   pyinstaller --onefile --windowed main.py
   ```

   `--onefile`: Creates a single executable file.

   `--windowed`: Prevents a terminal console from opening when running.

3. When you run PyInstaller, several additional files and folders are generated:

   The `build/` folder contains temporary files used during the packaging process.

   The `.spec` file is automatically generated with the project specifications; you can customize it if you have the knowledge (look for a YouTube tutorial).

   The `dist/` folder contains the final `.exe` executable, ready for distribution and use.

   Only the file inside `dist/` is required to run the application.

**IMPORTANT:** Inside the `dist/` folder is a fully functional `writerassist.exe` executable.

### Limitations

Writer Assistant it's just for educational purpose under the MIT License.

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
