import tkinter as tk
from tkinter import filedialog, messagebox
from PIL import Image, ImageTk
import os

def convert_image_to_pdf(image_path, output_pdf_path):
    image = Image.open(image_path)
    if image.mode in ("RGBA", "P"):
        image = image.convert("RGB")
    a4_width, a4_height = 600, 500
    image.thumbnail((a4_width, a4_height))
    image.save(output_pdf_path, "PDF")

def browse_file():
    file_path = filedialog.askopenfilename(
        title="Select an Image",
        filetypes=[("Image Files", "*.png *.jpg *.jpeg *.bmp *.gif")]
    )
    if file_path:
        entry_path.delete(0, tk.END)
        entry_path.insert(0, file_path)
        show_image_preview(file_path)

def show_image_preview(path):
    try:
        image = Image.open(path)
        image.thumbnail((150, 150))  # Resize to thumbnail
        photo = ImageTk.PhotoImage(image)
        preview_label.config(image=photo)
        preview_label.image = photo  # Keep reference to avoid garbage collection
    except Exception as e:
        messagebox.showerror("Error", f"Could not load image preview:\n{str(e)}")

def save_pdf():
    image_path = entry_path.get()
    if not image_path:
        messagebox.showwarning("Warning", "Please select an image file.")
        return

    output_path = filedialog.asksaveasfilename(
        defaultextension=".pdf",
        filetypes=[("PDF files", "*.pdf")],
        title="Save PDF As"
    )

    if output_path:
        try:
            convert_image_to_pdf(image_path, output_path)
            messagebox.showinfo("Success", f"PDF saved at:\n{output_path}")
        except Exception as e:
            messagebox.showerror("Error", f"Failed to convert image.\n{str(e)}")

# Tkinter UI setup
root = tk.Tk()
root.title("Image to PDF Converter")
root.geometry("450x350")
root.resizable(False, False)

label = tk.Label(root, text="Select an image to convert to PDF:")
label.pack(pady=10)

entry_path = tk.Entry(root, width=50)
entry_path.pack(pady=5)

browse_button = tk.Button(root, text="Browse", command=browse_file)
browse_button.pack(pady=5)

# Image preview section
preview_label = tk.Label(root, text="Image preview will appear here", width=80, height=80, bg="lightgray")
preview_label.pack(pady=10)

convert_button = tk.Button(root, text="Convert to PDF", command=save_pdf)
convert_button.pack(pady=20)

root.mainloop()
