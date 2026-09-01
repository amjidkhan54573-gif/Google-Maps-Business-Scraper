# 🗺️ Google Maps Business Scraper

> **A Selenium-powered Python scraper for collecting Google Maps business data and exporting it to CSV.**

[![Python](https://img.shields.io/badge/Python-3.x-blue?style=for-the-badge&logo=python&logoColor=white)](https://www.python.org/)
[![Selenium](https://img.shields.io/badge/Selenium-WebDriver-green?style=for-the-badge&logo=selenium&logoColor=white)](https://www.selenium.dev/)
[![CSV](https://img.shields.io/badge/Output-CSV-orange?style=for-the-badge)](https://docs.python.org/3/library/csv.html)

## 🚀 Overview

This project automates Google Maps with **Selenium WebDriver**, searches for a target category/location, scrolls through dynamically loaded results, opens individual listings, extracts useful business information, and saves the collected data into a CSV file.

The project was built as a practical exercise in **dynamic web scraping, browser automation, explicit waits, scrolling, duplicate handling, error handling, and structured data export**.

## ✨ Features

- 🔎 Search Google Maps automatically
- 🌐 Handle dynamically loaded results with Selenium
- 📜 Scroll through the results feed to discover more listings
- ♻️ Prevent duplicate listings using URLs
- 🏢 Extract business names and Google Maps URLs
- ⭐ Extract ratings and review information when available
- 📍 Extract addresses
- 📞 Extract phone numbers
- 🔗 Extract business websites
- 💾 Export results to CSV
- ⏳ Use explicit waits for dynamic page elements
- 🛡️ Continue scraping when individual listings fail

## 📊 Data Collected

| Field | Description |
|---|---|
| `name` | Business name |
| `google_maps_url` | Google Maps listing URL |
| `rating` | Business rating, when available |
| `reviews` | Review information, when available |
| `address` | Business address |
| `phone` | Phone number, when available |
| `website` | Business website, when available |

## 🧰 Technologies

- **Python**
- **Selenium WebDriver**
- **Chrome WebDriver**
- **CSV module**
- **Explicit waits & Expected Conditions**
- **CSS selectors**

## ⚙️ How It Works

```text
Start
  ↓
Open Google Maps
  ↓
Enter search query
  ↓
Wait for results
  ↓
Collect listing URLs
  ↓
Scroll results feed
  ↓
Collect new listings
  ↓
Open each listing
  ↓
Extract business details
  ↓
Save data to CSV
  ↓
Finish
```

## ▶️ Run the Project

### 1. Clone the repository

```bash
git clone https://github.com/amjidkhan54573-gif/Google-map-.git
cd Google-map-
```

### 2. Install Selenium

```bash
pip install selenium
```

### 3. Run the scraper

```bash
python main.py
```

> **Note:** If your Python file has a different name, replace `main.py` with your filename.

## 📁 Output

The scraper creates a CSV file containing the collected business data:

```text
schools.csv
```

Example structure:

```text
name,google_maps_url,rating,reviews,address,phone,website
Business Name,https://...,4.6,120 reviews,Address,...,https://...
```

## 🧠 What I Practiced

This project helped me strengthen practical Selenium skills including:

- `find_element()` and `find_elements()`
- CSS selectors
- `WebDriverWait`
- Expected Conditions
- JavaScript-based scrolling
- Dynamic content handling
- Retry/wait logic
- Duplicate prevention with Python `set`
- Exception handling
- CSV writing with `csv.DictWriter`
- Navigating between multiple pages

## 🔧 Possible Improvements

Future versions can add:

- Configurable search queries
- Configurable maximum results
- Better pagination/scroll detection
- More robust selectors
- Logging instead of console-only output
- Retry mechanisms for transient failures
- Additional business fields
- Data cleaning and validation
- Optional pandas-based export

## ⚠️ Responsible Use

This project is intended for **learning, research, and legitimate automation use cases**. Always respect the target website's terms, applicable laws, and reasonable request limits when collecting data.

## 👨‍💻 About Me

I'm **Amjid Khan**, a Python developer focused on **web scraping, Selenium automation, and practical data extraction projects**.

I learn by building real projects, debugging real problems, and continuously improving my Python skills.

### 🤝 Let's Connect

- 💼 [LinkedIn](https://linkedin.com/in/amjid-khan-69231a397/)
- 📧 `contact.amjid.freelancer@gmail.com`

---

⭐ **If you find this project useful, consider giving it a star!**

**Built with Python 🐍 + Selenium 🌐**