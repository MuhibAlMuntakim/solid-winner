# solid-winner
# Task 1: Web Scraping & Data Processing

**Objective:**  
Scrape 30,000 data points from [Trustpilot](https://www.trustpilot.com/) and process the data into a structured CSV file.

**Key Requirements:**

- **Data Points:**  
  - Extract 30,000 records by targeting multiple categories (e.g., *beauty_wellbeing*, *food_beverages_tobacco*, *electronics_technology*). I chose to scrape from 'Categories' because it provides the email data needed for the upcoming task, as Trustpilot does not disclose reviewer emails due to privacy policies. Each category contains up to 10,000 listings, so by selecting three categories with the highest number of entries, we can obtain 30,000 data points with email information—all while staying within privacy guidelines. For testing, I set `max_items=100`—this should be increased to 30,000 to scrape the full dataset. On average, it takes about 2 seconds per lead. This delay is due to the challenge of scraping email data from Trustpilot, as the email is hidden behind a pop-up that only appears after clicking the "Contact" button and requires a waiting period.
  

- **Fields to Extract:**  
  - **Business Name:** Extracted from a dedicated business card.
  - **Website:** Refined URL (removing UTM parameters) from the contact details.
  - **Location:** Address information from the contact details.
  - **Email:** Extracted from the tooltip that appears after clicking the "Contact" button.
  - **Category:** The category from which the data point was scraped.

- **Automation:**  
  - Fully automated Python script using Selenium (with webdriver-manager for driver setup) to navigate and scrape data without manual intervention.
  - Implements pagination to traverse multiple pages.

- **Error Handling & Deduplication:**  
  - Comprehensive exception handling for element retrieval, timeouts, and network issues.
  - Retry mechanisms to ensure data extraction even if some elements are delayed.
  - Deduplication logic to remove any duplicate records before saving.

**Outcome:**  
A reproducible, automated script that outputs a CSV file (`trustpilot_data.csv`) containing the deduplicated records with fields: Category, Business Name, Website, Location, and Email.

# Task 2: Email Marketing Automation

**Objective:**  
Automate the process of sending 20–50 personalized emails using the scraped Trustpilot data. For this implementation, we send 7 emails per target category (beauty_wellbeing, food_beverages_tobacco, and electronics_technology), totaling 21 emails.

**Key Features:**

- **Personalized Emails:**  
  Each email is customized using dynamic fields from the scraped data (e.g., Business Name, Location, Category). The Groq API is used to generate concise, professional emails that include a subject line, a clear call-to-action, and a proper sign-off.

- **Fixed Special Offer:**  
  A predefined offer ("a special 10% discount on our services for new clients") is included in every email to ensure consistency across the campaign.

- **Automated Delivery:**  
  The script uses Gmail-SMTP for sending emails automatically without any manual intervention. It also implements rate-limiting to comply with API and SMTP best practices.

- **CSV Integration & Updates:**  
  The script reads the scraped data from `trustpilot_data.csv` (produced in Task 1) and groups records by category. After sending an email, it updates the CSV by adding two new fields:
  - **Email Sent:** Marked "Yes" if an email was successfully sent, otherwise "No".
  - **Email Body:** Stores the exact email content sent.

- **Robust Error Handling:**  
  Comprehensive error handling is built-in for API requests and SMTP operations, ensuring reliable automation even when issues arise.

**Workflow:**

1. **Data Loading:**  
   The script reads the scraped data from `trustpilot_data.csv` and groups records by target categories.

2. **Email Generation:**  
   For each selected business (7 per category), the script generates a personalized email via the Groq API. The prompt instructs the model to produce a concise email with a subject line, body, and professional sign-off (ending with "Best regards, Muhib Al Muntakim, Ray Advertising Limited") that includes the fixed offer.

3. **Email Sending:**  
   The generated email is sent using Gmail-SMTP. If an email is successfully sent, the record is updated to reflect this status along with the email body.

4. **CSV Update:**  
   Once all emails are sent, the updated records (with the new fields "Email Sent" and "Email Body") are written back to `trustpilot_data.csv` for complete traceability.

**Outcome:**  
A fully automated email marketing solution that leverages your scraped Trustpilot data to send 21 customized B2B emails and updates your dataset with detailed send-status information, ensuring consistency and reliability for your campaign.
