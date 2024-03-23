# HELP-Is-
Can someone help me make this code in zip tabs with an interface look like GPU or CPU or Asiic?  
import hashlib
import tkinter as tk
from tkinter import ttk
import threading
import pyperclip

class HashGeneratorApp:
    def __init__(self, master):
        self.master = master
        self.master.title("Hash Generator")

        self.running = False
        self.hashes = {}

        self.frame = ttk.Frame(self.master)
        self.frame.pack(padx=10, pady=10)

        self.start_button = ttk.Button(self.frame, text="Start", command=self.start_generation)
        self.start_button.grid(row=0, column=0, padx=5, pady=5)

        self.stop_button = ttk.Button(self.frame, text="Stop", command=self.stop_generation, state=tk.DISABLED)
        self.stop_button.grid(row=0, column=1, padx=5, pady=5)

        self.copy_button = ttk.Button(self.frame, text="Copy Hashes", command=self.copy_hashes, state=tk.DISABLED)
        self.copy_button.grid(row=0, column=2, padx=5, pady=5)

        self.emulator_text = tk.Text(self.frame, wrap=tk.WORD, width=50, height=20)
        self.emulator_text.grid(row=1, column=0, columnspan=3, padx=5, pady=5)

    def generate_hashes(self):
        for i in range(20_000_000_000_000_000_000_000_000_000_000):
            if not self.running:
                break
            num_str = str(i)
            hash_value = hashlib.sha256(num_str.encode()).digest()
            self.hashes[i] = hash_value
            self.emulator_text.insert(tk.END, f"Number: {i}, Hash: {hash_value}\n")
            pyperclip.copy(hash_value)
        self.emulator_text.insert(tk.END, "Hashes generated successfully!")
        self.running = False
        self.start_button.config(state=tk.NORMAL)
        self.stop_button.config(state=tk.DISABLED)
        self.copy_button.config(state=tk.NORMAL)

    def start_generation(self):
        self.running = True
        self.start_button.config(state=tk.DISABLED)
        self.stop_button.config(state=tk.NORMAL)
        self.copy_button.config(state=tk.DISABLED)
        threading.Thread(target=self.generate_hashes).start()

    def stop_generation(self):
        self.running = False

    def copy_hashes(self):
        self.master.clipboard_clear()
        self.master.clipboard_append(self.emulator_text.get(1.0, tk.END))

def main():
    root = tk.Tk()
    app = HashGeneratorApp(root)
    root.mainloop()

if __name__ == "__main__":
    main()
