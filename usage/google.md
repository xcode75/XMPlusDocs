## Google Mail
  
### Obtain Google Client ID and Client Secret
 
Click to enter [Google Cloud Platform Console](https://console.developers.google.com/) Before that, you must register and log in with your Google account

Click APIs and Services and then click the OAuth consent screen. You may need to click the Menu icon "" first

In the User Type field, select External

click create

In the App Name field, add the name of your app

Select user support email

Add Authorized domains

In the Developer Contact Information field, enter an email address where Google can contact you about changes to the project

Click Save and Continue, then click Return to Dashboard

Click APIs and Services and then click Credentials. You may need to click the Menu icon "" first

Click Create Credentials and then click OAuth Client ID

For Application Type, choose Web Application

For Name, enter a name for the OAuth web client.

For Authorized JavaScript origins, click Add URI and enter Hosting (XMPlus System Settings -> Google Settings -> Redirect URL) eg http://tld.com/getGoogleToken

click create

Copy your Client ID and Client Secret section

Tip: You can also get the client ID through APIs and services and then click Credentials

### Mail Settings

XMPlus System Settings -> Mail Settings -> Select Google (Google Settings) / Select Others (use other SMTP information)

### Google Settings

XMPlus System Settings -> Google Settings

- Enter your google email
- Enter client ID
- Enter client secret
- Select Enable for Renewal Token and click Submit. Follow the instructions and allow access.

