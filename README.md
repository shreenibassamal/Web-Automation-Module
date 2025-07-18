# Web-Automation-Module

## Alert Module
In the context of Selenium, "Alerts" almost exclusively refers to the JavaScript-generated browser pop-ups. These are modal dialogs that are controlled by the browser's JavaScript engine, not by the HTML DOM of the web page itself. They are distinct from HTML-based modal pop-ups (which are part of the page's HTML structure).

1. Characteristics of JavaScript Alerts
Modal Nature: They are "modal," meaning they block all interaction with the underlying web page until they are dismissed. You cannot click on elements, scroll, or type anywhere on the main page until the alert is dealt with.

Browser-Controlled: Their look and feel are determined by the browser (Chrome, Firefox, Edge, Safari) and the operating system. You cannot style them with CSS.

Not in DOM: Since they are not part of the HTML Document Object Model, standard Selenium element locators (like By.id(), By.xpath(), etc.) cannot be used to interact with them.

Triggered by JavaScript: They are typically invoked by JavaScript functions like alert(), confirm(), or prompt().

2. Types of JavaScript Alerts
There are three primary types of JavaScript alerts that Selenium can handle:

a. alert() - The Simple Alert
Purpose: Displays a message to the user.

Buttons: Only an "OK" (or "Close") button.

Action: Clicking "OK" dismisses the alert.

Return Value: undefined (JavaScript).

Example (JavaScript):

JavaScript

alert("This is a simple alert message!");
b. confirm() - The Confirmation Alert
Purpose: Asks the user for confirmation (Yes/No, OK/Cancel).

Buttons: "OK" and "Cancel" buttons.

Action:

Clicking "OK" returns true.

Clicking "Cancel" returns false.

Return Value: true or false (JavaScript).

Example (JavaScript):

JavaScript

let result = confirm("Do you want to proceed?");
if (result) {
    console.log("User clicked OK");
} else {
    console.log("User clicked Cancel");
}
c. prompt() - The Input Alert
Purpose: Prompts the user to enter some text.

Buttons: "OK" and "Cancel" buttons, along with a text input field.

Action:

Clicking "OK" returns the entered text.

Clicking "Cancel" returns null.

Return Value: A string (the entered text) or null (JavaScript).

Example (JavaScript):

JavaScript

let name = prompt("Please enter your name:", "Guest");
if (name != null) {
    console.log("Hello, " + name);
} else {
    console.log("User cancelled.");
}
3. How Selenium Java Handles Alerts
Selenium provides the Alert interface to interact with these browser-level pop-ups. To get an instance of the Alert interface, you must first switch the driver's focus to the alert.

Core Methods of Alert Interface:

alert.accept(): Clicks the "OK" button on the alert. For confirm() and prompt(), this is equivalent to clicking "OK".

alert.dismiss(): Clicks the "Cancel" button on the alert. For simple alert(), this usually acts like "OK" because there's no "Cancel" option.

alert.getText(): Retrieves the message text displayed in the alert.

alert.sendKeys(String keysToSend): Enters text into the input field of a prompt() alert.

Steps to Handle Alerts in Selenium Java:

Trigger the Alert: Perform the action (e.g., click a button) that causes the JavaScript alert to appear.

Wait for the Alert: It's crucial to wait for the alert to be present before trying to interact with it. If you don't, you might get a NoAlertPresentException.

Switch to the Alert: Use driver.switchTo().alert() to transfer the Selenium WebDriver's focus from the main web page to the alert dialog.

Perform Actions: Use the methods of the Alert interface (accept(), dismiss(), getText(), sendKeys()) to interact with the alert.

Return Control (Implicitly): Once you accept() or dismiss() the alert, control is automatically returned to the main web page. There's no explicit driver.switchTo().defaultContent() needed for alerts.

5. Common Issues and Solutions
NoAlertPresentException:

Cause: Selenium tried to switch to an alert, but no alert was present. This often happens if the action didn't trigger the alert, or if the test executed too quickly before the alert appeared.

Solution: Always use WebDriverWait with ExpectedConditions.alertIsPresent() before attempting to switch to or interact with the alert.

UnhandledAlertException:

Cause: An alert appeared, and Selenium didn't switch to it and dismiss it. Subsequent Selenium commands will fail because the alert is blocking interaction with the main page.

Solution: Ensure you always handle any expected alerts immediately after they are triggered. If alerts appear unexpectedly, your test logic needs to be more robust to handle them, potentially by wrapping actions in a try-catch block that specifically catches this exception and dismisses the alert.

Alert Text Verification:

Issue: Sometimes the exact text of an alert might vary slightly (e.g., due to locale).

Solution: Instead of strict equals(), use contains() for more robust checks: alert.getText().contains("expected part of text").