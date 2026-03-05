# 📋 **COMPLETE APPLICATION DOCUMENTATION - AIR QUALITY PREDICTION SYSTEM**

---

## 🎯 **APPLICATION OVERVIEW**

**Name:** ADPSI - Air Quality Prediction & Monitoring System for Delhi NCR

**Purpose:** A comprehensive air quality monitoring and prediction platform that uses Machine Learning to forecast AQI levels, identify pollution sources, and provide actionable recommendations for citizens and policymakers.

---

## 🏗️ **COMPLETE TECHNOLOGY STACK (PIN-TO-PIN)**

### **1. FRONTEND TECHNOLOGIES**

| Technology | Version | Purpose |
|-----------|---------|---------|
| **React** | 19.0.0 | Core UI framework |
| **React Router DOM** | 7.5.1 | Client-side routing |
| **Tailwind CSS** | 3.4.17 | Utility-first styling |
| **Radix UI** | Multiple | Accessible UI components (Accordion, Dialog, Dropdown, Tabs, etc.) |
| **Leaflet** | 1.9.4 | Interactive maps for heatmap visualization |
| **Leaflet.heat** | 0.2.0 | Heatmap overlay for pollution hotspots |
| **Recharts** | 3.6.0 | Data visualization (charts, graphs) |
| **Axios** | 1.8.4 | HTTP client for API calls |
| **Lucide React** | 0.507.0 | Icon library |
| **React Hook Form** | 7.56.2 | Form management |
| **Zod** | 3.24.4 | Schema validation |
| **Date-fns** | 4.1.0 | Date manipulation |
| **Sonner** | 2.0.3 | Toast notifications |
| **CRACO** | 7.1.0 | Create React App configuration override |

**Build Tool:** React Scripts 5.0.1 (with CRACO customization)

---

### **2. BACKEND TECHNOLOGIES**

| Technology | Version | Purpose |
|-----------|---------|---------|
| **FastAPI** | 0.110.1 | Modern async web framework |
| **Uvicorn** | 0.25.0 | ASGI server |
| **Python** | 3.x | Backend language |
| **Pydantic** | 2.12.5 | Data validation & settings |
| **SQLAlchemy** | 2.0.36 | ORM for database |
| **Motor** | 3.3.1 | Async database driver |
| **aiohttp** | 3.13.3 | Async HTTP client |
| **aiosmtplib** | 5.1.0 | Async email sending |
| **bcrypt** | 4.1.3 | Password hashing |
| **python-jose** | 3.5.0 | JWT token handling |
| **python-dotenv** | 1.2.1 | Environment variable management |
| **passlib** | 1.7.4 | Password utilities |

---

### **3. MACHINE LEARNING STACK**

| Technology | Version | Purpose |
|-----------|---------|---------|
| **XGBoost** | 2.1.3 | AQI forecasting ensemble model |
| **scikit-learn** | 1.8.0 | Source attribution (Random Forest) |
| **NumPy** | 2.4.2 | Numerical computing |
| **Pandas** | 3.0.0 | Data manipulation |
| **joblib** | 1.5.3 | Model serialization |
| **scipy** | 1.17.0 | Scientific computing |

---

### **4. DATABASE**

**Type:** SQLite (Development) / PostgreSQL-compatible (Production)

**ORM:** SQLAlchemy 2.0.36

**Tables:**
1. **admin_users** - Admin authentication
2. **pollution_reports** - Citizen pollution reports
3. **aqi_prediction_logs** - ML prediction history
4. **source_attribution_logs** - Source prediction history

**Database File:** `/app/backend/aqi_data.db`

---

### **5. EXTERNAL APIs & INTEGRATIONS**

| Service | Purpose | Configuration |
|---------|---------|---------------|
| **WAQI API** | Real-time AQI data | Token: `WAQI_API_TOKEN` |
| **Google Gemini AI** | AI-powered recommendations | Key: `GEMINI_API_KEY` |
| **SMTP Email** | Report confirmations & updates | Configured via environment |

---

## 🧠 **MACHINE LEARNING MODELS - DETAILED**

### **MODEL 1: AQI FORECASTING (XGBoost Ensemble)**

**Architecture:**
- **Type:** XGBoost Gradient Boosting Ensemble
- **Ensemble Size:** 5 boosters with different random seeds (42, 53, 64, 75, 86)
- **Prediction Method:** Mean of ensemble predictions
- **Confidence Calculation:** Based on ensemble agreement (std deviation)

**Input Features (28 features):**
```
Pollutants (6):     PM2.5, PM10, NO2, SO2, CO, O3
Time (5):           hour, day, month, day_of_week, is_weekend
Cyclic (4):         month_sin, month_cos, hour_sin, hour_cos
Location (2):       lat, lon
AQI Memory (6):     AQI_t-1, AQI_t-6, AQI_t-12, AQI_t-24, 
                    rolling_mean_24h, rolling_mean_72h
Derived (2):        pm_ratio (PM10/PM2.5), traffic_ratio (NO2/CO)
```

**Output:**
- AQI prediction at 24 hours
- AQI prediction at 48 hours
- AQI prediction at 72 hours
- Trend (increasing/decreasing/stable)
- Confidence level (0-100%)

**Model Files:**
```
/app/backend/ml_models/model1/
├── artifact_wrapper.pkl          (Feature definitions)
├── booster_seed42.json           (Booster 1)
├── booster_seed53.json           (Booster 2)
├── booster_seed64.json           (Booster 3)
├── booster_seed75.json           (Booster 4)
├── booster_seed86.json           (Booster 5)
└── ensemble_metadata.json        (Metadata)
```

**Training Data:** CPCB & WAQI historical data (2019-2025)

---

### **MODEL 2: SOURCE ATTRIBUTION (Random Forest Regressor)**

**Architecture:**
- **Type:** Multi-output Random Forest Regression
- **Outputs:** 5 pollution sources with percentage contributions
- **Feature Engineering:** Pollutant ratios, temporal features

**Input Features (10 features):**
```
Pollutants (6):     PM2.5, PM10, NO2, SO2, CO, O3
Derived (2):        pm_ratio (PM10/PM2.5), no2_co_ratio (NO2/CO)
Time (2):           month, hour
```

**Output:**
- Traffic contribution (%)
- Industry contribution (%)
- Construction contribution (%)
- Stubble burning contribution (%)
- Other sources contribution (%)
- Dominant source identification
- Confidence level

**Model File:**
```
/app/backend/ml_models/model2/
└── pollution_source_regression_model.pkl
```

**Training Data:** Labeled CPCB data (2015-2024) with source attribution

---

## 📂 **COMPLETE DIRECTORY STRUCTURE**

```
/app/adpsi-main/
│
├── backend/                          # Backend Application
│   ├── server.py                     # Main FastAPI application (1203 lines)
│   ├── database.py                   # SQLAlchemy ORM models
│   ├── requirements.txt              # Python dependencies (131 packages)
│   ├── .env                          # Environment variables
│   ├── aqi_data.db                   # SQLite database
│   │
│   ├── ml_models/                    # Machine Learning Models
│   │   ├── aqi_forecaster.py         # XGBoost ensemble loader
│   │   ├── source_attribution.py     # Random Forest loader
│   │   ├── MODEL_SETUP.md            # Setup guide
│   │   ├── model1/                   # AQI Forecasting Model
│   │   │   └── [XGBoost model files]
│   │   └── model2/                   # Source Attribution Model
│   │       └── [Random Forest model file]
│   │
│   └── utils/                        # Utility Functions
│       └── email_service.py          # Email notification service
│
├── frontend/                         # React Frontend
│   ├── public/
│   │   └── index.html                # HTML template
│   ├── src/
│   │   ├── App.js                    # Main app component
│   │   ├── index.js                  # Entry point
│   │   ├── index.css                 # Global styles
│   │   ├── pages/                    # Page components
│   │   ├── components/               # Reusable components
│   │   ├── hooks/                    # Custom React hooks
│   │   └── lib/                      # Utility libraries
│   ├── package.json                  # Node dependencies
│   ├── tailwind.config.js            # Tailwind configuration
│   ├── postcss.config.js             # PostCSS config
│   ├── craco.config.js               # CRACO config
│   └── components.json               # Shadcn/ui config
│
├── tests/                            # Test Suite
│   └── __init__.py
│
├── test_reports/                     # Test Reports
│   └── pytest/
│
├── memory/                           # Application Memory
│
├── test_result.md                    # Testing Data & History
├── README.md                         # Project documentation
├── ML_INTEGRATION_GUIDE.md           # ML setup guide
├── SEVERITY_COLORS_UPDATE.md         # Design guidelines
└── design_guidelines.json            # UI/UX standards
```

---

## 🔄 **SYSTEM ARCHITECTURE & DATA FLOW**

```
┌─────────────────────────────────────────────────────────────────┐
│                         USER INTERFACE                          │
│                    (React Frontend - Port 3000)                 │
│  - Dashboard, Prediction Page, Reports, Admin Panel, Maps       │
└────────────────────────┬────────────────────────────────────────┘
                         │ HTTP/REST API Calls
                         │ (Axios)
                         ▼
┌─────────────────────────────────────────────────────────────────┐
│                      FASTAPI BACKEND                            │
│                      (Port 8001 - /api)                         │
│                                                                 │
│  API Endpoints:                                                 │
│  ├── /api/auth/login                 (Admin auth)              │
│  ├── /api/aqi/current                (Real-time AQI)           │
│  ├── /api/aqi/forecast               (ML predictions)          │
│  ├── /api/aqi/sources                (Source attribution)      │
│  ├── /api/aqi/heatmap                (Map data)                │
│  ├── /api/reports                    (Citizen reports)         │
│  ├── /api/recommendations            (AI recommendations)      │
│  ├── /api/alerts                     (Forecast alerts)         │
│  ├── /api/health-advisory            (Health guidance)         │
│  ├── /api/seasonal-outlook           (Seasonal patterns)       │
│  ├── /api/routes/safe                (Safe route calculation)  │
│  ├── /api/policy/impact              (Policy simulator)        │
│  ├── /api/insights/summary           (AI insights)             │
│  └── /api/model/transparency         (Model info)              │
│                                                                 │
└───┬─────────────┬──────────────┬─────────────┬─────────────────┘
    │             │              │             │
    │             │              │             │
    ▼             ▼              ▼             ▼
┌────────┐  ┌──────────┐  ┌──────────┐  ┌─────────────┐
│ SQLite │  │ ML MODEL │  │ ML MODEL │  │  EXTERNAL   │
│   DB   │  │    #1    │  │    #2    │  │    APIs     │
│        │  │          │  │          │  │             │
│ Tables:│  │ XGBoost  │  │  Random  │  │- WAQI API   │
│ admin  │  │ Ensemble │  │  Forest  │  │- Gemini AI  │
│reports │  │          │  │          │  │- SMTP Email │
│aqi_logs│  │ (5 seed) │  │ Multi-   │  │             │
│source_ │  │          │  │ Output   │  │             │
│logs    │  │ 24h,48h, │  │          │  │             │
│        │  │ 72h AQI  │  │ Traffic, │  │             │
│        │  │          │  │ Industry,│  │             │
│        │  │          │  │ Construc-│  │             │
│        │  │          │  │ tion...  │  │             │
└────────┘  └──────────┘  └──────────┘  └─────────────┘
```

---

## 🐳 **DOCKER CONTAINERIZATION & DEPLOYMENT STRATEGY**

### **PHASE 1: BACKEND ML MODEL TO DOCKER IMAGE**

#### **Step 1: Create Dockerfile for Backend**

```dockerfile
# Dockerfile
FROM python:3.11-slim

WORKDIR /app

# Install system dependencies
RUN apt-get update && apt-get install -y \
    gcc \
    g++ \
    && rm -rf /var/lib/apt/lists/*

# Copy requirements
COPY backend/requirements.txt .

# Install Python dependencies
RUN pip install --no-cache-dir -r requirements.txt

# Copy backend code
COPY backend/ ./backend/

# Copy ML models
COPY backend/ml_models/ ./backend/ml_models/

# Expose port
EXPOSE 8001

# Set environment variables
ENV PYTHONUNBUFFERED=1
ENV WAQI_API_TOKEN=${WAQI_API_TOKEN}
ENV GEMINI_API_KEY=${GEMINI_API_KEY}

# Run the application
CMD ["uvicorn", "backend.server:app", "--host", "0.0.0.0", "--port", "8001"]
```

#### **Step 2: Create .dockerignore**

```
__pycache__/
*.py[cod]
*$py.class
*.so
.Python
.env
.venv
env/
venv/
*.log
*.db-journal
.DS_Store
node_modules/
```

#### **Step 3: Build Docker Image**

```bash
# Navigate to project root
cd /app/adpsi-main

# Build Docker image
docker build -t adpsi-backend:latest .

# Tag for Docker Hub
docker tag adpsi-backend:latest yourusername/adpsi-backend:v1.0
docker tag adpsi-backend:latest yourusername/adpsi-backend:latest
```

#### **Step 4: Test Docker Image Locally**

```bash
# Run container locally
docker run -d \
  -p 8001:8001 \
  -e WAQI_API_TOKEN="your_token" \
  -e GEMINI_API_KEY="your_key" \
  --name adpsi-backend \
  yourusername/adpsi-backend:latest

# Check logs
docker logs adpsi-backend

# Test API
curl http://localhost:8001/api/aqi/current
```

---

### **PHASE 2: PUSH TO DOCKER HUB**

#### **Step 1: Login to Docker Hub**

```bash
# Login to Docker Hub
docker login

# Enter your Docker Hub credentials
# Username: yourusername
# Password: your_password
```

#### **Step 2: Push Image to Docker Hub**

```bash
# Push both tags
docker push yourusername/adpsi-backend:v1.0
docker push yourusername/adpsi-backend:latest

# Verify on Docker Hub
# Visit: https://hub.docker.com/r/yourusername/adpsi-backend
```

#### **Step 3: Make Repository Public (Optional)**

```
1. Go to Docker Hub
2. Navigate to your repository
3. Settings → Make Public
4. Now anyone can pull: docker pull yourusername/adpsi-backend:latest
```

---

### **PHASE 3: DEPLOY TO RAILWAY CLOUD**

#### **Method 1: Deploy from Docker Hub**

```yaml
# railway.json (Railway configuration)
{
  "$schema": "https://railway.app/railway.schema.json",
  "build": {
    "builder": "DOCKERFILE",
    "dockerfilePath": "Dockerfile"
  },
  "deploy": {
    "startCommand": "uvicorn backend.server:app --host 0.0.0.0 --port $PORT",
    "restartPolicyType": "ON_FAILURE",
    "restartPolicyMaxRetries": 10
  }
}
```

**Steps:**

1. **Create Railway Account**
   - Visit: https://railway.app
   - Sign up with GitHub

2. **Create New Project**
   - Click "New Project"
   - Select "Deploy from Docker Hub"
   - Enter: `yourusername/adpsi-backend:latest`

3. **Configure Environment Variables**
   ```
   WAQI_API_TOKEN=your_waqi_token
   GEMINI_API_KEY=your_gemini_key
   ADMIN_EMAIL=admin@delhiair.gov.in
   ADMIN_PASSWORD=your_secure_password
   DB_NAME=aqi_database
   CORS_ORIGINS=*
   ```

4. **Add SQLite Volume (for persistence)**
   - Go to project settings
   - Add Volume: `/app/backend` → to persist `aqi_data.db`

5. **Deploy**
   - Railway automatically deploys
   - You'll get a URL like: `https://adpsi-backend-production.up.railway.app`

---

#### **Method 2: Deploy from GitHub Repository**

1. **Push Code to GitHub**
   ```bash
   git init
   git add .
   git commit -m "Initial commit"
   git remote add origin https://github.com/yourusername/adpsi-backend.git
   git push -u origin main
   ```

2. **Connect Railway to GitHub**
   - New Project → Deploy from GitHub repo
   - Select repository: `adpsi-backend`
   - Railway will detect Dockerfile automatically

3. **Configure & Deploy**
   - Same environment variables as Method 1
   - Railway builds and deploys automatically

---

### **PHASE 4: CONTINUOUS DEPLOYMENT**

#### **Automatic Updates from Docker Hub**

```bash
# On your local machine, after making changes:

# 1. Rebuild image
docker build -t yourusername/adpsi-backend:latest .

# 2. Push to Docker Hub
docker push yourusername/adpsi-backend:latest

# 3. Trigger Railway Redeploy
# Railway will automatically detect new image and redeploy
# Or manually trigger from Railway dashboard
```

#### **CI/CD with GitHub Actions**

Create `.github/workflows/deploy.yml`:

```yaml
name: Build and Deploy to Railway

on:
  push:
    branches: [ main ]

jobs:
  build:
    runs-on: ubuntu-latest
    
    steps:
    - uses: actions/checkout@v3
    
    - name: Login to Docker Hub
      uses: docker/login-action@v2
      with:
        username: ${{ secrets.DOCKER_USERNAME }}
        password: ${{ secrets.DOCKER_PASSWORD }}
    
    - name: Build and push
      uses: docker/build-push-action@v4
      with:
        context: .
        push: true
        tags: |
          yourusername/adpsi-backend:latest
          yourusername/adpsi-backend:${{ github.sha }}
    
    - name: Deploy to Railway
      run: |
        curl -X POST ${{ secrets.RAILWAY_WEBHOOK_URL }}
```

---

## 🔐 **ENVIRONMENT VARIABLES REQUIRED**

### **Backend (.env)**

```env
# Database (auto-managed by SQLite)
SQLITE_DB_URL=sqlite:////app/backend/aqi_data.db
DB_NAME=aqi_database

# Admin Credentials
ADMIN_EMAIL=admin@delhiair.gov.in
ADMIN_PASSWORD=DelhiAir@2026

# External APIs
WAQI_API_TOKEN=your_waqi_api_token_here
GEMINI_API_KEY=your_gemini_api_key_here

# CORS
CORS_ORIGINS=http://localhost:3000,https://yourdomain.com

# ML Model Paths (optional - defaults provided)
ML_MODEL1_DIR=/app/backend/ml_models/model1
ML_MODEL2_DIR=/app/backend/ml_models/model2

# Email Configuration (optional)
SMTP_HOST=smtp.gmail.com
SMTP_PORT=587
SMTP_USER=your_email@gmail.com
SMTP_PASSWORD=your_app_password
```

### **Frontend (.env)**

```env
REACT_APP_BACKEND_URL=http://localhost:8001
# Or for production:
# REACT_APP_BACKEND_URL=https://adpsi-backend-production.up.railway.app
```

---

## 🚀 **DEPLOYMENT SUMMARY**

### **Complete Deployment Flow:**

```
1. LOCAL DEVELOPMENT
   ├── Backend: Python FastAPI + ML Models
   ├── Frontend: React App
   └── Database: SQLite

2. DOCKERIZATION
   ├── Create Dockerfile
   ├── Build Image: docker build -t adpsi-backend .
   └── Test Locally: docker run -p 8001:8001 adpsi-backend

3. DOCKER HUB
   ├── docker login
   ├── docker tag adpsi-backend yourusername/adpsi-backend:latest
   └── docker push yourusername/adpsi-backend:latest

4. RAILWAY CLOUD
   ├── Create Railway Project
   ├── Deploy from Docker Hub image
   ├── Configure Environment Variables
   ├── Add Volume for SQLite persistence
   └── Get Production URL

5. FRONTEND DEPLOYMENT (Separate)
   ├── Update REACT_APP_BACKEND_URL to Railway URL
   ├── Build: npm run build
   └── Deploy to: Vercel / Netlify / Railway
```

---

## 📊 **DATABASE SCHEMA (SQLite with PostgreSQL Compatibility)**

### **Table 1: admin_users**
```sql
CREATE TABLE admin_users (
    id INTEGER PRIMARY KEY,
    email VARCHAR(255) UNIQUE NOT NULL,
    hashed_password VARCHAR(255) NOT NULL,
    role VARCHAR(50) DEFAULT 'admin',
    created_at DATETIME DEFAULT CURRENT_TIMESTAMP,
    is_active BOOLEAN DEFAULT TRUE
);
```

### **Table 2: pollution_reports**
```sql
CREATE TABLE pollution_reports (
    id INTEGER PRIMARY KEY,
    report_id VARCHAR(255) UNIQUE NOT NULL,
    name VARCHAR(255) NOT NULL,
    mobile VARCHAR(20) NOT NULL,
    email VARCHAR(255) NOT NULL,
    location VARCHAR(500) NOT NULL,
    latitude FLOAT,
    longitude FLOAT,
    severity INTEGER NOT NULL,
    description TEXT,
    image_url VARCHAR(1000),
    status VARCHAR(50) DEFAULT 'pending',
    created_at DATETIME DEFAULT CURRENT_TIMESTAMP,
    updated_at DATETIME DEFAULT CURRENT_TIMESTAMP
);
```

### **Table 3: aqi_prediction_logs**
```sql
CREATE TABLE aqi_prediction_logs (
    id INTEGER PRIMARY KEY,
    current_aqi FLOAT NOT NULL,
    aqi_24h FLOAT,
    aqi_48h FLOAT NOT NULL,
    aqi_72h FLOAT NOT NULL,
    trend VARCHAR(50),
    confidence FLOAT,
    model_version VARCHAR(100),
    prediction_type VARCHAR(50),
    created_at DATETIME DEFAULT CURRENT_TIMESTAMP
);
```

### **Table 4: source_attribution_logs**
```sql
CREATE TABLE source_attribution_logs (
    id INTEGER PRIMARY KEY,
    traffic FLOAT NOT NULL,
    industry FLOAT NOT NULL,
    construction FLOAT NOT NULL,
    stubble_burning FLOAT NOT NULL,
    other FLOAT NOT NULL,
    dominant_source VARCHAR(100),
    confidence FLOAT,
    model_version VARCHAR(100),
    prediction_type VARCHAR(50),
    created_at DATETIME DEFAULT CURRENT_TIMESTAMP
);
```

---

## 🎨 **KEY FEATURES OF THE APPLICATION**

1. **Real-time AQI Dashboard** - Live air quality data for Delhi NCR
2. **ML-Powered Forecasting** - 24h, 48h, 72h AQI predictions
3. **Source Attribution** - Identify pollution sources (traffic, industry, construction, etc.)
4. **Interactive Heatmap** - Visualize pollution hotspots on map
5. **Citizen Reporting** - Allow citizens to report pollution incidents
6. **Admin Dash
