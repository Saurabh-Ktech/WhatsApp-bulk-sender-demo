# WhatsApp-bulk-sender-demo
Backend-driven WhatsApp Bulk Sender with automation using Spring Boot and Selenium
# WhatsApp Bulk Sender (Spring Boot + Selenium)

## Overview
This project is a WhatsApp bulk messaging automation tool built using Spring Boot and Selenium WebDriver. It allows users to upload an Excel file containing phone numbers and send messages in batches via WhatsApp Web.

The application includes a simple web interface, backend processing, and real-time progress updates.

# Requirements

Make sure the following are installed:

Java 17+ — https://adoptium.net
Maven 3.6+ — https://maven.apache.org
Google Chrome — Latest version
ChromeDriver — Managed automatically using WebDriverManager

# Project Structure
whatsapp-sender/
├── pom.xml
└── src/
    └── main/
        ├── java/com/kusum/whatsapp/
        │   ├── WhatsAppSenderApplication.java
        │   ├── controller/
        │   │   └── WhatsAppController.java
        │   ├── service/
        │   │   ├── WhatsAppService.java
        │   │   └── ExcelService.java
        │   └── model/
        │       ├── Store.java
        │       ├── SendRequest.java
        │       └── ProgressEvent.java
        └── resources/
            ├── application.properties
            └── static/
                └── index.html
# How to Run
1. Navigate to project directory
cd whatsapp-sender
2. Run using Maven
mvn spring-boot:run

The first run may take some time as dependencies will be downloaded.

3. Open in browser
http://localhost:8081
# Initial Setup (One-Time)

After starting the application:

Upload the Excel file
Click on Start Sending
A Chrome window will open with WhatsApp Web
Scan the QR code using your phone
Messages will start sending automatically

Note: The Chrome session is saved locally (~/whatsapp-chrome-profile/), so you won’t need to scan the QR code again.

# How It Works
Frontend (Browser UI)
        ↓ Upload Excel
Spring Boot Backend (localhost:8081)
        ↓ Parse contacts
Selenium WebDriver
        ↓ Controls Chrome
WhatsApp Web
        ↓ Types message
        ↓ Clicks send
        ✓ Message delivered
 Batch Processing

Messages are sent in batches to avoid detection:

Batch 1 (5 contacts) → Send → Send → Send → Send → Send
        ↓ Wait (5 seconds)
Batch 2 (5 contacts) → ...

This continues until all messages are delivered.

# Troubleshooting
❌ Server not starting?
java -version   # Should be 17+
mvn -version    # Ensure Maven is installed
❌ ChromeDriver issues?
WebDriverManager handles driver automatically
Ensure Chrome browser is updated
❌ Messages not sending?
Check Chrome automation window
Ensure WhatsApp Web is logged in
Verify phone number format
❌ Invalid phone number error?
Do not include country code (91 is auto-added)
Remove spaces or special characters
🏗️ Production Recommendations

For real-world usage, consider adding:

Rate limiting (to avoid WhatsApp bans)
Retry mechanism for failed messages
Database integration (e.g., MongoDB)
Logging system
Queue-based processing
# Tech Stack
Technology	Purpose
Spring Boot 3.2	Backend server & APIs
Selenium 4.16	Browser automation
Apache POI 5.2	Excel file processing
WebDriverManager	Driver management
SSE (Server-Sent Events)	Real-time updates
Lombok	Boilerplate reduction


👨‍💻 Author

Developed by Saurabh Kumar
