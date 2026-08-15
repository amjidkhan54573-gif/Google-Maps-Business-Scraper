# Google-map-

from selenium import webdriver
from selenium.webdriver.common.by import By
from selenium.webdriver.support.ui import WebDriverWait
from selenium.webdriver.support import expected_conditions as EC
from selenium.common.exceptions import TimeoutException
import csv

# ==================================================
# 1. CHROME BROWSER OPEN
# ==================================================

driver = webdriver.Chrome()


# ==================================================
# 2. GOOGLE MAPS OPEN
# ==================================================

driver.get("https://www.google.com/maps")


# ==================================================
# 3. SEARCH BOX LOCATE KARO
# ==================================================

search_box = WebDriverWait(driver, 20).until(
    EC.presence_of_element_located((By.TAG_NAME, "input"))
)


# ==================================================
# 4. SEARCH TEXT ENTER KARO
# ==================================================

search_box.send_keys("Schools in Plano")


# ==================================================
# 5. SEARCH BUTTON LOCATE KARO
# ==================================================

search_button = WebDriverWait(driver, 20).until(
    EC.element_to_be_clickable((By.CSS_SELECTOR, 'button[aria-label="Search"]'))
)


# ==================================================
# 6. SEARCH BUTTON CLICK KARO
# ==================================================

search_button.click()


# ==================================================
# 7. SCHOOL LISTINGS LOAD HONE KA WAIT
# ==================================================

try:

    WebDriverWait(driver, 20).until(
        EC.presence_of_element_located((By.CSS_SELECTOR, 'div[role="feed"]'))
    )

    print("Results feed loaded.")


except TimeoutException:

    print("Results feed load nahi hua.")
    driver.quit()
    exit()


# ==================================================
# 8. SCHOOL LISTINGS KA WAIT
# ==================================================

try:

    WebDriverWait(driver, 20).until(
        lambda d: len(d.find_elements(By.CSS_SELECTOR, "a.hfpxzc")) > 0
    )

    print("School listings loaded.")


except TimeoutException:

    print("School listings load nahi hui.")
    print("Google Maps ne results nahi diye.")

    driver.quit()
    exit()


# ==================================================
# 9. RESULTS FEED LOCATE KARO
# ==================================================

feed = driver.find_element(By.CSS_SELECTOR, 'div[role="feed"]')


# ==================================================
# 10. SCHOOL LIST
# ==================================================

schools = []


# ==================================================
# 11. DUPLICATE URL ROKNE KE LIYE SET
# ==================================================

processed_urls = set()


# ==================================================
# 12. INFINITE SCROLLING
# ==================================================

while True:

    # Current listings
    links = driver.find_elements(By.CSS_SELECTOR, "a.hfpxzc")

    # ----------------------------------------------
    # CURRENT LISTINGS COLLECT KARO
    # ----------------------------------------------

    for link in links:

        name = link.get_attribute("aria-label")

        url = link.get_attribute("href")

        if name and url and url not in processed_urls:

            schools.append({"name": name, "url": url})

            processed_urls.add(url)

            print("Collected:", name)

    # Current count
    old_count = len(links)

    print("Before scroll:", old_count)

    # ----------------------------------------------
    # FEED SCROLL
    # ----------------------------------------------

    driver.execute_script("arguments[0].scrollTop = arguments[0].scrollHeight", feed)

    # ----------------------------------------------
    # NEW LISTINGS KA WAIT
    # ----------------------------------------------

    try:

        WebDriverWait(driver, 10).until(
            lambda d: len(d.find_elements(By.CSS_SELECTOR, "a.hfpxzc")) > old_count
        )

    except TimeoutException:

        print("No new listings found.")

        print("Scrolling complete.")

        break


# ==================================================
# 13. TOTAL SCHOOLS
# ==================================================

print("\nTotal schools collected:", len(schools))


# ==================================================
# 14. CSV FILE
# ==================================================

csv_file = "schools.csv"


fieldnames = [
    "name",
    "google_maps_url",
    "rating",
    "reviews",
    "address",
    "phone",
    "website",
]


# CSV header
with open(csv_file, "w", newline="", encoding="utf-8-sig") as file:

    writer = csv.DictWriter(file, fieldnames=fieldnames)

    writer.writeheader()


# ==================================================
# 15. ALL SCHOOLS
# ==================================================

all_schools = schools


# ==================================================
# 16. SCHOOL DETAILS SCRAPING
# ==================================================

for school in all_schools:

    print("\n====================================")
    print("Opening:", school["name"])
    print("====================================")

    try:

        # ------------------------------------------
        # SCHOOL PAGE OPEN
        # ------------------------------------------

        driver.get(school["url"])

        # ------------------------------------------
        # SCHOOL NAME LOAD HONE KA WAIT
        # ------------------------------------------

        try:

            WebDriverWait(driver, 15).until(
                EC.presence_of_element_located((By.CSS_SELECTOR, "h1"))
            )

            print("Page loaded.")

        except TimeoutException:

            print("Page loading timeout.")

        # ------------------------------------------
        # EXTRA WAIT FOR DETAILS
        # ------------------------------------------

        try:

            WebDriverWait(driver, 5).until(
                lambda d: len(d.find_elements(By.CSS_SELECTOR, "button[data-item-id]"))
                > 0
            )

        except TimeoutException:

            print("Some details did not load.")

        # ==========================================
        # NAME
        # ==========================================

        try:

            name_element = driver.find_element(By.CSS_SELECTOR, "h1")

            name = name_element.text

        except:

            name = school["name"]

        print("Name:", name)

        # ==========================================
        # RATING
        # ==========================================

        try:

            rating_element = driver.find_element(
                By.CSS_SELECTOR, 'span[role="img"][aria-label*="stars"]'
            )

            rating = rating_element.get_attribute("aria-label")

            print("Rating:", rating)

        except:

            rating = None

            print("Rating: Not found")

        # ==========================================
        # REVIEWS
        # ==========================================

        try:

            reviews_element = driver.find_element(
                By.CSS_SELECTOR, 'span[role="img"][aria-label*="reviews"]'
            )

            reviews = reviews_element.get_attribute("aria-label")

            print("Reviews:", reviews)

        except:

            reviews = None

            print("Reviews: Not found")

        # ==========================================
        # ADDRESS
        # ==========================================

        try:

            address_element = driver.find_element(
                By.CSS_SELECTOR, 'button[data-item-id="address"]'
            )

            address = address_element.text

            print("Address:", address)

        except:

            address = None

            print("Address: Not found")

        # ==========================================
        # PHONE
        # ==========================================

        try:

            phone_element = driver.find_element(
                By.CSS_SELECTOR, 'button[data-item-id^="phone:tel:"]'
            )

            phone = phone_element.text

            print("Phone:", phone)

        except:

            phone = None

            print("Phone: Not found")

        # ==========================================
        # WEBSITE
        # ==========================================

        try:

            website_element = driver.find_element(
                By.CSS_SELECTOR, 'a[data-item-id="authority"]'
            )

            website = website_element.get_attribute("href")

            print("Website:", website)

        except:

            website = None

            print("Website: Not found")

        # ==========================================
        # SCHOOL DATA
        # ==========================================

        school_data = {
            "name": name,
            "google_maps_url": school["url"],
            "rating": rating,
            "reviews": reviews,
            "address": address,
            "phone": phone,
            "website": website,
        }

        # ==========================================
        # CSV SAVE
        # ==========================================

        with open(csv_file, "a", newline="", encoding="utf-8-sig") as file:

            writer = csv.DictWriter(file, fieldnames=fieldnames)

            writer.writerow(school_data)

        print("Saved to CSV.")

    except Exception as e:

        print("Unexpected error:", e)

        print("Moving to next school...")

        continue


# ==================================================
# 17. FINISH
# ==================================================

print("\n====================================")

print("SCRAPING COMPLETE")

print("Total schools:", len(schools))

print("CSV file:", csv_file)

print("====================================")


# ==================================================
# 18. BROWSER CLOSE
# ==================================================

driver.quit()
