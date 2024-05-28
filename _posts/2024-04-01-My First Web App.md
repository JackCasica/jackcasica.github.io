---
title: My First Web App
description: A reflection on my first web app project and the lessons learned.
published: true
date: 2022-08-08 11:33:00 +0800
categories: [Blogging, Demo]
tags: [typography]
pin: false
math: true
mermaid: true
image:
  path: https://static.independent.co.uk/s3fs-public/thumbnails/image/2016/11/27/16/nelly.jpg?quality=75&width=1200&auto=webp
  lqip: data:image/webp;base64,UklGRpoAAABXRUJQVlA4WAoAAAAQAAAADwAABwAAQUxQSDIAAAARL0AmbZurmr57yyIiqE8oiG0bejIYEQTgqiDA9vqnsUSI6H+oAERp2HZ65qP/VIAWAFZQOCBCAAAA8AEAnQEqEAAIAAVAfCWkAALp8sF8rgRgAP7o9FDvMCkMde9PK7euH5M1m6VWoDXf2FkP3BqV0ZYbO6NA/VFIAAAA
  alt: An angled view of an older model Nokia phone with a physical keyboard, displaying a spreadsheet application on its screen. The spreadsheet contains text in the first row that reads 'WHERE YOU AT' in column A and 'HOLLA WHEN YOU GET THIS' in column B, indicating a playful or informal message.
---

### Introduction and Background

During my four years as a middle school math teacher, I encountered a significant challenge: maintaining consistent communication with parents to keep them informed about their children's progress. Engaging parents in their children's education can significantly improve student accountability and performance, but reaching out to each parent individually was time-consuming. This challenge led me to explore technical solutions and eventually build my first web application to streamline this process.

### Identifying the Problem

Eighth graders, especially in a subject like math, often struggle with motivation and accountability. I noticed that students whose parents were actively engaged tended to perform better. However, manually contacting each parent was impractical due to the large number of students I taught. I needed a scalable solution that could send personalized, non-generic messages to multiple parents, including specific details about each student's performance.

### Researching Existing Solutions

At the time, existing educational software solutions did not meet my needs. Despite extensive searching, I couldn't find any tools that allowed me to efficiently send personalized updates to parents. This gap in available solutions led me to consider developing my own application. My technical inclination guided me to a Twilio blog post titled "Send SMS from Google Sheets with Twilio." This post provided a detailed walkthrough for integrating Twilio's SMS capabilities with Google Sheets using Google Scripts.

### Building the Web Application

Following the Twilio blog post, I implemented the solution using JavaScript and Google Scripts. Here’s a detailed summary of the implementation process:

#### Step 1: Setting Up Twilio

1. **Create a Twilio Account**: Sign up for a Twilio account and verify your phone number.
2. **Get Twilio Credentials**: Once your account is set up, navigate to the Twilio Console to get your Account SID and Auth Token. These credentials are necessary for authenticating API requests.
3. **Purchase a Twilio Phone Number**: From the Twilio Console, buy a phone number capable of sending SMS messages.

#### Step 2: Google Sheets Preparation

1. **Create a Google Sheet**: Set up a new Google Sheet to store student data. Include columns for parents' names, phone numbers, and relevant student performance data (e.g., homework completion, test scores).
2. **Populate the Sheet**: Manually input or import student data from a CSV file downloaded from the Schoology learning management system.

#### Step 3: Writing Google Scripts

1. **Open Google Scripts**: In your Google Sheet, navigate to `Extensions > Apps Script` to open the Google Scripts editor.
2. **Write the Script**: Write a script to fetch data from the Google Sheet and send personalized SMS messages via Twilio. Below is a sample script based on the Twilio blog post:

```javascript
function sendSMS() {
  const sheet = SpreadsheetApp.getActiveSpreadsheet().getActiveSheet();
  const data = sheet.getDataRange().getValues();
  const accountSid = "your_account_sid"; // Replace with your Twilio Account SID
  const authToken = "your_auth_token"; // Replace with your Twilio Auth Token
  const twilioNumber = "your_twilio_number"; // Replace with your Twilio Phone Number

  // Iterate over each row in the sheet
  data.forEach((row, index) => {
    if (index === 0) return; // Skip header row
    const parentName = row[0];
    const phoneNumber = row[1];
    const studentName = row[2];
    const performanceData = row[3];

    const message = `Hello ${parentName}, this is an update on ${studentName}'s performance: ${performanceData}.`;

    // Construct the payload for the POST request
    const payload = {
      To: phoneNumber,
      From: twilioNumber,
      Body: message,
    };

    // Configure the options for the HTTP request
    const options = {
      method: "post",
      payload: payload,
      headers: {
        Authorization:
          "Basic " + Utilities.base64Encode(accountSid + ":" + authToken),
      },
    };

    // Make the HTTP request to the Twilio API
    UrlFetchApp.fetch(
      "https://api.twilio.com/2010-04-01/Accounts/" +
        accountSid +
        "/Messages.json",
      options
    );
  });
}
```

3. **Save and Test the Script**: Save the script and run it to ensure it works as expected. Test with a small batch of data to verify that messages are sent correctly.

#### Step 4: Manual Data Handling

Since direct integration with Schoology was not possible, I manually downloaded a CSV file with student performance data and imported it into the Google Sheet weekly. This step ensured that the data used for SMS updates was current.

### Implementing and Testing the Solution

After setting up the system, I sent out the first batch of messages to parents, informing them of the upcoming weekly updates and giving them the option to opt-out. The feedback was positive, with many parents appreciating the regular updates on their child's progress. This new communication method significantly improved parent engagement and student accountability. Students became more diligent with their homework, knowing their parents were kept informed.

### Measuring Impact and Results

To measure the impact, I tracked homework completion rates and overall student performance before and after implementing the system. The results showed a noticeable increase in homework completion rates and improved grades for many students. The ease of sending personalized messages to 130 students' parents each week made a significant difference in maintaining consistent communication.

### Sharing the Solution with Others

Recognizing the potential of this solution, I presented it at my school district's edtech conference. The session was well-received, and several teachers adopted the system for their own classes. It’s gratifying to know that some of my colleagues are still using this approach to enhance parent communication and engagement.

### Learning Value and Programming Concepts

This project not only solved a practical problem but also provided significant learning value in a programming context. Here are some best practices and programming concepts showcased in this project:

1. **APIs and Third-Party Integrations**: Learning how to use the Twilio API for sending SMS messages introduced the concept of third-party integrations, which are crucial in modern web development.
2. **JavaScript and Google Apps Script**: Writing scripts in JavaScript and Google Apps Script provided hands-on experience with a popular programming language and an environment for automating tasks.
3. **Data Manipulation**: Working with Google Sheets for data storage and manipulation highlighted the importance of efficiently handling and processing data.
4. **Automation**: Automating the process of sending personalized messages to multiple recipients demonstrated the power of automation in reducing repetitive tasks.
5. **String Interpolation**: Using string interpolation to personalize messages emphasized the value of dynamic content generation.
6. **Error Handling and Testing**: Implementing and testing the script involved debugging and error handling, essential skills for any developer.
7. **User Feedback and Iteration**: Collecting feedback from parents and iterating on the solution underscored the importance of user-centered design and continuous improvement.
8. **Manual vs. Automated Processes**: The project also illustrated the trade-offs between manual and automated processes, guiding decisions on when and how to automate.

### Personal and Professional Growth

This project marked a turning point in my career. Seeing the tangible impact of a simple web application inspired me to pursue a career in web development. I realized the power of programming to solve real-world problems, not just in education but in various fields. My subsequent journey included furthering my education in educational technology and ultimately transitioning into a full-time developer role.

### Conclusion and Future Directions

This experience was a transformative introduction to programming. It demonstrated the profound effect that technology can have on education and beyond. As I continue to develop my skills and work on new projects, I carry with me the lessons learned from this initial foray into web development. For educators facing similar challenges, I encourage exploring technical solutions and leveraging the power of programming to create impactful changes.

---

This revised version includes a section on the learning value and programming concepts highlighted in this project, providing a comprehensive and informative guide to your experience building the web application. Feel free to adjust any parts or add more specific details as needed.
