# Security Assessment Report: Lab 6 - Web Application Hacking
**Environment:** Decentralized Academic Lab Network (Local Workstation Hosting)

## What We Did
We targeted the OWASP Juice Shop application hosted locally at 127.0.0.1. We bypassed the login portal using a manual SQL injection in the email field (`' OR 1=1 --`). For data extraction, standard payloads against the backend search API kept throwing 500 errors due to the query structure. We pivoted to a highly targeted, automated scan using sqlmap to force the SQLite database to dump its tables. We finished the assessment by injecting an XSS payload to pop a client-side alert.

## Commands & Flags
* `sqlmap -u "http://127.0.0.1/rest/products/search?q=1" --dbms=sqlite --level=5 --risk=3 --batch --dump`
    * `-u`: Specifies the target URL and vulnerable parameter.
    * `--dbms=sqlite`: Forces the tool to strictly use SQLite payloads, saving fingerprinting time and reducing noise.
    * `--level=5`: Executes the highest level of tests and payloads available.
    * `--risk=3`: Authorizes the tool to use high-risk payloads, including heavy time-based queries.
    * `--batch`: Automatically answers "yes" or default to all tool prompts to keep the scan running unattended.
    * `--dump`: Instructs the tool to extract and download the database table entries.

## The Results
We completely compromised the web application's backend infrastructure. We successfully bypassed authentication, dumped sensitive tables from the SQLite database, and proved client-side execution via Cross-Site Scripting (XSS).

![](./Images/5.%20server.png)

