# 📈 AI Trading Journal

A smart, cloud-synced trading journal powered by Google Gemini AI and Google Sheets. 

This project allows day traders and investors to log their trades using natural language. The AI acts as a trading coach, extracts key metrics (Entry, Stop, Target, R-Multiple, Emotion), and saves the structured data directly into a private Google Sheet.

## ✨ Features
* **Natural Language Processing:** Log trades simply by chatting with the AI.
* **Instant Feedback:** The AI identifies rule violations (e.g., revenge trading, missing stop-loss).
* **Cloud Database:** Seamlessly saves data to Google Sheets via Google Apps Script (No traditional backend required).
* **Responsive UI:** Dark-mode dashboard displaying R-multiple progress bars and execution quality.
* **Privacy First:** API keys are stored locally in the browser's local storage and never sent to external servers.

## 🛠️ Built With
* **Frontend:** HTML5, CSS3, Vanilla JavaScript
* **Backend / Database:** Google Apps Script, Google Sheets API
* **AI Model:** Google Gemini 1.5 Pro API

## 🚀 How to Run It Locally
Since this is a static web application, running it is incredibly simple.

1. Clone the repository:
   ```bash
   git clone [https://github.com/YourUsername/my-trading-journal.git](https://github.com/YourUsername/my-trading-journal.git)
   ```

2. Open the `index.html` file in your favorite web browser.

3. In the header, enter your:
   * **Gemini API Key** (Get one from Google AI Studio)
   * **Google Script URL** (See instructions below)

## 📊 Setting Up Your Own Google Sheet Database
If you want to use this journal and save your own trades:

1. Create a new Google Sheet.
2. Go to `Extensions` > `Apps Script`.
3. Delete any existing code and paste the following backend code:

   ```javascript
   function doPost(e) {
     try {
       var data = JSON.parse(e.postData.contents);
       var sheet = SpreadsheetApp.getActiveSpreadsheet().getActiveSheet();
       
       sheet.appendRow([
         data.date, data.ticker, data.setup_type, data.direction,
         data.entry_price, data.stop_price, data.target_price,
         data.result, data.actual_r, data.emotional_state,
         data.did_right, data.would_change
       ]);
       
       return ContentService.createTextOutput(JSON.stringify({"status": "success"}))
         .setMimeType(ContentService.MimeType.JSON);
     } catch (f) {
       return ContentService.createTextOutput(JSON.stringify({"status": "error"}))
         .setMimeType(ContentService.MimeType.JSON);
     }
   }

   function doGet() {
     var sheet = SpreadsheetApp.getActiveSpreadsheet().getActiveSheet();
     var data = sheet.getDataRange().getValues();
     var headers = data.shift();
     
     var result = data.map(function(row) {
       var obj = {};
       headers.forEach(function(header, i) {
         obj[header.toLowerCase().replace(" ", "_")] = row[i];
       });
       return obj;
     });
     
     return ContentService.createTextOutput(JSON.stringify(result))
       .setMimeType(ContentService.MimeType.JSON);
   }
   ```

4. Click `Deploy` > `New Deployment` > Choose `Web App`.
5. Set "Who has access" to `Anyone`.
6. Click Deploy, authorize the app, copy the Web App URL, and paste it into the "Google Script URL" input in the application.

## 📱 Live Demo
Check out the live application here: [Insert your Vercel/Netlify/GitHub Pages link here]