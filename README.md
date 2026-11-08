# Presence Monitoring – face-recognition attendance web app

A Streamlit web app that records the presence of operators in a smart factory using face recognition.
An operator takes a picture with the camera; the app recognises the face, logs the time, and keeps an attendance history.

## Features

| Tab | What it does |
|---|---|
| **Operator Validation** | Takes a photo, detects faces, matches them against the database and logs each recognised operator (or "Unknown") with a timestamp |
| **View Operator History** | Shows the attendance log, newest first, searchable by check ID |
| **Add to Database** | Registers a new operator from a photo and a name (stores the 128-value face encoding) |

## Tech stack

Python · Streamlit · `face_recognition` (dlib) · OpenCV · pandas · NumPy

## Project structure

| File | Purpose |
|---|---|
| `app.py` | Main app: photo-based check-in with `st.camera_input` (works in the browser, Codespaces, Streamlit Cloud) |
| `app_webcam.py` | Variant for a local machine: continuous webcam stream with a 1-minute cooldown per operator |
| `settings.py` | Paths, parameters, face-encoding helpers and attendance logging |
| `requirements.txt` | Python dependencies |
| `.devcontainer/` | GitHub Codespaces setup that installs the dependencies and starts `app.py` |

## Run it

**In GitHub Codespaces:** *Code → Codespaces → Create codespace on main*. The app starts automatically on port 8501.

**Locally:**
```bash
pip install -r requirements.txt
streamlit run app.py          # browser camera
streamlit run app_webcam.py   # live webcam (local machine only)
```

`face_recognition` needs `dlib`; `requirements.txt` installs the prebuilt `dlib-bin` wheel.

## How recognition works

1. Each registered face is stored as a 128-value encoding in `operators_database/operators_db.csv`.
2. A new photo is encoded the same way and compared with every stored encoding.
3. Each distance is converted to a similarity score; the best match above 0.5 is accepted, otherwise the face is logged as "Unknown".
4. The check is appended to `operators_history/operators_history.csv` with a unique ID and a timestamp.

## Privacy

Face encodings and photos are biometric personal data. The `operators_database/` and `operators_history/` folders (encodings and attendance log) are created at run time and are excluded from Git: never commit real people's faces or attendance records.
