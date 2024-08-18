import tkinter as tk
from tkinter import messagebox
import random
import string
import pyperclip

def generate_password():
    length = int(length_entry.get())
    use_uppercase = uppercase_var.get()
    use_lowercase = lowercase_var.get()
    use_digits = digits_var.get()
    use_special = special_var.get()

    character_pool = ''
    if use_uppercase:
        character_pool += string.ascii_uppercase
    if use_lowercase:
        character_pool += string.ascii_lowercase
    if use_digits:
        character_pool += string.digits
    if use_special:
        character_pool += string.punctuation

    if not character_pool:
        messagebox.showerror("Error", "Please select at least one character type.")
        return

    password = ''.join(random.choice(character_pool) for _ in range(length))
    if use_uppercase and not any(c.isupper() for c in password):
        password += random.choice(string.ascii_uppercase)
    if use_lowercase and not any(c.islower() for c in password):
        password += random.choice(string.ascii_lowercase)
    if use_digits and not any(c.isdigit() for c in password):
        password += random.choice(string.digits)
    if use_special and not any(c in string.punctuation for c in password):
        password += random.choice(string.punctuation)
    password = password[:length]

    password_entry.delete(0, tk.END)
    password_entry.insert(0, password)

def copy_to_clipboard():
    pyperclip.copy(password_entry.get())
    messagebox.showinfo("Copied", "Password copied to clipboard!")

root = tk.Tk()
root.title(" Best Password Generator")

tk.Label(root, text="Password Length:").grid(row=0, column=0, padx=10, pady=10)
length_entry = tk.Entry(root)
length_entry.grid(row=0, column=1, padx=10, pady=10)
length_entry.insert(0, "12") 

uppercase_var = tk.BooleanVar(value=True)
lowercase_var = tk.BooleanVar(value=True)
digits_var = tk.BooleanVar(value=True)
special_var = tk.BooleanVar(value=True)

tk.Checkbutton(root, text="Include Uppercase Letters", variable=uppercase_var).grid(row=1, column=0, columnspan=2, sticky='w', padx=10)
tk.Checkbutton(root, text="Include Lowercase Letters", variable=lowercase_var).grid(row=2, column=0, columnspan=2, sticky='w', padx=10)
tk.Checkbutton(root, text="Include Digits", variable=digits_var).grid(row=3, column=0, columnspan=2, sticky='w', padx=10)
tk.Checkbutton(root, text="Include Special Characters", variable=special_var).grid(row=4, column=0, columnspan=2, sticky='w', padx=10)

generate_button = tk.Button(root, text="Generate Password", command=generate_password)
generate_button.grid(row=5, column=0, columnspan=2, pady=10)

password_entry = tk.Entry(root, width=40)
password_entry.grid(row=6, column=0, columnspan=2, padx=10, pady=10)

copy_button = tk.Button(root, text="Copy to Clipboard", command=copy_to_clipboard)
copy_button.grid(row=7, column=0, columnspan=2, pady=10)

root.mainloop()
 
