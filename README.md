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
![Static Badge](https://img.shields.io/badge/TKinter-000000?logo=tkinter&logoSize=auto)
![Static Badge](https://img.shields.io/badge/Speech%20Recognition-000000?logo=googletranslate&logoSize=auto)
![Static Badge](https://img.shields.io/badge/Pyttsx3-000000?logo=pyttsx3&logoSize=auto)
![Static Badge](https://img.shields.io/badge/Threading-000000?logo=threading&logoSize=auto)
![Static Badge](https://img.shields.io/badge/Webbrowser-000000?logo=webbrowser&logoSize=auto)
![Static Badge](https://img.shields.io/badge/Filedialog-000000?logo=filedialog&logoSize=auto)
![Static Badge](https://img.shields.io/badge/Google%20Docs-000000?logo=googledocs&logoSize=auto)
![Static Badge](https://img.shields.io/badge/MessageBox-000000?logo=mesagebox&logoSize=auto)

- Python
- `Tkinter` (GUI development)
- `SpeechRecognition` (voice input processing)
- `Pyttsx3` (text-to-speech synthesis)
- `Threading` (concurrent task management)
- `Webbrowser` (Google Docs integration)
- `FileDialog` and `MessageBox` (file handling and alerts)

### Project

#### Preview

<img align="center" src="https://i.imgur.com/HJdRkEd.jpeg" alt="WritterAssistant_01" />
<img align="center" src="https://i.imgur.com/cNq3MCB.jpeg" alt="WritterAssistant_02" />
<img align="center" src="https://i.imgur.com/xuizL4G.jpeg" alt="WritterAssistant_03" />

#### Code with Comments (English)

```python
# WritterAssistant

# Import the necessary libraries
import tkinter as tk
from tkinter import scrolledtext, filedialog, messagebox
import speech_recognition as sr
import pyttsx3
import threading
import webbrowser

# Global variable to control listening
listening = False

# Function to start listening to the microphone in a separate thread
def start_listening():
global listening
listening = True
threading.Thread(target=listen, daemon=True).start()

# Function to stop listening
def stop_listening():
global listening
listening = False

# Function to pause and resume listening
def toggle_listening():
global listening
if listening:
stop_listening()
play_pause_button.config(text="▶ Play")
else:
start_listening()
play_pause_button.config(text="⏸ Pause")

# Function that captures voice and converts it to text
def listen():
global listening
recognizer = sr. Recognizer() # Define the recognizer object
recognizer. pause_threshold = 1.5 # Adjust the wait time before processing the voice
mic = sr. Microphone()

with mic as source:
recognizer. adjust_for_ambient_noise(source) # Adjust to reduce ambient noise
while listening:
try:
print("Listening...")
audio = recognizer. listen(source) # Listen without a time limit
text = recognizer. recognize_google(audio, language='en') # Convert audio to text
text_area. insert(tk. END, text + " \n") # Add the text to the text area
text_area. see(tk. END) # Automatically scroll down
except sr.UnknownValueError:
pass # If it doesn't understand the voice, it keeps waiting
except sr.RequestError:
messagebox.showerror("Error", "Could not connect to the speech recognition service.")
except Exception as e:
print(f"Error: {e}")

# Function to save text to a text file
def save_text():
file_path = filedialog.asksaveasfilename(defaultextension=".txt", filetypes=[("Text files", "*.txt")])
if file_path:
with open(file_path, "w", encoding="utf-8") as file:
file.write(text_area.get("1.0", tk.END)) # Save the content of the text area
messagebox.showinfo("Saved", "Text saved successfully.")

# Function to open a new document in Google Docs
def open_google_docs():
webbrowser.open("https://docs.google.com/document/create")

# Function to copy text to clipboard and paste into Google Docs
def copy_to_google_docs():
root.clipboard_clear()
root.clipboard_append(text_area.get("1.0", tk.END)) # Copy content to clipboard
root.update()
messagebox.showinfo("Copied", "Text copied to clipboard. Paste into Google Docs.")

# Setting up female voice for Assistant
def setup_voice():
global engine
engine = pyttsx3.init()
voices = engine.getProperty('voices')
for voice in voices:
if "female" in voice.name.lower() or "woman" in voice.name.lower(): # Find a female voice
engine.setProperty('voice', voice.id)
break

# Function to welcome the user in the main thread
def welcome_message():
message = "Hello! I am your writing assistant, please start speaking and I will take notes of everything, so you can use it later."
threading.Thread(target=speak, args=(message,), daemon=True).start()

# Function for the assistant to speak without blocking the graphical interface
def speak(message):
engine.say(message)
engine.runAndWait()

# Configuration of the main application window
root = tk.Tk()
root.title("Writing Assistant")
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
root.grid_rowconfigure(0, weight=1) # Let the row 0 (text_area) expands
root.grid_columnconfigure(0, weight=1) # Let column 0 expand

# Button container
frame_buttons = tk.Frame(root, bg="#e0e0e0") # Create a frame to hold the buttons
frame_buttons.grid(row=1, column=0, padx=20, pady=10, sticky="ew") # Places it in a new row

# Common settings for buttons
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
