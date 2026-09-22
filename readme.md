# Remote Sensing Water Body Detection

A full-stack remote sensing application that detects water bodies from **Sentinel-2 satellite imagery** using **Deep Learning** (TensorFlow) with an **NDWI fallback**. Users can draw a Region of Interest (ROI) on an interactive map, retrieve satellite imagery, generate a water segmentation mask, and calculate the total water-covered area.

> **Tech Stack:** React • FastAPI • TensorFlow • Sentinel Hub • Rasterio • PostgreSQL • Vercel

---

## Features

- Interactive map for selecting an ROI
- Downloads Sentinel-2 imagery using Sentinel Hub
- Deep learning-based water segmentation
- Automatic NDWI fallback when the trained model is unavailable
- Water mask overlay generation
- Water area calculation in square meters, hectares, and square kilometers
- REST API built with FastAPI
- PostgreSQL storage for ROI data
- Vercel deployment support

---

## Project Architecture

```text
React Frontend
      │
      ▼
 FastAPI Backend
      │
      ▼
 Sentinel Hub API
      │
      ▼
 RGB + NDWI Images
      │
      ▼
 TensorFlow Model
      │
      ▼
 Water Mask + Area Calculation
      │
      ▼
 Results Displayed on Map
```

### Workflow

1. User draws a polygon on the map.
2. Frontend sends the polygon as GeoJSON.
3. FastAPI validates the request.
4. Sentinel Hub downloads Sentinel-2 imagery.
5. Image is preprocessed.
6. TensorFlow predicts the water mask.
7. If the model is unavailable, NDWI thresholding is used.
8. Water area is calculated.
9. Overlay and statistics are returned.

---

## Project Structure

```text
Remote-Sensing-Change-Detection/
│
├── backend/
│   ├── app/
│   │   ├── api/
│   │   ├── services/
│   │   ├── db/
│   │   ├── core/
│   │   └── main.py
│   ├── requirements.txt
│   └── vercel.json
│
├── frontend/
│   ├── src/
│   ├── public/
│   ├── package.json
│   └── vite.config.js
│
└── README.md
```

---

## Backend Overview

The backend is built with **FastAPI** and exposes REST APIs for health monitoring, ROI storage, and water segmentation.

### Key Files

| File | Purpose |
|------|---------|
| `main.py` | Creates FastAPI application |
| `segmentation.py` | Prediction pipeline |
| `roi.py` | Stores GeoJSON polygons |
| `health.py` | Health check endpoint |
| `config.py` | Loads environment variables |

### Services

#### `sentinel.py`

Responsible for:

- Connecting to Sentinel Hub
- Downloading Sentinel-2 imagery
- Creating RGB and NDWI outputs

#### `inference.py`

Responsible for:

- Loading TensorFlow model
- Image preprocessing
- Water mask prediction
- Overlay generation

### Sentinel-2 Bands Used

| Band | Description |
|------|-------------|
| B02 | Blue |
| B03 | Green |
| B04 | Red |
| B08 | Near Infrared |

---

## Frontend Overview

Built using:

- React
- Vite
- Tailwind CSS
- React Leaflet

### Main Components

| Component | Purpose |
|-----------|---------|
| `MapComponent` | Interactive map |
| `Sidebar` | Prediction controls |
| `Topbar` | Header |
| `ErrorBoundary` | Error handling |

---

## NDWI Fallback

If the trained TensorFlow model is unavailable, the application automatically switches to NDWI.

**Formula**

NDWI = (Green − 0.5 × NIR) / (Green + NIR)

This allows water detection even without the trained model.

---

## Water Area Calculation

Sentinel-2 has a spatial resolution of **10 meters**.

Each pixel represents:

10 m × 10 m = **100 m²**

Water area is computed as:

Water Area = Water Pixels × 100 m²

Results are reported in:

- Square meters
- Hectares
- Square kilometers

---

## API Endpoints

### Health Check

```http
GET /api/v1/health
```

**Response**

```json
{
  "status": "healthy"
}
```

### Store ROI

```http
POST /api/v1/roi
```

**Example Request**

```json
{
  "geometry": {
    "type": "Polygon",
    "coordinates": [...]
  }
}
```

### Predict Water Mask

```http
POST /api/v1/predict
```

Returns:

- Water mask
- Overlay image
- Water area
- Acquisition date

---

## Installation

### Clone Repository

```bash
git clone https://github.com/parthu1029/Remote-Sensing-Change-Detection.git
cd Remote-Sensing-Change-Detection
```

### Backend Setup

```bash
cd backend
pip install -r requirements.txt
uvicorn app.main:app --reload
```

### Frontend Setup

```bash
cd frontend
npm install
npm run dev
```

---

## Environment Variables

Create a `.env` file inside the `backend` folder.

```env
SENTINEL_HUB_CLIENT_ID=your_client_id
SENTINEL_HUB_CLIENT_SECRET=your_client_secret
MODEL_PATH=models/water_segmentation_model.h5
DATABASE_URL=postgresql://user:password@localhost/dbname
```

---

## Deployment

### Frontend

Deploy using:

- Vercel
- Netlify

### Backend

Configured for Vercel using:

- FastAPI
- Mangum
- Serverless Functions

---

## Future Improvements

- U-Net++ support
- DeepLabV3+ integration
- Temporal change detection
- Cloud probability filtering
- Multi-date comparison
- Export results as GeoJSON

---

## Tech Stack

| Category | Technology |
|----------|------------|
| Frontend | React, Vite, Tailwind CSS |
| Backend | FastAPI |
| AI | TensorFlow |
| Remote Sensing | Sentinel Hub |
| Geospatial | Rasterio, Shapely |
| Database | PostgreSQL |
| Deployment | Vercel |

---

## License

This project was developed for educational and research purposes.
