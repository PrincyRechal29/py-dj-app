# py-dj-app

A stateless Django web app with a generative ambient jazz music experience.
No database required — ready to containerize and deploy to EKS.

---

## Prerequisites

- Python 3.12+
- pip

Verify you have them:

```bash
python --version
pip --version
```

---

## Local Setup & Run

### 1. Clone the repo

```bash
git clone <your-repo-url>
cd py-dj-app
```

### 2. (Optional) Create a virtual environment

```bash
python -m venv venv
```

Activate it:

- **Windows**
  ```bash
  venv\Scripts\activate
  ```
- **Mac / Linux**
  ```bash
  source venv/bin/activate
  ```

### 3. Install dependencies

```bash
pip install -r requirements.txt
```

### 4. Start the development server

```bash
python manage.py runserver
```

### 5. Open in browser

```
http://127.0.0.1:8000
```

You should see the NOCTURNE music player. Press the gold button (or `Space`) to play.

### 6. Test the health endpoint

```
http://127.0.0.1:8000/health/
```

Expected response:

```json
{"status": "ok"}
```

---

## Project Structure

```
py-dj-app/
├── manage.py
├── requirements.txt
├── Dockerfile
├── .dockerignore
├── mysite/
│   ├── settings.py      # env-var driven, no database
│   ├── urls.py          # routes: / and /health/
│   └── wsgi.py
└── home/
    ├── views.py         # renders the music page
    ├── urls.py
    └── templates/
        └── home/
            └── index.html   # full music player (Web Audio API)
```

---

## Environment Variables

| Variable        | Default                          | Description                        |
|-----------------|----------------------------------|------------------------------------|
| `SECRET_KEY`    | insecure default                 | Django secret key — change in prod |
| `DEBUG`         | `false`                          | Set to `true` for local dev logs   |
| `ALLOWED_HOSTS` | `*`                              | Comma-separated list of hostnames  |

Set them before running:

- **Windows**
  ```bash
  set SECRET_KEY=your-secret-key
  set DEBUG=true
  python manage.py runserver
  ```
- **Mac / Linux**
  ```bash
  export SECRET_KEY=your-secret-key
  export DEBUG=true
  python manage.py runserver
  ```

---

## Docker (local test before EKS)

### Build the image

```bash
docker build -t py-dj-app .
```

### Run the container

```bash
docker run -p 8000:8000 -e SECRET_KEY=your-secret-key -e DEBUG=false py-dj-app
```

### Open in browser

```
http://localhost:8000
```

---

## Running Checks

Verify Django configuration is valid (no server needed):

```bash
python manage.py check
```

Expected output:

```
System check identified no issues (0 silenced).
```
