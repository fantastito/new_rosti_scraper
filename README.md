# What's this?
This is a thinly veiled excuse to try web scraping and use serverless computing. It's 'new' because a previous attempt didn't work and it's a 'rösti scraper' because it checks if I can buy rösti at my local supermarket.

![Python](https://img.shields.io/badge/python-3670A0?style=flat&logo=python&logoColor=ffdd54) ![AWS Lambda Badge](https://img.shields.io/badge/AWS%20Lambda-F90?logo=awslambda&logoColor=fff&style=flat) ![Amazon Simple Email Service Badge](https://img.shields.io/badge/Amazon%20Simple%20Email%20Service-DD344C?logo=amazonsimpleemailservice&logoColor=fff&style=flat) ![Docker Badge](https://img.shields.io/badge/Docker-2496ED?logo=docker&logoColor=fff&style=flat)

## Use case
As a user, I'd like to recieve a notification when I can buy rösti in the supermarket.

## Solution
A cron expression triggers an AWS Lambda script once a week; BeautifulSoup checks [Lidl's offers page](https://www.lidl.co.uk/c/food-offers/s10023092) to see if this week is 'Alpen Fest', when rösti is normally sold, returning an appropriate subject and email body; this is passed to AWS Simple Email Service and lands in my inbox.

# What didn't work?
The original rösti_scraper was written in the gaps between childcare over a weekend. I hoped to take advantage of the great benefit of serverless computing—simply uploading a script and clicking run. Quickly (naturally), I ran into trouble: on AWS Lambda, my code didn't have access to the BeautifulSoup library, or any other dependencies.

I followed instructions and tutorials (all I needed to do was install the dependencies inside a folder, zip and upload it) but I couldn't get this solution to work.

In the end I tried another route: containerising my code with Docker and uploading. This solution has a larger footprint but brings the stability of a framework. Jonathan Davies' tutorial on [developing lambdas in VS Code using AWS SAM](https://www.youtube.com/watch?v=mhdX4znMd2Q) was quick and clear, swiftly setting up a 'Hello world' template.

With this starting point I could then adapt the template.yaml, app.py and requirements.txt to achieve my goal.

# What if I'd like to do this?
Depending on familiarity and goals, the usual tools provide most answers and you could start writing code in AWS Lambda right away, though I'd recommend [Jonathan Davies' tutorial](https://www.youtube.com/watch?v=mhdX4znMd2Q).

You'll need to:
- play around with BeautifulSoup
- understand what robot.txt files are
- make friends with AWS IAM to manage permissions for your lambda function to send an email
- set up AWS Simple Email Service to send an email

# #Improve
AWS Lambda provides 'layers' that functions can use; if I'd got my script working via .zip upload, I would have liked run the dependencies from a layer, allowing for reuse with other projects. I might also have seperated out the logic inside my app.py file, spliting 'send email' and 'web scrape' into two functions.