import cv2  
import numpy as np  
import pytesseract  
import re  
import dateutil.parser  
  
# Load the image  
img = cv2.imread('/Users/abc/Downloads/sample.jpg')  
gray = cv2.cvtColor(img, cv2.COLOR_BGR2GRAY)  
blurred = cv2.GaussianBlur(gray, (5, 5), 0)  
binary = cv2.adaptiveThreshold(blurred, 255, cv2.ADAPTIVE_THRESH_GAUSSIAN_C, cv2.THRESH_BINARY, 11, 2)  
  
# Find contours  
contours, _ = cv2.findContours(binary, cv2.RETR_EXTERNAL, cv2.CHAIN_APPROX_SIMPLE)  
  
recognized_text = []  
brands = ["kurkure", "loreal", "lux", "bhujia", "pears", "lays", "santoor", "colgate"]  
  
def word_tokenize(text):  
   tokens = re.findall(r'\b\w+\b', text)  
   print(f"Tokens: {tokens}")  # Debug print  
   return tokens  
  
def recognize_brands_and_dates(text, brands):  
   tokens = word_tokenize(text.lower())  # Convert text to lowercase  
   recognized_brands = [token for token in tokens if token.lower() in [brand.lower() for brand in brands]]  
   recognized_dates = extract_dates(text)  
   print(f"Recognized Brands: {recognized_brands}")  # Debug print  
   print(f"Recognized Dates: {recognized_dates}")  # Debug print  
   return recognized_brands, recognized_dates  
  
def extract_dates(text):  
   dates = []  
   for match in re.finditer(r'\b\d{1,2}[-/th|st|nd|rd\s]*[A-Za-z]{3,9}[-/\s]*\d{1,2}[-/\s]*\d{2,4}\b', text):  
      date_str = match.group()  
      try:  
        date = dateutil.parser.parse(date_str)  
        dates.append(date.strftime('%Y-%m-%d'))  # Normalize date format to YYYY-MM-DD  
      except ValueError:  
        pass  # Ignore invalid dates  
   return dates  
  
# Compute the Convex Hull for each contour and detect text  
for contour in contours:  
   hull = cv2.convexHull(contour)  
   x, y, w, h = cv2.boundingRect(hull)  
   cv2.drawContours(img, [hull], -1, (0, 255, 0), 2)  
   text_region = binary[y:y + h, x:x + w]  
    
   # Use pytesseract to detect text with custom configuration  
   custom_config = r'--oem 3 --psm 6 -c tessedit_char_whitelist=0123456789abcdefghijklmnopqrstuvwxyzABCDEFGHIJKLMNOPQRSTUVWXYZ'  
   text = pytesseract.image_to_string(text_region, config=custom_config)  
   if text.strip():  
      text = text.replace('\n', ' ')  
      recognized_text.append(text)  
      brands_found, dates_found = recognize_brands_and_dates(text, brands)  
      if brands_found or dates_found:  
        display_text = ', '.join(brands_found + dates_found)  
        cv2.putText(img, display_text, (x, y - 10), cv2.FONT_HERSHEY_SIMPLEX, 0.5, (0, 255, 0), 2)  
  
# Save recognized text to a file  
with open('/Users/abc/Downloads/recognized_text.txt', 'w') as file:  
   for line in recognized_text:  
      file.write(line + ' ')  
  
# Display the result  
print(recognized_text)  
cv2.imshow('Text Detection', img)  
cv2.waitKey(0)  
cv2.destroyAllWindows()
