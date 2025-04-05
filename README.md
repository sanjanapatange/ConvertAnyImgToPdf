# ConvertAnyImgToPdf
from PIL import Image
import os

def convert_images_to_pdf(image_paths, output_pdf_path):
    images = []

    for path in image_paths:
        img = Image.open(path)

        # Convert if image has alpha channel (e.g., PNG)
        if img.mode in ("RGBA", "P"):
            img = img.convert("RGB")

        images.append(img)

    # Save all images in a single PDF
    if images:
        images[0].save(output_pdf_path, save_all=True, append_images=images[1:])
        print(f"PDF created successfully at: {output_pdf_path}")
    else:
        print("No valid images found to convert.")

# Example usage
if __name__ == "__main__":
    # List your image file names in the order you want them in the PDF
    image_files = ["flower.jpg", "flow.jpg", "flow1.jpg"]  # Add your images here
    output_pdf = "combined_output.pdf"
    convert_images_to_pdf(image_files, output_pdf)
