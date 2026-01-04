
# 📖 Face Recognition Django / Colab / Base64 Pipeline

![GitHub stars](https://img.shields.io/github/stars/alok-kumar8765/face_recogenize?style=social)
![GitHub forks](https://img.shields.io/github/forks/alok-kumar8765/face_recogenize?style=social)
![GitHub contributors](https://img.shields.io/github/contributors/alok-kumar8765/face_recogenize)
![GitHub last commit](https://img.shields.io/github/last-commit/alok-kumar8765/face_recogenize)
![GitHub language](https://img.shields.io/github/languages/top/alok-kumar8765/face_recogenize)

---

## 📌 Table of Contents

<details>
<summary>Click to expand</summary>

1. [Description](#description)
2. [Features](#features)
3. [Installation](#installation)
4. [Requirements](#requirements)
5. [Usage / Codes](#usage--codes)

   * [Code 1: CGI Web (Legacy)](#code-1-cgi-web-legacy)
   * [Code 2: Base64 Pipeline (Local / API-ready)](#code-2-base64-pipeline-local--api-ready)
   * [Code 3: Google Colab Upload](#code-3-google-colab-upload)
   * [Django Implementation](#django-implementation)
6. [Images / Example](#images--example)
7. [Known Issues / Fixes](#known-issues--fixes)
8. [Contributing](#contributing)
9. [License](#license)
10. [Tags](#tags)

</details>

---

## 📄 Description

This repo implements **face recognition** using Python’s [`face_recognition`](https://github.com/ageitgey/face_recognition) library.
It supports:

* Legacy **CGI web scripts**
* **Base64 image pipeline** (API-ready, local testing)
* **Google Colab-friendly uploads**
* **Full Django integration** (modern web framework)

It allows you to compare a **stored reference image** with a **current/uploaded image** and determine if the faces match.

---

## ⭐ Features

* Face verification between **stored reference image** and **live/current image**
* Supports **Base64 conversion** automatically
* Works in **Colab, local scripts, and Django web apps**
* Safety checks for **no-face images**
* Easy for beginners to test and extend
* Ready for **API integration**

---

## 💻 Installation

```bash
git clone https://github.com/alok-kumar8765/face_recognize.git
cd face_recognize
pip install -r requirements.txt
```

---

## 📦 Requirements

**requirements.txt**

```text
face_recognition>=1.3.0
dlib>=19.22
numpy>=1.19
Pillow>=9.0
opencv-python>=4.5
django>=4.2
```

> Tip: On **Windows**, install via Anaconda or use Colab to avoid `dlib` compilation issues.
> On Colab:

```python
!pip install face_recognition dlib numpy Pillow opencv-python django
```

---

## Usage / Codes

### 🔹 Code 1: CGI Web (Legacy) ❌

<details>
<summary>Click to expand</summary>

**Problem:**

* CGI requires a **web server** (Apache/Nginx with `cgi-bin`).
* Cannot run in Colab or direct Python (`FieldStorage` expects HTTP POST).

**Fix / Recommendation:**

* Use **Code 2** (Base64 Pipeline) or **Code 3** (Colab Upload) for local testing.
* For production, **convert CGI logic to Django or Flask API** (see below).

</details>

---

### 🔹 Code 2: Base64 Pipeline (Local / API-ready)

<details>
<summary>Click to expand</summary>

**Purpose:** Convert images to Base64 internally and compare faces.

**Steps:**

```python
stored_image_path = "/content/image.png"
current_image_path = "/content/image_0.jpg"
```

* Converts images → Base64 → temporary files
* Loads images → encodes faces → compares faces → prints result

> Works locally, in Colab, or as part of an API backend.

</details>

---

### 🔹 Code 3: Google Colab Upload

<details>
<summary>Click to expand</summary>

**Purpose:** Easy testing in Colab with manual uploads:

```python
from google.colab import files
import face_recognition

# Upload stored and current image
stored_upload = files.upload()
current_upload = files.upload()

stored_image_path = list(stored_upload.keys())[0]
current_image_path = list(current_upload.keys())[0]

# Load and encode faces
stored_image = face_recognition.load_image_file(stored_image_path)
current_image = face_recognition.load_image_file(current_image_path)
```

* Encodes faces → compares → prints **FACE MATCHED / NOT MATCHED**

</details>

---

### 🔹 Django Implementation (Recommended)

<details>
<summary>Click to expand</summary>

**1️⃣ Create Django project**

```bash
django-admin startproject face_recognition_project
cd face_recognition_project
python manage.py startapp face_app
```

**2️⃣ Settings**

```python
# face_recognition_project/settings.py
INSTALLED_APPS = [
    ...
    'face_app',
]
MEDIA_URL = '/media/'
MEDIA_ROOT = BASE_DIR / 'media'
```

**3️⃣ Forms**

```python
# face_app/forms.py
from django import forms

class FaceUploadForm(forms.Form):
    email = forms.CharField(max_length=100)
    current_image = forms.ImageField()
```

**4️⃣ Views**

```python
# face_app/views.py
from django.shortcuts import render
from .forms import FaceUploadForm
import face_recognition
from django.http import HttpResponse
import os

STUDENTS_FOLDER = "students"

def face_verify(request):
    if request.method == "POST":
        form = FaceUploadForm(request.POST, request.FILES)
        if form.is_valid():
            email = form.cleaned_data['email']
            uploaded_file = request.FILES['current_image']

            uploaded_path = f"/tmp/{uploaded_file.name}"
            with open(uploaded_path, "wb") as f:
                for chunk in uploaded_file.chunks():
                    f.write(chunk)

            uploaded_image = face_recognition.load_image_file(uploaded_path)
            ref_image_path = os.path.join(STUDENTS_FOLDER, f"{email}.jpg")
            if not os.path.exists(ref_image_path):
                return HttpResponse("❌ Stored image not found")
            stored_image = face_recognition.load_image_file(ref_image_path)

            uploaded_faces = face_recognition.face_encodings(uploaded_image)
            stored_faces = face_recognition.face_encodings(stored_image)

            if not uploaded_faces:
                return HttpResponse("❌ No face found in uploaded image")
            if not stored_faces:
                return HttpResponse("❌ No face found in stored image")

            match = face_recognition.compare_faces([stored_faces[0]], uploaded_faces[0])
            if match[0]:
                return HttpResponse(f"✅ Welcome {email}! Face matched.")
            else:
                return HttpResponse("❌ Face not recognized.")
    else:
        form = FaceUploadForm()
    return render(request, "face_app/upload.html", {"form": form})
```

**5️⃣ URLs**

```python
# face_app/urls.py
from django.urls import path
from . import views

urlpatterns = [
    path("verify/", views.face_verify, name="face_verify"),
]

# face_recognition_project/urls.py
from django.contrib import admin
from django.urls import path, include

urlpatterns = [
    path('admin/', admin.site.urls),
    path('', include('face_app.urls')),
]
```

**6️⃣ Templates**

```html
<!-- face_app/templates/face_app/upload.html -->
<html>
<body>
<h2>Face Verification</h2>
<form method="post" enctype="multipart/form-data">
    {% csrf_token %}
    {{ form.as_p }}
    <button type="submit">Verify Face</button>
</form>
</body>
</html>
```

**7️⃣ Run Server**

```bash
python manage.py runserver
```

Open browser: [http://127.0.0.1:8000/verify/](http://127.0.0.1:8000/verify/)

Upload **current image** and input **email**.
Result will appear on page.

</details>

---

## 🖼 Images / Example

* Put images in repo `images/` folder

```markdown
**Stored Reference**
![Stored Image](images/image.png)

**Current Image**
![Current Image](images/image_0.jpg)
```

* Recommended for README display and testing

---

## ⚠ Known Issues / Fixes

<details>
<summary>Click to expand</summary>

1. **CGI cannot run in Colab** → use Code 2/3 or Django
2. **No face detected** → image must have **clear, frontal face**
3. **dlib installation errors** → use Colab or Anaconda
4. **Parentheses in filename `(1)`** → rename file to avoid errors
5. **Python <3.7** → upgrade Python

</details>

---

## 🤝 Contributing

* ⭐ Star the repo to show support
* Fork & submit PRs with fixes or features
* Report issues for improvements
* Enhance Django API / Base64 pipeline / Colab workflow

---

## 📄 License

[MIT License](LICENSE)

---

Folder Structure :

```
face_recognition_project/
├─ face_app/
│  ├─ templates/face_app/upload.html
│  ├─ forms.py
│  ├─ views.py
│  ├─ urls.py
├─ students/
│  ├─ student1.jpg
├─ images/
│  ├─ image.png
│  ├─ image_0.jpg
├─ requirements.txt
├─ README.md
```
---
