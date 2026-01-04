
# 🔥 Face Recognition Pipeline

![GitHub stars](https://img.shields.io/github/stars/alok-kumar8765/face_recogenize?style=social)
![GitHub forks](https://img.shields.io/github/forks/alok-kumar8765/face_recogenize?style=social)
![GitHub contributors](https://img.shields.io/github/contributors/alok-kumar8765/face_recogenize)
![GitHub last commit](https://img.shields.io/github/last-commit/alok-kumar8765/face_recogenize)
![GitHub language](https://img.shields.io/github/languages/top/alok-kumar8765/face_recogenize)

---

## 📖 Table of Contents

<details>
<summary>Click to Expand</summary>

1. [Description](#description)  
2. [Features](#features)  
3. [Usage](#usage)  
   - [Code 1: Web Server (CGI)](#code-1-web-server-cgi)  
   - [Code 2: Base64 Pipeline](#code-2-base64-pipeline)  
   - [Code 3: Google Colab](#code-3-google-colab)  
4. [Installation](#installation)  
5. [Requirements](#requirements)  
6. [Known Issues / Fixes](#known-issues--fixes)  
7. [Contributing](#contributing)  
8. [License](#license)  
9. [Tags](#tags)  

</details>

---

## 📄 Description

This repository provides a **complete face recognition pipeline** using Python's [`face_recognition`](https://github.com/ageitgey/face_recognition) library. It supports **three modes of usage**:

- **Code 1:** Traditional web server CGI setup (accepts base64 image via browser form)  
- **Code 2:** Backend-ready Base64 pipeline (convert images to Base64 internally, API-ready)  
- **Code 3:** Google Colab-friendly version for testing with manual uploads  

The repo is designed to be **modular, beginner-friendly, and easily extensible** for APIs, mobile apps, and production environments.

---

## ⭐ Features

- Face verification between **stored reference image** and **live/current image**
- Works in **web servers, Colab, and local scripts**
- Handles **base64 conversion automatically** for future API integration
- Safe checks for **no-face images**
- Easy for beginners: **just provide image paths or upload files in Colab**
- Modular structure for **future Flask/FastAPI integration**

---

## 🛠 Usage

### 🔹 Code 1: Web Server (CGI)

<details>
<summary>Click to Expand</summary>

**Purpose:** Accepts base64 image from a browser form and compares it with stored student images.

**Usage:**

1. Host on a CGI-enabled web server (Apache/Nginx).  
2. Form must send:
   - `current_image` → Base64 string of uploaded image  
   - `email` → student ID corresponding to stored image  
3. Place reference images in `students/` folder named as `{email}.jpg`.  

**Example flow:**



Browser → Form → Base64 → Server → Compare → JS alert

</details>

---

### 🔹 Code 2: Base64 Pipeline

<details>
<summary>Click to Expand</summary>

**Purpose:** Converts images to base64 internally for **API-ready logic**. Users only need to provide image paths.

**Usage:**

1. Change paths:
```python
stored_image_path = "/content/image.png"
current_image_path = "/content/image_0.jpg"
````

2. Run the script; it automatically:

   * Converts images → Base64
   * Converts Base64 → temporary images
   * Compares faces using `face_recognition`
3. Prints **Face Matched / Not Matched** result

**Benefits:**

* No manual Base64 needed
* API-ready for Flask/FastAPI
* Works locally or in Colab

</details>

---

### 🔹 Code 3: Google Colab Friendly

<details>
<summary>Click to Expand</summary>

**Purpose:** Allows testing in Google Colab using **manual uploads**.

**Usage:**

1. Open Colab notebook.
2. Run the code; it will prompt:

   * Upload **STORED (reference) image**
   * Upload **CURRENT (image to verify)**
3. Automatically compares faces and prints result.

**Best For:** Beginners, testing, and quick prototyping.

</details>

---

## 💻 Installation

```bash
git clone https://github.com/alok-kumar8765/face_recogenize.git
cd face_recogenize
pip install -r requirements.txt
```

---

## 📦 Requirements

* Python ≥ 3.7
* face_recognition
* dlib
* numpy

Optional (Colab):

```bash
!pip install face_recognition
```

---

## ⚠ Known Issues / Fixes

<details>
<summary>Click to Expand</summary>

1. **`face_recognition` installation fails**

   * On Windows, dlib may fail. Solution: Use **Anaconda** or Colab.
2. **No face detected**

   * Ensure image contains **one clear, front-facing face**.
   * Lighting and resolution matter.
3. **Base64 pipeline errors**

   * Make sure the image path exists and is readable.
   * File names should not contain parentheses (e.g., `(1)` may cause issues in some environments).
4. **Older Python / Library versions**

   * Ensure `face_recognition>=1.3.0`, `dlib>=19.22`, `numpy>=1.19`

</details>

---

## 🤝 Contributing

We welcome contributions! 💡

* **Star the repo** to show support ⭐
* **Fork and make changes**
* Submit **pull requests** with improvements
* Report **issues or bugs**

> Motivation: Help others easily implement face recognition in their apps 🚀

---

## 📄 License

[MIT License](LICENSE)

---

## 💡 Motivation / Notes

This repo is designed to help developers and beginners:

* Quickly test face recognition in Colab or local machines
* Integrate face recognition in web applications
* Learn base64 pipelines for image handling
* Prepare code for future REST APIs

---

**Enjoy and contribute! 🌟**

```
"Hit Star ⭐, contribute code, and make face recognition accessible for everyone!"
```


---

### ✅ Features included in this README:

- Table of contents with collapsible sections  
- Badges: stars, forks, contributors, last commit, language  
- Clear description, features, and usage for all 3 codes  
- Installation, requirements  
- Known issues and fixes  
- Contribution encouragement  
- Tags for discoverability  
- Motivation for stars/contributions  

---

