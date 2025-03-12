# SIMPLE-WEBSITE-SCRAPER
A software application (or "bot") that automatically extracts and stores data from websites
Step 2: Update & Install Python (If Not Installed)

Check if Python is installed:

python3 --version

If not installed, install it:

sudo apt update && sudo apt install python3 -y
![Screenshot_2025-03-12_04_24_47](https://github.com/user-attachments/assets/f0b0e4f6-e7de-4bdb-8771-ba7e94a6d934)

Check if pip is installed:

pip3 --version

If not installed, install it:

sudo apt install python3-pip -y

Step 3: Create a New Project Directory

Navigate to your preferred location and create a project folder:

mkdir web_scraper
cd web_scraper

Step 4: Install Required Python Libraries

Install the necessary libraries using pip:

pip3 install requests beautifulsoup4 pandas

✅ requests → Fetches web pages
✅ beautifulsoup4 → Extracts data from HTML
✅ pandas → Saves data to a CSV file
Step 5: Create the Python Script

Create a new Python file:

nano scraper.py

This will open the Nano editor. Copy and paste the following code inside:

import requests
from bs4 import BeautifulSoup
import pandas as pd
![Screenshot_2025-03-12_04_26_48](https://github.com/user-attachments/assets/c8d0b679-9837-4a45-8bc4-9a9a88f488ce)

# Step 1: Define the Target URL
URL = "https://news.ycombinator.com/"

# Step 2: Send a Request to the Website
headers = {"User-Agent": "Mozilla/5.0"}  # Helps prevent blocking
response = requests.get(URL, headers=headers)

# Step 3: Parse the Webpage
if response.status_code == 200:
    soup = BeautifulSoup(response.text, "html.parser")
else:
    print("Failed to retrieve the webpage.")
    exit()

# Step 4: Extract Data (Article Titles)
titles = soup.find_all("span", class_="titleline")
news_data = []
for title in titles:
    headline = title.find("a")
    if headline:
        news_data.append({"Title": headline.text, "Link": headline["href"]})

# Step 5: Save Data to CSV
df = pd.DataFrame(news_data)
df.to_csv("news_headlines.csv", index=False)

# Step 6: Print the Extracted Data
for idx, news in enumerate(news_data, 1):
    print(f"{idx}. {news['Title']} ({news['Link']})")

After pasting the code, save the file by pressing:

    Ctrl + X (Exit Nano)
    Y (Yes to save)
    Enter (Confirm file name)

Step 6: Run Your Web Scraper

In the terminal, run:

python3 scraper.py
![Screenshot_2025-03-12_04_27_41](https://github.com/user-attachments/assets/7091dea5-c7ef-49b2-adde-8bf6bfe3fc95)

✅ This will scrape news headlines from Hacker News.
✅ The results will be printed in the terminal.
Step 7: Check the CSV File

The scraper saves data in news_headlines.csv.
To check the file contents in the terminal:

cat news_headlines.csv

Or open it using Nano:

nano news_headlines.csv

To open it in LibreOffice (GUI):

libreoffice --calc news_headlines.csv

Step 8: Move & Share the CSV File

You can move the file to another location:

mv news_headlines.csv ~/Documents/

To copy it to a USB drive (assuming it's mounted at /media/usb):

cp news_headlines.csv /media/usb/

Step 9: Automate the Scraper (Optional)

If you want the scraper to run automatically every day, use cron jobs:

crontab -e

Add this line at the bottom to run it daily at 9 AM:

0 9 * * * /usr/bin/python3 /home/yourusername/web_scraper/scraper.py

Replace yourusername with your actual Linux username.
