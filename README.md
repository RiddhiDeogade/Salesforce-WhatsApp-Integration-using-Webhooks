# Salesforcs-WhatsApp-Integration-using-Webhooks

## 🔁 PART 1: Set Up WhatsApp on Twilio (Sandbox Mode)
✅ Step 1: Access WhatsApp Sandbox
Go to your Twilio Console WhatsApp Sandbox.

You’ll see:

A sandbox number

A code like: join smoke-blue (this is your keyword)

✅ Step 2: Join the Sandbox
On your WhatsApp, send the join code (e.g., join smoke-blue) to the sandbox number shown.

You’ll now be connected to Twilio WhatsApp Sandbox.

🔁 Full Setup to Create Salesforce Webhook for Twilio Integration
🔧 Prerequisites:
You have a Salesforce Developer Org (sign up here: https://developer.salesforce.com/signup)

You are using Twilio Sandbox for WhatsApp (setup done)

You are familiar with basic Salesforce navigation

## ✅ PART 1: Create a Salesforce Site
Salesforce Sites allow you to expose Apex classes as public endpoints.

🔹 Step 1.1: Enable Sites
Go to Setup

In Quick Find, search: Sites

Click Sites, then click "New"

🔹 Step 1.2: Configure the Site
Field	Value Example
Site Label	TwilioIntegrationSite
Site Name	TwilioIntegrationSite
Site Contact	[Select your admin user]
Default Web Address	twiliointegration (will be part of URL)
Active	✅ Check it
Active Site Home Page	InMaintenance or UnderConstruction
Click Save	

After saving, you’ll get a Site URL like:

arduino
Copy code
https://yourdomain-dev-ed.my.site.com/twiliointegration
## ✅ PART 2: Create Apex Class for Webhook
🔹 Step 2.1: Go to Developer Console
Click the gear icon (⚙️) → Developer Console

File → New → Apex Class

Name it: TwilioWebhook

🔹 Step 2.2: Paste This Code:
apex

    @RestResource(urlMapping='/TwilioWebhook/*')
    global with sharing class TwilioWebhook {
        @HttpPost
        global static void handleIncoming() {
            RestRequest req = RestContext.request;
            String from = req.params.get('From');
            String body = req.params.get('Body');
    
            // Log received message
            System.debug('Incoming from: ' + from + ', Message: ' + body);
    
            // Optional: Create a record or reply
        }
    }
## ✅ PART 3: Give Guest User Access
By default, your site is read-only and has no Apex access. Let’s change that.

🔹 Step 3.1: Assign Apex Class Permission to Guest User
Go back to Sites (Setup → Sites)

Find your Site → Click Site Label

Click Public Access Settings

Under Enabled Apex Class Access:

Click Edit

Add TwilioWebhook → Save

_______________________________________________________________
🔹 Step 3.2: Allow Object Access (if needed)
If you are saving messages as records (e.g. Leads, Cases), also:
✅ Step-by-Step: Access Object Settings for Guest User in Salesforce
🔹 Step 1: Open Your Site
Go to Setup (gear icon > Setup).

In the Quick Find box (top left), type and select Sites.

Scroll to find the Site you created (e.g., TwilioIntegrationSite).

Click the Site Label (blue text link).

🔹 Step 2: Open Public Access Settings
On the site details page, click Public Access Settings (a button near the top).

This opens the Guest User Profile.

🔹 Step 3: Find Object Settings
Scroll down OR use the left-side menu and click on Object Settings (under "Apps" section).

(If you don’t see a side menu, click the ☰ icon or scroll way down until you find Object Settings.)

You’ll see a list of all standard and custom objects.

🔹 Step 4: Edit Permissions
Click on the object you want to give access to (e.g., Lead, Case, or your custom object).

Click Edit (top of the page).

Enable:

✅ Read

✅ Create

Click Save.
__________________________________________________________________

## ✅ PART 4: Test Your Webhook
🔹 Step 4.1: Copy the Full URL
It will be:

arduino
Copy code
https://yourdomain-dev-ed.my.site.com/twiliointegration/services/apexrest/TwilioWebhook
This is your Twilio webhook URL.

## ✅ PART 5: Add URL in Twilio
Go to Twilio WhatsApp Sandbox Settings

In “When a Message Comes In” field, paste:

arduino
Copy code
https://yourdomain-dev-ed.my.site.com/twiliointegration/services/apexrest/TwilioWebhook
Save

