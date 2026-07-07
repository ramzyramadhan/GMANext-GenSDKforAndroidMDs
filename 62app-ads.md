Select platform: [Android](https://developers.google.com/admob/android/next-gen/app-ads "View this page for the Android platform docs.") [iOS](https://developers.google.com/admob/ios/app-ads "View this page for the iOS platform docs.") [Android (Legacy)](https://developers.google.com/admob/android/app-ads "View this page for the Android (Legacy) platform docs.")

<br />

Authorized Sellers for Apps, also known as
[app-ads.txt](https://iabtechlab.com/wp-content/uploads/2019/03/app-ads.txt-v1.0-final-.pdf),
is an IAB initiative that helps protect your app ad inventory from ad fraud. You
create app-ads.txt files to identify who is authorized to sell your inventory.
Identifying authorized sellers can help you receive advertiser spend that might
have otherwise gone toward counterfeit inventory of spoofed apps.

The app-ads.txt files are publicly available and crawlable by exchanges,
supply-side platforms (SSP), other buyers, and third-party vendors.

Use of app-ads.txt is not mandatory, but is highly recommended, especially if
you are concerned that others may be spoofing your app.

An app-ads.txt file is a text file that an app developer posts in the root
domain of their app's developer website. It contains a list of entities
authorized to sell that publisher's inventory. The usage of the app-ads.txt file
requires that publishers have a web domain to publish their authorized sellers
list for different ad tech vendors to crawl. There are a number of domain
hosting solutions that allow for the arbitrary hosting of files including
[Firebase](https://firebase.google.com/docs/hosting).

## Prerequisites

- Read [Set up an app-ads.txt file for your
  app](https://support.google.com/admob/answer/9363762).
- Browse through [Manage your Firebase
  projects](https://firebase.google.com/docs/projects/learn-more).

## How to set up app-ads.txt for your apps

1. If you haven't already, create a text file and save it with the name
   "app-ads.txt".

2. Copy and paste the following code snippet into your app-ads.txt file.
   (Replace `pub-00000000000000` with your publisher ID. Your publisher ID can
   be found at **AdMob console \> Settings**.)

       google.com, pub-00000000000000, DIRECT, f08c47fec0942fa0

3. Publish your app-ads.txt at the root of your developer website (for example,
   `https://example.com/app-ads.txt`). Make sure the domain is entered exactly
   as listed on Google Play.

   > [!NOTE]
   > **Note:** If your website does not allow you to publish app-ads.txt on the root level, you can use [Firebase Hosting](https://developers.google.com/admob/android/next-gen/app-ads#firebase) as one of the options for hosting your app-ads.txt file.

4. Wait at least 24 hours for AdMob to crawl and verify your app-ads.txt file.

5. Come back to AdMob and check your [app-ads.txt
   status](https://support.google.com/admob/answer/9788846).

## Publish app-ads.txt with Firebase Hosting

If you have a website that disallows the uploading of your app-ads.txt file at
the root level (e.g., a site built and hosted by a site-generation service), you
can use Firebase Hosting to host your app-ads.txt file.

Firebase offers a free, fast, and reliable way to host your app-ads.txt file
with your own [custom
domain](https://firebase.google.com/docs/hosting/custom-domain) or on Firebase
project's free subdomains: `web.app` and `firebaseapp.com`.

### Before you begin

You'll need to have a Firebase project to publish app-ads.txt with Firebase
Hosting. If you don't have a Firebase project, create a new one by following the
[developer guide](https://firebase.google.com/docs/projects/learn-more).

If you've already [linked your AdMob apps to
Firebase](https://support.google.com/admob/answer/6383165) or your app is using one of
the Firebase products (e.g., Google Analytics for Firebase, Remote Config,
etc.), you can use the existing Firebase project.

### Install the Firebase CLI

You can install the Firebase CLI by using [npm](https://www.npmjs.com/) (Node Package
Manager). However, if you're not familiar with Node.js, you can use the
standalone binary instead.

Visit the Firebase CLI documentation to learn how to [install the
CLI](https://firebase.google.com/docs/cli#install_the_firebase_cli) or [update to its
latest version](https://firebase.google.com/docs/cli#update-cli).

### Initialize your project

To initialize your Firebase project in your local machine, run the following
command from the root of your project directory.

```
firebase init
```

> [!IMPORTANT]
> **Key Point:** Make sure you're signed in with the account which has the Editor/Owner role of the project you're initializing.

During project initialization, from the Firebase CLI prompts:

1. Select to set up **Hosting**.

2. Select a Firebase project to connect to your local project directory.

   Select **Use an existing project**, then choose a project from the list
   that you want to connect.
3. Specify a directory to use as your public root directory.

   Press enter to select a default one (public).
4. Choose a configuration for your site.

   Since the website you're going to create is not a single-page app, select
   **N**.

At the end of initialization, Firebase creates and adds two files to the root of
your local project directory:

- A `public` directory that contains files hosted on your website.
- A `firebase.json` configuration file that lists your project configuration.
- A `.firebaserc` file that stores your project alias.

### Publish app-ads.txt

To publish app-ads.txt to your site:

1. Put the app-ads.txt file into the `public` directory in your local project
   directory.

2. Run the following command from the root of your local project directory:

   ```
   firebase deploy --only hosting
   ```
3. Once deployment is complete, visit the following URL to make sure
   app-ads.txt is published. (`PROJECT_ID` is your Firebase project ID.)

   `https://PROJECT_ID.web.app/app-ads.txt`

   Example: If "awesome-project" is the project ID, enter
   `https://awesome-project.web.app/app-ads.txt` in the address bar of your
   browser.

### Add domain/subdomain to your app's store listing

In order for your app-ads.txt file to be crawled, you will need to list the
newly created domain or subdomain in your app listing on
Google Play.


Update the
developer website in the app store listing as follows:

`https://PROJECT_ID.web.app`

> [!IMPORTANT]
> **Key Point:** It can take up to 24 hours for AdMob to crawl and verify your app-ads.txt files. Please wait at least 24 hours for the app-ads.txt status to update.

### Configure redirection settings (optional)

If you have an existing website and plan to use Firebase Hosting just for
hosting your app-ads.txt file, you can configure Firebase Hosting to redirect
the landing page to your existing website.

Firebase Hosting will use `public/index.html` as a landing page by default when
a user visits your site. To redirect users to the website that you want (for
example, your app's social media page):

1. Open `firebase.json` file located in the root of your local project
   directory.

2. Under hosting object, add redirects object as follows:

       "hosting": {
         ...
         "redirects": [
           {
             "source": "/",
             "destination": "URL_TO_REDIRECT",
             "type": 301
           }
         ]
       }

   For example, if the landing page URL is `https://www.example.com`, the
   redirect configuration will be as follows:

       "hosting": {
         ...
         "redirects": [
           {
             "source": "/",
             "destination": "https://www.example.com",
             "type": 301
           }
         ]
       }

3. Run the following command to deploy the changes to your site.

   ```
   firebase deploy --only hosting
   ```
4. Once the deployment is complete, access your site
   (`https://PROJECT_ID.web.app`) to check whether the redirection setting is
   correct or not.

## Resources

- [Ensure your app-ads.txt files can be crawled](https://support.google.com/admob/answer/9679128)
- [Learn more about app-ads.txt file statuses](https://support.google.com/admob/answer/9363762)
- [App-ads.txt FAQ](https://support.google.com/admob/answer/9675354)