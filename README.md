# Raspberry-blototh-

import os
import time
import tkinter as tk
from PIL import Image, ImageTk
from evdev import UInput, ecodes as e

# Funktion zum Konvertieren von Zeichen zu Keycodes
def char_to_keycode(char):
    mapping = {
        'a': e.KEY_A, 'b': e.KEY_B, 'c': e.KEY_C, 'd': e.KEY_D,
        'e': e.KEY_E, 'f': e.KEY_F, 'g': e.KEY_G, 'h': e.KEY_H,
        'i': e.KEY_I, 'j': e.KEY_J, 'k': e.KEY_K, 'l': e.KEY_L,
        'm': e.KEY_M, 'n': e.KEY_N, 'o': e.KEY_O, 'p': e.KEY_P,
        'q': e.KEY_Q, 'r': e.KEY_R, 's': e.KEY_S, 't': e.KEY_T,
        'u': e.KEY_U, 'v': e.KEY_V, 'w': e.KEY_W, 'x': e.KEY_X,
        'y': e.KEY_Y, 'z': e.KEY_Z,
        '0': e.KEY_0, '1': e.KEY_1, '2': e.KEY_2, '3': e.KEY_3,
        '4': e.KEY_4, '5': e.KEY_5, '6': e.KEY_6, '7': e.KEY_7,
        '8': e.KEY_8, '9': e.KEY_9,
        ' ': e.KEY_SPACE, ':': e.KEY_SEMICOLON, '/': e.KEY_SLASH,
        '.': e.KEY_DOT, '-': e.KEY_MINUS, '_': e.KEY_MINUS
    }
    return mapping.get(char.lower(), e.KEY_SPACE)

# Funktion zum Senden von Tastatureingaben
def send_keys(sequence):
    with UInput() as ui:
        for item in sequence:
            if isinstance(item, list):
                for code in item:
                    ui.write(e.EV_KEY, code, 1)
                for code in item:
                    ui.write(e.EV_KEY, code, 0)
                ui.syn()
            elif isinstance(item, str):
                for char in item:
                    key = char_to_keycode(char)
                    ui.write(e.EV_KEY, key, 1)
                    ui.write(e.EV_KEY, key, 0)
                    ui.syn()
                    time.sleep(0.005)  # 200 Zeichen pro Sekunde
            elif item == 'ENTER':
                ui.write(e.EV_KEY, e.KEY_ENTER, 1)
                ui.write(e.EV_KEY, e.KEY_ENTER, 0)
                ui.syn()
            time.sleep(0.1)

# Aktionen für die Buttons
def action_rickroll():
    sequence = [
        [e.KEY_LEFTMETA, e.KEY_R],
        'cmd /k start https://youtu.be/dQw4w9WgXcQ',
        'ENTER'
    ]
    send_keys(sequence)

def action_notepad():
    sequence = [
        [e.KEY_LEFTMETA, e.KEY_R],
        'notepad',
        'ENTER',
        'Hallo, dein Kissen ist am schlafen.',
        'ENTER'
    ]
    send_keys(sequence)

# GUI erstellen
root = tk.Tk()
root.title("Bluetooth Tastatur")
root.geometry("800x480")  # Anpassen an deine Bildschirmgröße

# Bild für den roten Knopf laden
img = Image.open("assets/red_button.png")
img = img.resize((100, 100))
button_image = ImageTk.PhotoImage(img)

# Buttons erstellen
button1 = tk.Button(root, image=button_image, command=action_rickroll)
button1.grid(row=0, column=0, padx=20, pady=20)
label1 = tk.Label(root, text="Rick Roll")
label1.grid(row=1, column=0)

button2 = tk.Button(root, text="Notepad", command=action_notepad, width=10, height=5)
button2.grid(row=0, column=1, padx=20, pady=20)
label2 = tk.Label(root, text="Notizen öffnen")
label2.grid(row=1, column=1)

root.mainloop()