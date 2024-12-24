# U-Boat Commanders Web Scraping Project

## Overview

The objective of this project is to use a **PYTHON SCRAPING BOT** to collect and organize detailed information on all U-boat commanders listed on the webpage [https://uboat.net/men/commanders/a.htm](https://uboat.net/men/commanders/a.htm). This includes approximately 1,400 commanders, whose names span from A to Z. It collects key data such as the commander's name, date of birth, date of death, ranks, and decorations. The script processes the raw data, cleans it, formats it for better presentation, and saves the results in a structured Excel file for easy analysis and further use.

The script utilizes various Python libraries, including `requests` for making HTTP requests, `BeautifulSoup` for parsing HTML content, `re` for using regular expressions to extract specific patterns, and `pandas` for data manipulation and organization.

## Steps in the Script

### Step 1: Extracting a List of BeautifulSoup Objects

In this step, the script constructs a list of URLs that link to the individual pages of U-Boat commanders. The U-Boat commander pages are indexed numerically, and the script dynamically generates URLs from the base URL by looping through a defined range of page numbers.

The script then uses the `requests` library to fetch the HTML content of each URL. The response text from each URL is parsed using `BeautifulSoup` into BeautifulSoup objects, which are stored in a list (`bs_list`) for further extraction.

**Key features of this step:**
- **Generating URLs**: URLs for the commanders' pages are generated based on an index range.
- **HTTP requests**: The script makes GET requests to fetch the page content.
- **Error handling and retries**: In case of failed requests (due to timeouts, connection issues, or server errors), the script automatically retries the request up to 10 times with increasing wait times between each attempt.

#### Main functions:
- `getpage(url_link)`: This function takes a list of URLs and retrieves the corresponding BeautifulSoup objects by sending HTTP requests to each URL. It also handles retries in case of connection issues.

### Step 2: Extracting the Desired Contents

Once the BeautifulSoup objects are created, the next step is to extract the desired data from each page. This includes:
- **Name of Commander**: Extracted from the `<h1>` tag.
- **Date of Birth**: Extracted from the table cell containing the string "Born".
- **Date of Death**: Extracted from the table cell containing the string "Died".
- **Ranks**: Extracted from a specific table cell with class `width400`.
- **Decorations**: Extracted from the same `width400` table cell after filtering out the ranks.

The data for each category is appended to respective lists (`Name`, `Born`, `Died`, `Ranks`, and `Decorations`). Regular expressions are used to identify specific patterns in the HTML content and extract the correct values.

#### Main functions:
- `getcontent()`: This function loops through each BeautifulSoup object in the `bs_list` and extracts the required data (name, birth, death, ranks, and decorations) using various parsing techniques, including regular expressions.

### Step 3: Formatting/Cleaning the Data for Better Presentation

After extracting the data, the script formats and cleans it to make it more useful:
- **Padding Lists**: The lengths of the `Ranks` and `Decorations` lists are padded to ensure each list entry has the same number of values. If a commander has fewer ranks or decorations, `None` values are added to match the longest list in the data.
- **Creating a Pandas DataFrame**: The extracted data is organized into a `pandas` DataFrame, where each column corresponds to one of the attributes (Name, Born, Died, Ranks, Decorations).
- **Exploding the DataFrame**: The DataFrame is "exploded" so that each rank and decoration appears in its own row, making it easier to analyze the data for each commander.
- **Grouping Data**: The data is grouped by the commander's name, and duplicate ranks or decorations are removed by aggregating unique entries.
- **Creating Subcolumns**: For better presentation, new subcolumns are created for ranks and decorations, where each rank or decoration is assigned to its own column, based on the maximum number of ranks or decorations for any commander.

#### Main functions:
- Data manipulation using `pandas` to clean, pad, explode, and group the data.
- Creation of subcolumns for ranks and decorations to display each entry in its own column.

### Output

The final output is a cleaned and well-structured DataFrame that contains the following columns:
- **Name of Commander**: The full name of the commander.
- **Born**: The date of birth.
- **Died**: The date of death.
- **Ranks**: One or more columns representing the commander's ranks.
- **Decorations**: One or more columns representing the commander's decorations.

The script also generates an Excel file (`Scraped U-Boat Commanders.xlsx`) containing the cleaned and formatted data. This file can be opened for analysis or used in further research.

### Disclaimer

**Please note that this script relies on the structure of the uboat.net website to extract data.** If the website's layout or structure changes, the script may fail to work as expected. In such cases, the HTML parsing logic and URL generation may need to be updated to align with the new structure of the site.

### Required Libraries

This script relies on the following Python libraries:
- `requests`: For sending HTTP requests to retrieve HTML pages.
- `beautifulsoup4`: For parsing and extracting data from HTML content.
- `re`: For performing regular expression-based text matching to extract specific information.
- `pandas`: For data processing, cleaning, and aggregation.
- `openpyxl`: For saving the final DataFrame to an Excel file (optional but recommended).

You can install the required libraries using `pip`:

```bash
pip install requests beautifulsoup4 pandas openpyxl
