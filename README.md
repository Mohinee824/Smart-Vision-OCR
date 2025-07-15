Frontend and Backend are still in work.
Technology Used
1. Optical Character Recognition (OCR)
Pytesseract: This is a Python wrapper for Google’s Tesseract-OCR Engine, which is an open-source OCR tool that can recognise text in images. It supports multiple languages and can be configured for various OCR tasks.
OpenCV: This library is used for image processing tasks such as reading images, converting them to grayscale, applying Gaussian blur, and detecting contours.
2. Image Processing
NumPy: A fundamental package for scientific computing with Python, used here for handling arrays and matrices, which are essential for image processing tasks.
Regular Expressions (re): Used for text tokenization and pattern matching, particularly for extracting dates from the recognised text.
3. Date Parsing
Dateutil: A powerful extension to the standard Python datetime module, used for parsing dates in various formats and normalising them.
Hardware Specifications
Processor: A multi-core processor (e.g., Intel i5/i7 or AMD Ryzen 5/7) is recommended for faster image processing and OCR tasks.
Memory: At least 8GB of RAM is recommended to handle large images and multiple processing tasks simultaneously.
Storage: Sufficient storage space (SSD preferred) to store images and output files. The exact requirement depends on the volume of images processed.
Software Specifications
Operating System: The script can run on Windows, macOS, or Linux. Ensure that the necessary libraries and dependencies are installed.
Python Version: Python 3.6 or higher is recommended for compatibility with the libraries used.
Libraries and Dependencies:
OpenCV: Install using pip install opencv-python.
Pytesseract: Install using pip install pytesseract. Additionally, Tesseract-OCR needs to be installed on the system. For installation:
Windows: Download the installer from the official Tesseract GitHub page.
macOS: Use Homebrew with the command brew install tesseract.
Linux: Use the package manager, e.g., sudo apt-get install tesseract-ocr.
NumPy: Install using pip install numpy.
Dateutil: Install using pip install python-dateutil.
How the OCR Works
Image Acquisition: Capturing the image using a scanner or camera.
Preprocessing: Enhancing the image quality through techniques like grayscale conversion, noise reduction, and thresholding.
Text Recognition: Using algorithms to identify and extract text from the processed image. Tesseract uses pattern matching and feature extraction for this purpose.
Post-Processing: Refining the recognised text, including correcting errors and formatting dates.
This script integrates these technologies and processes to automate the extraction and recognition of product information from images, making it a versatile tool for various applications.
