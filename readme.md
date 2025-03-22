# **Fully Automated Job Alert Analyzing**

This is a recent automation flow that I built to assist with job hunting by removing the need to manually look through every job posting include in job alert emails. Historically, I've been terrible about setting up job alerts, and then filtering them away and never checking them. This pipeline takes the busy work of reading through each posting manually, and short lists the best jobs for me to apply for. 

In a perfect world we'd feed the links into ChatGPT and just have them analyze it directly, but it's inconsistent as to whether or not GPT actually reads the posting/visits the link. This method avoids this potential problem. 

### 🤖 External API Integrations Used:
- ChatGPT
- Google Sheets
- Google Tasks
- Gmail
- Web Scraping APIs (Scrape Ninja, 0codekit, webscrapingapi)

### ✨ Features:
- Sorts all incoming emails into their correct categories
- Does the heavy lifting for sorting through job alert emails
- Automatically disqualifies jobs that are in the wrong location, or that don't match my experience.
- Utilizes Regular Expressions to automatically grab the essential information without needing to use generative AI.
  

### 🌊 The Flow:
1. **Email Sorter.json**
   - Sorts emails into the correct folders/labels.
2. **Job Alert Analyzer.json**
   - Grabs valid links from the email and records them in a sheet.
3. **Posting Scraper.json**
   - Scrapes the html from each job link, and determines the different variables using common formatting convention for the different job sites.
4. **Job Validator.json**
   - Compares the job posting against my experience, and grabs any required skills for later analysis.

  
# Pipeline Breakdowns:
_Please note that in interest of space, the pipelines have been compressed to fit a single screen_
      
### 1. (Email Sorter.json) 
![Email Sorter](https://github.com/user-attachments/assets/2173daf4-2b4c-4bf7-b071-35f8ea428291)

- This is a fairly simple automation. I use a custom ChatGPT assistant to sort incoming mail into the matching label. I've set up early filters as well for job alert emails to remove the need to run them through AI.
    
  
### 2. (Job Alert Analyzer.json)
![Job Alert Analyzer](https://github.com/user-attachments/assets/b13cb170-7eeb-45fc-9fe5-a09e18d2ba38)

- The main purpose of this flow is to grab all the links using a regular expression (simply just anything starting with HTTPS). Due to the loose pattern matching, multiple layers of filters are set up to filter out additional links based on my testing.
- As certain sites (namely Bebee), utilize an internal redirect page before exposing the actual job link, I've included Scrapeninja as it features JS following allowing me to bypass the redirect barrier.

### 3. (Posting Scraper.json)
![Posting Scraper](https://github.com/user-attachments/assets/6acbda64-5823-4b1f-9b50-d4e3b95327b1)

This workflow does the heavy lifting for the pipeline:
1. It starts by scraping the posting link using either webscrapingapi or 0codekit. Webscrapingapi is cheaper so that is the priority, however, 0codekit can access Linkedin, which the former struggles with. 0codekit is also faster in most cases.
2. Once the HTML is scraped, we parse the text to be used in the regex analysis.
3. As the pipeline is meant to work on muiltiple posting links within a single execution, we reset all the variables before the following steps.
4. For each different job site, I have built out different regular expressions to scrape the essential information based on what my testing and analysis has identified as common patterns that can act as anchor points. I did have to get creative in some cases due to how Make formats different variables, but most run successfully without any errors.
5. Once the posting is analyzed and the variables collected, it's moved back to the spreadsheet. If any essential information is missing due to not matching the expression (job description or job title and organization name), it's labeled as missing on the sheet. Additional protection is included to avoid exceeding the Google sheets character limit as well.



### 4. (Job Validator.json)
![Job Validator](https://github.com/user-attachments/assets/2759486d-9d21-464d-8c56-e0b1a8d72268)

The final automation is what really saves me time, everything up to step four is in preparation to complete this step. It has two main functionalities:

1. A custom Chatgpt Assistant is sent the raw text of the posting information and based on my experience that I've trained it on, it will choose "pass" or "fail" (if it's fail it gives a reason as well). If it's a pass, a Google task is created with the job link for me to apply.
2. The second usage is to have Chatgpt pull the different skill requirements mentioned in the job posting. It will return them in an easily scrapable format, and simply tracks how many times that skill was mentioned. The current version is filtered to only track "Business Analyst" roles, but it can be tweaked to track every/any role type. 
