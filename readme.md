This workflow analyzes job postings gathered from email in other flows. Essentially it does the following:

1. Takes a job website URL and scrapes the HTML using the 0codekit or Webscraping API.
2. Parses it into text and then utilizes this text, along with custom regular expressions, to gather the essential information from each ad. The expressions are tailored for each job site.
3. Records the data gathered and once the flow has fully finished running it triggers a webhook in another scenario that will utilize ChatGPT to compare the scraped posting against my experience.

![chrome_5qzRPKmbtR](https://github.com/user-attachments/assets/ffb6ef69-6f12-4cbd-bfe0-aa514d43879f)
![image](https://github.com/user-attachments/assets/26f929a8-0bcd-4788-a979-f9c7dfac3618)
![image](https://github.com/user-attachments/assets/f504278a-1742-4189-bebd-a97a5e3f0fd6)
