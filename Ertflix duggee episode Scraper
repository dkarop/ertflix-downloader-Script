import re
from selenium import webdriver
from selenium.webdriver.common.by import By
from selenium.webdriver.support.ui import WebDriverWait
from selenium.webdriver.support import expected_conditions as EC
import time
import subprocess

# Set up the WebDriver
browser = webdriver.Chrome()  # You can use Chrome or Firefox here
url = "https://www.ertflix.gr/series/ser.232898-geia-sou-ntagki"  # Replace with the actual URL
browser.get(url)

try:
    # Handle the "Reject cookies" button if it exists
    try:
        cookies_button = WebDriverWait(browser, 5).until(
            EC.presence_of_element_located((By.XPATH, "//button[@class='CookiePolicyInfo__accept___bozEi']"))
        )
        cookies_button.click()
    except:
        pass  # Continue even if the button doesn't exist

    # Find and click the "Show more episodes" button if it exists
    try:
        button = WebDriverWait(browser, 10).until(
            EC.presence_of_element_located((By.XPATH, "//button[@data-test='next-episodes-button']"))
        )
        button.click()

        # Wait for the content to load (you might need to adjust the time)
        time.sleep(5)

        # Extract image URLs from the page source code using regular expressions
        page_source = browser.page_source
        image_urls = re.findall(r'https:\/\/files2.app.ertflix.gr\/files\/paidika\/hey-duggee\/s03\/.*?\.jpg', page_source)
        # Convert the list of image URLs to a set to get unique values
        unique_image_urls = set(image_urls)

        base_url_pattern = "https://mediaserve.ert.gr/bpk-vod/vodext/default/hey-duggee-s03-ep%s-%s/hey-duggee-s03-ep%s-%s/index.mpd"

        new_urls = []

        # Loop through the unique image URLs
        for url in unique_image_urls:
            # Extract episode number and title from the original image URL
            episode_number = re.search(r'hey-duggee-s03-ep(\d+)-', url).group(1)
            episode_title = re.search(r'hey-duggee-s03-ep\d+-(.*)\.jpg', url).group(1).upper()
            
            # Construct the new URL using the extracted values
            new_url = base_url_pattern % (episode_number, episode_title, episode_number, episode_title)
            new_urls.append(new_url)

        # Print the new URLs
        for url in new_urls:
            print(url)
            # Loop through the new_urls list and call the executable
        for url in new_urls:
            subprocess.call(["\\dash-mpd-cli.exe", url, "--quality", "best"])


        # You can add additional actions here if needed
    except:
        pass  # Continue even if the button doesn't exist

    # Keep the browser window open
    input("Press Enter to exit...")  # This waits for user input

finally:
    # Close the browser
    browser.quit()
