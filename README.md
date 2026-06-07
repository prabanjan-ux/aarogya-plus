# Aarogya+

A smart prescription reader, medicine reminder, and symptom analyzer designed to help elderly patients manage their healthcare independently.

## Overview

Aarogya+ is an AI-powered healthcare assistant that combines advanced OCR, natural language processing, and reminder systems to help patients:

- **Digitize prescriptions** by scanning paper prescriptions and extracting structured medicine information
- **Understand symptoms** through text-based analysis with plain-language explanations
- **Never miss doses** with automated voice reminders and tracking
- **Find pharmacies** nearby when medicines need to be purchased
- **Access medical history** with a secure, searchable database

The application is specifically designed for elderly users who may struggle with medical jargon, complex prescription formats, or remembering medication schedules. It provides multi-language support and simple, accessible interfaces.

## Features

- **Prescription OCR & Extraction**
  - Scan paper prescriptions using Gemini Vision API
  - Extract structured medicine data (name, dosage, schedule, duration, food instructions)
  - Enrich with FDA drug label information for simplified explanations
  - Support for Indian brand names with generic name mapping

- **Symptom Analysis**
  - Text-based symptom input with LLM-powered analysis
  - Multiple condition predictions with probability scores
  - Plain-language explanations suitable for elderly users
  - Emergency warning system for serious symptoms
  - Triage advice with clear next steps

- **Medicine Reminders**
  - Automatic dose scheduling based on prescription data
  - Text-to-speech voice alerts at scheduled times
  - Mark doses as taken with simple interactions
  - Missed dose detection and caregiver alerts
  - Follow-up tracking with days remaining

- **Pharmacy Locator**
  - Find nearby pharmacies using OpenStreetMap/Overpass API
  - Real-time distance calculation (road distance preferred)
  - Opening hours and contact information
  - Direct navigation links
  - Fallback location support when GPS unavailable

- **Medical History**
  - Secure prescription storage in Supabase database
  - Duplicate detection to prevent redundant entries
  - Searchable history with full prescription details
  - Image storage for original prescriptions
  - Delete and manage prescriptions

- **Multi-language Support**
  - English, Hindi, Tamil, Telugu, Kannada, Spanish, French
  - Dynamic translation of UI elements and medical data
  - Language-specific symptom analysis
  - Brand name handling during translation

## Tech Stack

| Category | Technology | Purpose |
|----------|-----------|---------|
| **Frontend** | React 18.3.1 | UI framework |
| | Vite 5.4.0 | Build tool and dev server |
| | Lucide React 1.8.0 | Icon library |
| **Backend** | Python 3.10+ | Runtime environment |
| | Flask 3.0.0 | Web framework and API |
| | Flask-CORS 4.0.0 | Cross-origin resource sharing |
| **Database** | Supabase (PostgreSQL) | Prescription history and data persistence |
| **Authentication** | To Be Updated | User authentication (not yet implemented) |
| **AI/ML** | Ollama (qwen2.5:3b) | Local LLM for medicine extraction and symptom analysis |
| | Gemini 2.5 Flash Lite | Vision API for prescription OCR |
| | OpenAI SDK 1.30.0 | Unified client for Gemini and Ollama |
| **External APIs** | FDA Drug Label API | Medicine purpose and indication data |
| | RxNorm API | Drug name normalization |
| | Google Translate API | Multi-language translation |
| | Overpass API (OpenStreetMap) | Pharmacy location data |
| | OSRM API | Road distance calculation |
| | Nominatim API | Reverse geocoding |
| **Scheduling** | Schedule 1.2.1 | Medicine reminder scheduler |
| **Audio** | pyttsx3 2.90 | Text-to-speech for reminders |
| | openai-whisper 20231117 | Speech-to-text (optional) |
| **Utilities** | python-dotenv 1.0.0 | Environment variable management |
| | deep-translator 1.11.4 | Translation services |
| | requests 2.31.0 | HTTP client |

## Project Structure

```
aarogya-plus/
├── backend/
│   ├── main.py              # Flask API entry point and route definitions
│   ├── pipeline.py          # Prescription OCR pipeline (Gemini → LLM → FDA)
│   ├── symptom.py           # Symptom analysis using LLM
│   ├── reminder.py          # Medicine reminder scheduler with TTS
│   ├── locator.py           # Pharmacy finder using OpenStreetMap
│   ├── database.py          # Supabase database operations
│   ├── requirements.txt     # Python dependencies
│   ├── .env                 # Environment variables (gitignored)
│   ├── static/              # Production React build output
│   └── venv/                # Python virtual environment (gitignored)
│
├── frontend/
│   ├── src/
│   │   ├── main.jsx         # React entry point
│   │   ├── UI.jsx           # Main UI component with all screens
│   │   ├── components/      # Reusable UI components
│   │   ├── pages/           # Page-specific components
│   │   ├── services/        # API service layer
│   │   ├── utils/           # Utility functions
│   │   ├── constants/       # Application constants
│   │   ├── styles/          # CSS/styling
│   │   └── translations.js  # Multi-language translation data
│   ├── index.html           # HTML template
│   ├── package.json         # Node.js dependencies
│   ├── vite.config.js       # Vite configuration with API proxy
│   ├── dist/                # Production build output
│   └── node_modules/        # Node dependencies (gitignored)
│
├── .gitignore               # Git ignore rules
└── README.md                # This file
```

### Key Files Description

- **backend/main.py**: Flask application with all API endpoints, translation logic, and static file serving
- **backend/pipeline.py**: Three-step prescription processing: OCR with Gemini, structured extraction with Ollama, FDA enrichment
- **backend/symptom.py**: LLM-powered symptom analysis with elderly-friendly explanations
- **backend/reminder.py**: Background scheduler for medicine reminders with TTS alerts
- **backend/locator.py**: Pharmacy finder using OpenStreetMap Overpass API with distance calculations
- **backend/database.py**: Supabase integration for prescription history with duplicate detection
- **frontend/src/UI.jsx**: Single-file React application with all screens and state management
- **frontend/src/translations.js**: Comprehensive translation dictionary for supported languages

## Prerequisites

Before setting up Aarogya+, ensure you have the following installed:

| Software | Version | Purpose |
|----------|---------|---------|
| Python | 3.10 or higher | Backend runtime |
| Node.js | 18 or higher | Frontend build tool |
| Ollama | Latest | Local LLM runtime |
| Git | Latest | Version control |

### Required Accounts/API Keys

| Service | Required | Purpose |
|---------|----------|---------|
| Gemini API Key | Yes | Prescription OCR |
| Supabase URL & Key | Optional | Medical history persistence |
| Google Places API Key | No | Pharmacy locator (uses OpenStreetMap instead) |

### Install Ollama and Pull Model

```bash
# Install Ollama (if not already installed)
# Visit: https://ollama.ai/download

# Pull the required model
ollama pull qwen2.5:3b

# Verify installation
ollama list
```

## Installation

### 1. Clone the Repository

```bash
git clone https://github.com/your-username/aarogya-plus.git
cd aarogya-plus
```

### 2. Backend Setup

```bash
cd backend

# Create virtual environment
python -m venv venv

# Activate virtual environment
# On Windows:
venv\Scripts\activate
# On macOS/Linux:
source venv/bin/activate

# Install dependencies
pip install -r requirements.txt
```

### 3. Configure Environment Variables

Create a `.env` file in the `backend` directory:

```bash
cd backend
# Create .env file (copy the template below)
```

**backend/.env**:
```env
# Gemini API (Required for OCR)
GEMINI_API_KEY=your_gemini_api_key_here
GEMINI_MODEL=gemini-2.5-flash-lite

# Ollama Configuration
OLLAMA_BASE_URL=http://localhost:11434/v1
OLLAMA_MODEL=qwen2.5:3b

# Supabase (Optional - for medical history)
SUPABASE_URL=your_supabase_project_url
SUPABASE_KEY=your_supabase_anon_key

# Flask Configuration
PORT=5000
FLASK_DEBUG=true
```

Get your Gemini API key from: https://aistudio.google.com/app/apikey

### 4. Database Setup (Optional)

If using Supabase for medical history:

1. Create a Supabase project at https://supabase.com
2. Run the following SQL in the Supabase SQL Editor:

```sql
-- Create prescriptions table
CREATE TABLE prescriptions (
    id UUID DEFAULT gen_random_uuid() PRIMARY KEY,
    patient_name TEXT NOT NULL,
    patient_age TEXT,
    patient_gender TEXT,
    doctor_name TEXT NOT NULL,
    doctor_speciality TEXT,
    doctor_reg_id TEXT,
    diagnosis TEXT,
    description TEXT,
    image_url TEXT,
    image_data TEXT,
    scan_date TIMESTAMP WITH TIME ZONE DEFAULT NOW()
);

-- Create prescription_medicines table
CREATE TABLE prescription_medicines (
    id UUID DEFAULT gen_random_uuid() PRIMARY KEY,
    prescription_id UUID REFERENCES prescriptions(id) ON DELETE CASCADE,
    medicine_name TEXT NOT NULL,
    dosage TEXT,
    schedule TEXT,
    duration TEXT,
    food_instruction TEXT,
    purpose TEXT
);

-- Create indexes for better query performance
CREATE INDEX idx_prescriptions_scan_date ON prescriptions(scan_date DESC);
CREATE INDEX idx_prescription_medicines_prescription_id ON prescription_medicines(prescription_id);
```

### 5. Frontend Setup

```bash
cd frontend

# Install dependencies
npm install
```

### 6. Run Development Servers

**Terminal 1 - Backend:**
```bash
cd backend
venv\Scripts\activate  # Windows
# source venv/bin/activate  # macOS/Linux
python main.py
# Backend runs on http://localhost:5000
```

**Terminal 2 - Frontend:**
```bash
cd frontend
npm run dev
# Frontend runs on http://localhost:3000
```

The Vite dev server proxies all `/api/*` requests to the Flask backend.

## Environment Variables

| Variable Name | Description | Required | Default |
|---------------|-------------|----------|---------|
| `GEMINI_API_KEY` | Google Gemini API key for OCR | Yes | - |
| `GEMINI_MODEL` | Gemini model to use | No | `gemini-2.5-flash-lite` |
| `OLLAMA_BASE_URL` | Ollama server URL | No | `http://localhost:11434/v1` |
| `OLLAMA_MODEL` | Ollama model for LLM tasks | No | `qwen2.5:3b` |
| `SUPABASE_URL` | Supabase project URL | No | - |
| `SUPABASE_KEY` | Supabase anonymous key | No | - |
| `PORT` | Flask server port | No | `5000` |
| `FLASK_DEBUG` | Flask debug mode | No | `true` |

## Usage

### Application Workflow

1. **Home Screen**: Select from main features - Scan Prescription, Symptom Analysis, Reminders, History, or Pharmacy Locator

2. **Prescription Scan**:
   - Upload or capture a prescription image
   - System processes through OCR pipeline
   - Review extracted medicines and patient information
   - Save to medical history (optional)
   - Reminders are automatically scheduled

3. **Symptom Analysis**:
   - Enter symptoms in text form
   - Select language (supports multiple languages)
   - Receive condition predictions with plain-language explanations
   - Get triage advice and emergency warnings if applicable

4. **Medicine Reminders**:
   - View today's dose schedule
   - Mark doses as taken
   - Receive voice alerts at scheduled times
   - Track missed doses

5. **Follow-up Tracker**:
   - View days remaining for each medicine
   - See end dates for prescription courses
   - Plan refills accordingly

6. **Medical History**:
   - View all saved prescriptions
   - Access detailed prescription information
   - View original prescription images
   - Delete prescriptions if needed

7. **Pharmacy Locator**:
   - Allow location access or use fallback location
   - Search for nearby pharmacies
   - View distance, opening hours, and contact info
   - Get navigation directions

### Main Screens

- **Home**: Feature selection dashboard
- **Scan**: Prescription image upload and processing
- **Symptom**: Symptom input and analysis results
- **Reminders**: Today's medication schedule
- **Follow-up**: Days remaining tracker
- **History**: Medical prescription history
- **Locator**: Pharmacy finder with map integration

### Language Selection

Change the application language using the language selector in the UI. Supported languages:
- English (en)
- Hindi (hi)
- Tamil (ta)
- Telugu (te)
- Kannada (kn)
- Spanish (es)
- French (fr)

## API Documentation

### Base URL
- Development: `http://localhost:5000/api`
- Production: To Be Updated

### Endpoints

#### POST /api/analyze
Analyze symptom text using LLM.

**Request:**
```json
{
  "text": "I have a headache and fever",
  "lang": "en"
}
```

**Response:**
```json
{
  "symptoms": ["headache", "fever"],
  "conditions": [
    {
      "name": "Common Cold",
      "probability": 0.6,
      "simple_explanation": "A viral infection that causes runny nose, sore throat, and mild fever."
    }
  ],
  "advice": "Rest, stay hydrated, and monitor your symptoms.",
  "warning": ""
}
```

#### POST /api/scan
Scan prescription image and extract medicines.

**Request:**
```multipart
image: [file]
lang: en
```

**Response:**
```json
{
  "medicines": [
    {
      "medicine_name": "Paracetamol",
      "dosage": "500MG",
      "schedule": "1-0-1",
      "duration": "5 Days",
      "food_instruction": "after meal",
      "purpose": "Relieves pain and reduces fever"
    }
  ],
  "patient": {
    "patient_name": "John Doe",
    "patient_age": "45 Years",
    "patient_gender": "Male",
    "doctor_name": "Dr. Smith",
    "doctor_speciality": "MBBS - General Medicine",
    "diagnosis": "Viral Fever"
  },
  "message": "Reminders scheduled [OK]",
  "can_save": true
}
```

#### POST /api/save-prescription
Save the last scanned prescription to history.

**Request:**
```json
{}
```

**Response:**
```json
{
  "message": "Saved to medical history!",
  "prescription_id": "uuid-here"
}
```

#### GET /api/prescriptions
Get all saved prescriptions.

**Query Parameters:**
- `lang`: Language code (default: en)

**Response:**
```json
[
  {
    "id": "uuid",
    "patient_name": "John Doe",
    "doctor_name": "Dr. Smith",
    "diagnosis": "Viral Fever",
    "scan_date": "2024-01-15T10:30:00Z",
    "prescription_medicines": [...]
  }
]
```

#### GET /api/prescriptions/{id}
Get a specific prescription by ID.

**Query Parameters:**
- `lang`: Language code (default: en)

#### DELETE /api/prescriptions/{id}
Delete a prescription by ID.

#### GET /api/reminders
Get today's medication schedule.

**Query Parameters:**
- `lang`: Language code (default: en)

**Response:**
```json
[
  {
    "medicine_name": "Paracetamol",
    "time": "08:00",
    "slot": "Morning",
    "food_instruction": "after meal",
    "taken": false
  }
]
```

#### POST /api/taken
Mark a dose as taken.

**Request:**
```json
{
  "medicine_name": "Paracetamol",
  "time": "08:00"
}
```

**Response:**
```json
{
  "success": true
}
```

#### GET /api/followup
Get follow-up information for active medicines.

**Query Parameters:**
- `lang`: Language code (default: en)

**Response:**
```json
[
  {
    "medicine_name": "Paracetamol",
    "days_remaining": 3,
    "end_date": "2024-01-20"
  }
]
```

#### GET /api/locate-medicine
Find nearby pharmacies.

**Query Parameters:**
- `name`: Medicine name (optional)
- `lat`: Latitude
- `lng`: Longitude

**Response:**
```json
{
  "results": [
    {
      "id": 12345,
      "name": "Health Pharmacy",
      "lat": 13.129,
      "lon": 77.586,
      "distance_km": 1.2,
      "phone": "+91-1234567890",
      "is_open_now": true,
      "opening_hours_display": "Open 9:00 AM - 9:00 PM",
      "address": "Main Street, City",
      "maps_link": "https://www.openstreetmap.org/directions?..."
    }
  ],
  "count": 5
}
```

## Screenshots

<!-- Add screenshots of the application here -->

**Home Screen**
[To Be Updated]

**Prescription Scan**
[To Be Updated]

**Symptom Analysis**
[To Be Updated]

**Medicine Reminders**
[To Be Updated]

**Pharmacy Locator**
[To Be Updated]

## Deployment

### Production Build

1. **Build Frontend:**
```bash
cd frontend
npm run build
```

2. **Copy to Backend:**
```bash
# Windows
xcopy /E /I dist ..\backend\static

# macOS/Linux
cp -r dist/* ../backend/static/
```

3. **Configure Production Environment:**
```bash
cd backend
# Update .env for production
FLASK_DEBUG=false
PORT=5000
```

4. **Run Production Server:**
```bash
python main.py
```

The application will now be available at `http://localhost:5000` with the React build served from Flask.

### Deployment Options

**Option 1: Traditional VPS/Dedicated Server**
- Deploy backend with Gunicorn or uWSGI
- Configure Nginx as reverse proxy
- Set up SSL with Let's Encrypt

**Option 2: Cloud Platforms**
- Render, Railway, or Fly.io for backend
- Vercel or Netlify for frontend (with CORS configuration)
- Supabase for database

**Option 3: Docker**
```dockerfile
# Dockerfile example (To Be Updated)
FROM python:3.10-slim
# ... configuration
```

### Environment Variables for Production

Ensure all sensitive variables are set in your production environment:
- `GEMINI_API_KEY`
- `SUPABASE_URL` and `SUPABASE_KEY` (if using database)
- `FLASK_DEBUG=false`

## Contributing

We welcome contributions to Aarogya+! Here's how you can help:

1. **Fork the Repository**
   ```bash
   # Click "Fork" on GitHub, then clone your fork
   git clone https://github.com/your-username/aarogya-plus.git
   ```

2. **Create a Feature Branch**
   ```bash
   git checkout -b feature/your-feature-name
   ```

3. **Make Your Changes**
   - Follow the coding standards outlined below
   - Add tests if applicable
   - Update documentation as needed

4. **Commit Your Changes**
   ```bash
   git add .
   git commit -m "feat: add your feature description"
   ```

5. **Push to Your Fork**
   ```bash
   git push origin feature/your-feature-name
   ```

6. **Open a Pull Request**
   - Go to the original repository on GitHub
   - Click "New Pull Request"
   - Provide a clear description of your changes

### Contribution Guidelines

- Write clear, descriptive commit messages
- Add tests for new features
- Update documentation for API changes
- Ensure code passes linting (if configured)
- Be respectful in code reviews and discussions

## Coding Standards

### Python (Backend)
- Follow PEP 8 style guide
- Use type hints where appropriate
- Maximum line length: 100 characters
- Use descriptive variable and function names
- Add docstrings for all functions and classes

### JavaScript/React (Frontend)
- Use ESLint configuration (if set up)
- Use functional components with hooks
- Prefer const/let over var
- Use camelCase for variables and functions
- Use PascalCase for components

### Git Commit Messages

Follow conventional commits format:
- `feat:` New feature
- `fix:` Bug fix
- `docs:` Documentation changes
- `style:` Code style changes (formatting, etc.)
- `refactor:` Code refactoring
- `test:` Adding or updating tests
- `chore:` Maintenance tasks

Example:
```
feat: add multi-language support for symptom analysis
```

## Testing

### Running Tests

To Be Updated - Test framework and commands to be added.

### Test Coverage

To Be Updated - Coverage information to be added.

### Manual Testing Checklist

- [ ] Prescription scan with clear image
- [ ] Prescription scan with blurry image
- [ ] Symptom analysis in English
- [ ] Symptom analysis in other languages
- [ ] Medicine reminder scheduling
- [ ] Mark doses as taken
- [ ] Missed dose detection
- [ ] Pharmacy locator with GPS
- [ ] Pharmacy locator with fallback location
- [ ] Save prescription to history
- [ ] View prescription history
- [ ] Delete prescription
- [ ] Language switching

## Troubleshooting

### Ollama Not Responding

**Problem:** LLM calls fail with connection errors

**Solution:**
```bash
# Start Ollama server
ollama serve

# Check if model is downloaded
ollama list

# Pull model if missing
ollama pull qwen2.5:3b
```

### Gemini API Quota Exceeded

**Problem:** OCR fails with 429 error

**Solution:**
- Check your Gemini API quota at https://aistudio.google.com/app/apikey
- Wait for quota reset or upgrade your plan
- Consider implementing rate limiting

### CORS Errors in Browser

**Problem:** API calls blocked by CORS policy

**Solution:**
- Ensure you're using the Vite dev server (`npm run dev`) which proxies to Flask
- Check that Flask-CORS is properly configured in `main.py`
- Verify the proxy configuration in `vite.config.js`

### Supabase Connection Failed

**Problem:** Database operations fail

**Solution:**
- Verify `SUPABASE_URL` and `SUPABASE_KEY` in `.env`
- Check Supabase project status
- Ensure database tables are created (see Database Setup section)
- Check Supabase logs for specific error messages

### Prescription OCR Fails

**Problem:** No medicines detected from image

**Solution:**
- Ensure image is clear and well-lit
- Check that `GEMINI_API_KEY` is valid
- Verify Ollama is running and model is available
- Try with a different prescription image

### Translation Errors

**Problem:** Text not translating or showing errors

**Solution:**
- Check internet connection (Google Translate API requires internet)
- Verify language code is valid (en, hi, ta, te, kn, es, fr)
- Check Google Translate API quota
- Some brand names may not translate correctly (this is expected)

### Port Already in Use

**Problem:** Flask or Vite fails to start due to port conflict

**Solution:**
```bash
# Find process using the port (Windows)
netstat -ano | findstr :5000
taskkill /PID <PID> /F

# Find process using the port (macOS/Linux)
lsof -ti:5000
kill -9 <PID>

# Or use a different port in .env
PORT=5001
```

## Roadmap

### Planned Features

- [ ] User authentication and profiles
- [ ] Caregiver dashboard for monitoring multiple patients
- [ ] Voice input for symptom analysis (re-enable Whisper)
- [ ] PDF prescription support
- [ ] Medicine interaction checker
- [ ] Integration with local pharmacy delivery services
- [ ] Mobile app (React Native)
- [ ] Dark mode support
- [ ] Offline mode with local storage
- [ ] Export medical history as PDF
- [ ] Doctor portal for prescribing directly
- [ ] Integration with electronic health records (EHR)
- [ ] AI-powered dosage recommendations
- [ ] Drug side effect information
- [ ] Appointment scheduling integration

### Technical Improvements

- [ ] Comprehensive test suite
- [ ] CI/CD pipeline setup
- [ ] Docker containerization
- [ ] Kubernetes deployment manifests
- [ ] Performance optimization
- [ ] Error tracking and monitoring
- [ ] API rate limiting
- [ ] Enhanced security measures
- [ ] Database migration system
- [ ] Caching layer for API responses

## License

To Be Updated - Choose an appropriate open-source license (MIT, Apache 2.0, GPL, etc.)

## Authors and Acknowledgements

### Maintainers
- To Be Updated - Add maintainer information

### Contributors
- To Be Updated - Add contributor information

### Acknowledgements

- **Gemini API** by Google for vision OCR capabilities
- **Ollama** for local LLM infrastructure
- **FDA** for drug label data
- **OpenStreetMap** and **Overpass API** for pharmacy location data
- **Supabase** for database infrastructure
- **React** and **Vite** communities for excellent frontend tools
- **Flask** community for the backend framework

### Special Thanks

To Be Updated - Add any special acknowledgements

---

**Note:** This project is designed for educational and assistive purposes. Always consult with healthcare professionals for medical advice. This application does not replace professional medical diagnosis or treatment.
