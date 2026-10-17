# 🛒 SmartCheckout

A self-checkout website for an app store project. It is a single HTML file with no server and no database: everything runs in the browser on the checkout machine.

> **This is a game/demo.** No real payments are taken. Pressing **Pay** just shows "Payment successful" and never asks for card details.

## Features

**Self-checkout (on the machine)**
- Uses the camera to recognise products placed in the on-screen box and adds them to the basket.
- Records **video and audio** from the camera and saves it on the machine (in the browser's IndexedDB), not in the cloud.
- After 5 seconds of inactivity it switches to a product slideshow. Touch the screen to wake it.
- **Price check** button: hold an item in the box to see its price.
- Always-visible **Need help?** button that alerts staff.
- Suspicious activity (an unscanned item, or a simulated theft) locks the checkout until staff scan their QR code and review the footage.
- Checkout asks "Do you have our app?" and then either shows a receipt QR code or asks for a phone number.
- Receipts are generated and saved on the machine.
- Starting self-checkout goes full screen and locks the page. Only a valid staff QR scan can leave it.

**Customer portal**
- Log in with a phone number and PIN, view purchases and receipts.
- Scan the receipt QR shown at checkout to save that receipt to your account.

**Staff portal**
- Log in with the staff password, then get a digital **staff QR code** (valid for 15 minutes, works from any phone).
- Tabs: **Access QR**, **Manual add**, **Alerts**, **Live** camera, **Products**, **Receipts**.
- Add products: place the item in the box, take its photo, then give it a name and price.
- Review footage from the time of an alert and tap **It's OK** to let the customer carry on.

## Quick start

1. Put the files in a GitHub repository. Rename `self-checkout.html` to `index.html`.
2. In the repo go to **Settings → Pages**, choose **Deploy from a branch**, pick `main` and `/ (root)`, then save.
3. Open the Pages URL on the checkout machine in Chrome. Camera and microphone prompts only work on `https` pages (GitHub Pages is fine) or `localhost`.
4. Open **Staff portal**, log in, go to **Products**, and add a few products.
5. Go back to the home screen and choose **Self-checkout**. Allow camera and microphone when Chrome asks.

For a proper lockdown of the machine, launch Chrome in kiosk mode: `chrome --kiosk https://your-site-url`. A web page on its own cannot stop someone using the rest of the computer.

## How staff use it

**Getting into the staff tools on the checkout**
1. On your phone, open the site, go to **Staff portal** and enter the password.
2. Your phone shows a staff QR code.
3. Hold it up to the checkout camera. The checkout opens the staff tools.
4. Use **Back to checkout** to return, or **Exit self-checkout mode** to leave self-checkout completely.

**Adding an item by hand when scanning fails**
1. The customer taps **Need help?**. The checkout shows "Help is on the way".
2. On your phone, open **Staff portal → Manual add**.
3. Type the item name, price and quantity, then tap **Show QR code**.
4. Hold the phone up to the checkout camera. The item is added to the customer's basket.

This only works while a Help request is open, and each QR code can be used once. The Help request closes automatically when the customer pays.

**Handling an alert**
When the checkout locks, scan your staff QR, open **Alerts**, tap **View footage**, and then tap **It's OK** to unlock it.

## Changing the password

The staff password is the `PW` constant near the top of the script (default `9463`). Change it there and redeploy. Because everything is client-side, the password is visible to anyone who views the page source, so this is only suitable for a game or demo.

## Where data lives

| Data | Location |
| --- | --- |
| Products, receipts, alerts, accounts | `localStorage` in the browser on that device |
| Camera and microphone footage | IndexedDB in the browser on the checkout machine (about the last 20 minutes, in 10-second clips) |

Clearing the browser's site data deletes all of it. Each device has its own data: a staff phone does not see the checkout's products, receipts or alerts.

## Limitations

- **Product recognition** is a simple visual match against the photo taken in the box, not true object recognition. Keep the lighting and camera position the same as when the products were added.
- **Unscanned-item detection** is a motion check (lots of movement with items in the basket but no scan). It can be turned off in **Staff portal → Access QR**.
- **No cross-device alerts.** Without a server, the Help button and alerts show on the checkout machine (and in the staff portal on that machine) but cannot be pushed to a phone.
- **Phone-number receipts** are saved on the checkout machine only. The receipt QR code is the way to get a receipt onto a customer's phone.
- **Receipt QR codes** hold about 6 to 10 items before item names are shortened.
- Footage playback and the live view only work on the machine that recorded them.

## Built with

Plain HTML, CSS and JavaScript, plus [qrcodejs](https://github.com/davidshimjs/qrcodejs) for generating QR codes and [jsQR](https://github.com/cozmo/jsQR) for reading them (both loaded from cdnjs).
