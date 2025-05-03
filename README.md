# InstaChatRemover for Instagram

[![Language](https://img.shields.io/badge/Language-JavaScript-yellow.svg)](https://developer.mozilla.org/en-US/docs/Web/JavaScript)

A browser console script to sequentially delete entire Instagram Direct Message conversations.

## Description

This JavaScript snippet automates the deletion of Instagram Direct Message conversations directly from your browser's developer console. It simulates the clicks required to delete the currently open chat thread, waits for the interface to update, and then automatically selects the next conversation in the list. The script repeats this process up to a defined number of times (default is 40), allowing for quicker cleanup of multiple chat threads.

## ⚠️ Disclaimer & Warnings

* **USE AT YOUR OWN RISK.** This script directly manipulates the Instagram web page's structure (DOM).
* **PRONE TO BREAKING:** Instagram frequently updates its website code. These updates **will likely break the script** over time. You may need to manually update the CSS selectors within the script by inspecting the elements in your browser if it stops working.
* **VERIFY FIRST CHAT:** Ensure the **first** conversation opened manually is indeed the one you want to start the deletion sequence with. The script will proceed from there down the list.
* **PURPOSE:** Intended solely for personal use (e.g., cleaning up your own inbox) and educational purposes.

## How it Works

When executed in the browser console on an open Instagram DM conversation page:

1.  Clicks the "Conversation information" button (icon 'i' in a circle).
2.  Waits briefly for the side panel to open.
3.  Finds and clicks the "Delete chat" (or "Supprimer la discussion") button within the panel.
4.  Waits briefly for the confirmation modal to appear.
5.  Finds and clicks the final "Delete" (or "Supprimer") button.
6.  Waits a longer duration (default 7 seconds) to allow Instagram's interface to process the deletion and refresh the chat list.
7.  Finds and clicks the **first** conversation now visible in the chat list on the left panel (assuming this is the "next" one).
8.  Repeats the process from step 1 for the newly selected conversation, up to the maximum number defined in the script (`maxLoops`).

## How to Use

1.  **Open Instagram:** Go to [www.instagram.com](https://www.instagram.com/) in your web browser (Chrome, Firefox, Edge recommended) and log in to your account.
2.  **Navigate to DMs:** Click on the "Messages" icon to open your Direct Messages (Inbox).
3.  **Select First Conversation:** **Crucially**, click on and open the **first conversation** in the list that you want to begin deleting from. The script will start with this currently open conversation.
4.  **Open Developer Console:**
    * Press `F12` on your keyboard.
    * Or, right-click anywhere on the page and select "Inspect" or "Inspect Element".
    * Find and click on the "**Console**" tab within the tools that appear.
5.  **Copy the Script:** Open the `script.js` file (or wherever you have the code) and copy its **entire** content.
6.  **Paste into Console:** Click inside the console input area (usually at the bottom, often marked with `>`), and paste the code (Ctrl+V or Cmd+V). *You might need to explicitly allow pasting if the browser warns you.*
7.  **Run the Script:** Press `Enter`.
8.  **Observe:** The script will start executing. Watch the console for log messages like "Itération 1 sur 40..." and monitor the browser window as it performs the clicks. The process will repeat for the subsequent conversations automatically. It will stop after deleting the number of chats specified in `maxLoops` or if it encounters an error (e.g., cannot find a required button).

## Configuration

* **Number of Deletions:** You can change the number of chats to delete in one run by modifying the `maxLoops` variable near the top of the script (default is `40`).
    ```javascript
    const maxLoops = 40; // Change 40 to your desired number
    ```
* **Language:** The script tries to find button text in both English ("Delete chat", "Delete") and French ("Supprimer la discussion", "Supprimer"). If your Instagram interface uses a different language, you may need to update the `targetText...` variables within the script to match the exact button labels you see.
* **Delays:** If the script runs too fast for your connection or computer, you can increase the values (in milliseconds) inside the `delay()` functions (e.g., `await delay(3000);` for 3 seconds instead of 2).

## Troubleshooting

* **"Button not found" errors:** This usually means Instagram has updated its website structure. You will need to:
    1.  Manually perform the step where the script failed (e.g., click "Info", then inspect the "Delete chat" button).
    2.  Use the Developer Tools ("Elements" or "Inspecteur" tab) to find the new CSS classes or structure of the button the script couldn't find.
    3.  Update the corresponding `document.querySelector(...)` lines in the script with the new, correct selectors. This often involves replacing the long `x1...` class names.
