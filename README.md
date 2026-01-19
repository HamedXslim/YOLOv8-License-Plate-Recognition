# YOLOv8 License Plate Recognition

A real-time license plate detection and recognition system using YOLOv8 for object detection and Tesseract OCR for text extraction. The system processes video footage, detects license plates, extracts text, and stores unique plates in a MySQL database with confidence scores.

## Features

- **Real-time Detection**: Processes video streams frame-by-frame using YOLOv8
- **OCR Integration**: Extracts text from detected plates using Tesseract OCR
- **Fuzzy Matching**: Prevents duplicate entries using intelligent similarity matching
- **Confidence Filtering**: Only processes detections above configurable thresholds
- **Database Storage**: Stores unique license plates with confidence scores in MySQL
- **Smart Pause System**: Prevents repeated processing of the same plate
- **Image Preprocessing**: Applies grayscale conversion, Gaussian blur, and OTSU thresholding for better OCR accuracy

## Prerequisites

- Python 3.8 or higher
- MySQL Server
- Tesseract OCR installed on your system

### Tesseract Installation

**Windows:**
- Download and install from: https://github.com/UB-Mannheim/tesseract/wiki
- Default installation path: `C:/Program Files/Tesseract-OCR/tesseract.exe`

**Linux:**
```bash
sudo apt-get install tesseract-ocr
```

**macOS:**
```bash
brew install tesseract
```

## Installation

1. **Clone the repository:**
```bash
git clone <repository-url>
cd computer_vision_for_license_plates
```

2. **Install Python dependencies:**
```bash
pip install ultralytics pytesseract opencv-python pandas mysql-connector-python rapidfuzz
```

3. **Set up MySQL database:**
```sql
CREATE DATABASE plaque_db;
```

4. **Configure database connection:**
Edit `main.py` to update your MySQL credentials:
```python
db = mysql.connector.connect(
    host="localhost",
    user="root",  
    password="root", 
    database="plaque_db" 
)
```

## Project Structure

```
computer_vision_for_license_plates/
├── main.py           # Main application script
├── best.pt           # Custom-trained YOLOv8 model for license plates
├── yolov8n.pt        # Base YOLOv8 nano model
└── README.md         # Project documentation
```

## Usage

1. **Place your video file** in the project directory and update the video name in `main.py`:
```python
video_name = "your_video.mp4"
```

2. **Run the application:**
```bash
python main.py
```

3. **Controls:**
- Press `q` to quit the application
- The system displays two windows:
  - **YOLO Detection**: Shows detected bounding boxes
  - **Processed Plate**: Shows preprocessed license plate images

## Configuration

You can adjust these parameters in `main.py`:

```python
CONFIDENCE_THRESHOLD = 0.7      # Minimum detection confidence (0-1)
DETECTION_THRESHOLD = 5         # Minimum detections before database insertion
SIMILARITY_THRESHOLD = 85       # Fuzzy match similarity percentage (0-100)
```

## How It Works

1. **Video Processing**: Reads video frame-by-frame
2. **Detection**: YOLOv8 detects license plate regions
3. **Preprocessing**: Converts to grayscale, applies Gaussian blur and OTSU thresholding
4. **OCR**: Tesseract extracts text from preprocessed images
5. **Validation**: Filters text based on length and character patterns
6. **Fuzzy Matching**: Compares with existing detections to avoid duplicates
7. **Database Storage**: Stores unique plates with best confidence score

## Database Schema

The application automatically creates a table for each video:

```sql
CREATE TABLE IF NOT EXISTS platesfor<video_name>video (
    id INT AUTO_INCREMENT PRIMARY KEY,
    number_plate VARCHAR(20) NOT NULL UNIQUE,
    confidence FLOAT NOT NULL
)
```

## Text Validation

The system filters detected text to ensure quality:
- Minimum length: 5 characters
- Excludes special characters: `!*'+()%/?~|,«—:[]§.°¥` and lowercase letters
- Trims leading/trailing spaces and hyphens

## Dependencies

- **ultralytics**: YOLOv8 model implementation
- **pytesseract**: Python wrapper for Tesseract OCR
- **opencv-python**: Computer vision and image processing
- **pandas**: Data manipulation
- **mysql-connector-python**: MySQL database connectivity
- **rapidfuzz**: Fast fuzzy string matching

## Troubleshooting

**Tesseract not found:**
- Ensure Tesseract is installed and the path in `main.py` is correct:
```python
pytesseract.pytesseract.tesseract_cmd = r'C:/Program Files/Tesseract-OCR/tesseract.exe'
```

**MySQL connection error:**
- Verify MySQL server is running
- Check database credentials in `main.py`
- Ensure `plaque_db` database exists

**Poor detection accuracy:**
- Adjust `CONFIDENCE_THRESHOLD` to filter low-confidence detections
- Consider retraining the model with your specific dataset
- Improve video quality or lighting conditions

## Model Information

- **best.pt**: Custom-trained YOLOv8 model for license plate detection
- **yolov8n.pt**: Base YOLOv8 nano model (smallest, fastest variant)

## License

[Add your license information here]

## Contributing

Contributions are welcome! Please feel free to submit a Pull Request.

## Author

[Add your name/contact information here]
