---
title:
description: How

date: 2019-08-08 11:33:00 +0800
categories: [Blogging, Demo]
tags: [typography]
pin: true
math: true
mermaid: true
image:
  path: https://static.independent.co.uk/s3fs-public/thumbnails/image/2016/11/27/16/nelly.jpg?quality=75&width=1200&auto=webp
  lqip: data:image/webp;base64,UklGRpoAAABXRUJQVlA4WAoAAAAQAAAADwAABwAAQUxQSDIAAAARL0AmbZurmr57yyIiqE8oiG0bejIYEQTgqiDA9vqnsUSI6H+oAERp2HZ65qP/VIAWAFZQOCBCAAAA8AEAnQEqEAAIAAVAfCWkAALp8sF8rgRgAP7o9FDvMCkMde9PK7euH5M1m6VWoDXf2FkP3BqV0ZYbO6NA/VFIAAAA
  alt: An angled view of an older model Nokia phone with a physical keyboard, displaying a spreadsheet application on its screen. The spreadsheet contains text in the first row that reads 'WHERE YOU AT' in column A and 'HOLLA WHEN YOU GET THIS' in column B, indicating a playful or informal message.
---

# The Project that Prompted my Career Switch (and How to Improve It)

## Intro

Hi everyone. This is FullstackJack, a channel that I've started to share any nuggets of insight that I might happen to have with all of you about programming and web development. My name is Jack, and I've got a background in teaching and a passion for coding. ~~So, whether you're a beginner or just looking to sharpen your skills, you’re in the right place. I’ve been thinking~~

I've been holding off on content creation for a while now, you know, perfectionism and all that, but also because I hadn't really found a topic until now.

As my first video, I’d like to share the project that prompted my decision to leave 4 years of teaching and start a career path toward web development. I'd then like to talk about how I might improve the original application that I used both in terms of features, and code quality.

## Defining the Problem

As a teacher, I was having a parent communication problem where parents said they didn't really know what was going on with their child in my classroom. Our learning management system had a simple email system, but what I found was that parents were much more engaged when you reached out to them via text. I started to look for an existing edTech solution for teachers to be able to cut through the noise of span and send out mass SMS where each message was personalized to the student and parent.

## Looking for solutions and finding one

It turned out, at the time, nothing with a polished UI yet existed for accomplishing something like this, but it didn't take long for me to find an article by [Greg Bauges](https://www.linkedin.com/in/gregbaugues/) from Twilio: [How to Send SMS from a Google Spreadsheet](https://www.twilio.com/blog/send-sms-from-a-google-spreadsheet-html) . In the post, Greg provided the following code:

```jsx
function sendSms(to, body) {
  var messages_url = "<https://api.twilio.com/2010-04-01/Accounts/YOURACCOUNTSID/Messages.json>";

  var payload = {
    To: to,
    Body: body,
    From: "YOURTWILIONUMBER",
  };

  var options = {
    method: "post",
    payload: payload,
  };

  options.headers = {
    Authorization: "Basic " + Utilities.base64Encode("YOURACCOUNTSID:YOURAUTHTOKEN"),
  };

  UrlFetchApp.fetch(messages_url, options);
}

function sendAll() {
  var sheet = SpreadsheetApp.getActiveSheet();
  var startRow = 2;
  var numRows = sheet.getLastRow() - 1;
  var dataRange = sheet.getRange(startRow, 1, numRows, 2);
  var data = dataRange.getValues();

  for (i in data) {
    var row = data[i];
    try {
      response_data = sendSms(row[0], row[1]);
      status = "sent";
    } catch (err) {
      Logger.log(err);
      status = "error";
    }
    sheet.getRange(startRow + Number(i), 3).setValue(status);
  }
}

function myFunction() {
  sendAll();
}
```

Greg's explanation of the code was fantastic. It was enough for me, an absolute beginner, to pick up on, but here's a summary

This code consists of three functions designed to send SMS messages via the Twilio API. It's written in Google Apps Script and integrates with Google Sheets.

1. **`sendSms(to, body)`**:
   - Sends an SMS message to a specific number (`to`) with a specific message (`body`).
   - Uses Twilio API to actually send the SMS.
   - The Twilio account SID, auth token, and number are placeholders and should be filled in.
2. **`sendAll()`**:
   - Reads phone numbers and messages from a Google Sheet.
   - Starts from the second row and goes until the last row of data.
   - Calls `sendSms` for each row to send the SMS.
   - Updates the sheet with the status ("sent" or "error") of each SMS send attempt.
3. **`myFunction()`**:
   - Acts as a wrapper and simply calls `sendAll()`.

## Adding automated personalization `1 min`

To get every message to be personalized, I used built-in interpolation offered by Google Sheets Expressions. It works like this:

```jsx

```

## Why I think this was a great first project `2 min`

### **Many Concepts Under One Roof**

**Functions**

This was a great way to understand how functions can be used to modularize code. The **`sendSMS`** and **`sendAll`** functions serve specific purposes and make the code reusable.

**Fetch**

The **`UrlFetchApp.fetch()`** method was a practical introduction to making HTTP requests—a fundamental concept in web development.

**Headers**

Understanding headers was crucial, especially when dealing with APIs that require authorization.

**APIs**

This project involved interacting with the Twilio API, providing hands-on experience with API calls.

**Auth**

API authentication is a topic you'll encounter often. This project taught me the basics of how to securely authenticate against an API.

**Error Handling**

With the **`try`** and **`catch`** blocks, I learned about the importance of handling errors gracefully in an application.

**Loops**

The **`for`** loop used to iterate over rows in the Google Sheet was a practical example of how loops can automate repetitive tasks.

**Class Instantiation**

While not explicit, the use of Google Apps Script services like **`SpreadsheetApp`** and **`UrlFetchApp`** gave a glimpse into object-oriented programming and class instantiation.

### Proof that I could work through new problems

This project helped solidify a healthy problem solving mindset.

### It S**howed Me Serious Value in Programming**

**More Free Time**

Automating the SMS sending process freed up a significant amount of time that would have been spent doing this manually.

**Parent Engagement Never Better**

In my case, this tool was used for sending updates to parents about school activities. The automation made it consistent and timely, leading to better engagement with parents.

## What I would do differently? `3 min`

While the basic script is functional, there's plenty of room for improvements, both in terms of features and code quality.

### Features

**Increased Personalization**

We can make an API call to ChatGPT prior to sending out the message to Twilio. We can provide ChatGPT with just enough information to provide a personalized message.

**UI: Add a Button onto the Spreadsheet to Call the Script**

Instead of manually running the script, how about adding a button directly in the Google Sheet? Clicking this button would trigger our `sendAll()` function, making the user experience seamless.

### Code Quality

**Define the Twilio Account ID and Key Once Outside of the `sendSMS` function**

By defining the Twilio Account ID and Auth Key once and referencing them in the `sendSMS` function, we adhere to the DRY principle, making the code easier to read and maintain. It's also more performant because we avoid re-declaring these variables each time we call the function.

**Use `const` / `let` Instead of `var`**

Modern JavaScript encourages the use of `const` and `let` over `var` for variable declarations. This gives us better scope control.

**Arrow Functions**

I like to use Arrow functions because they remind us that in JavaScript, functions are first-class objects and can be assigned to variables. Arrow functions are also terser and may be easier to read, depending on your coding style preference.

**Dynamically determine columns**

Instead of hardcoding which columns contain the phone numbers and messages, why not write code that dynamically figures this out? This makes our application more robust, ensuring it doesn't break if someone rearranges the columns.

## Outro `30 sec`

This is a project that I am actually interested in building out in public. So if you want to follow along, my next step is too plan out the system architecture on a FigJam board.
