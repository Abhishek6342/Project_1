# 🎓 Student Registration Form

A modern and responsive Student Registration Form using **HTML**, **CSS**, and **JavaScript**, connected to **Google Sheets** through **Google Apps Script**. This project includes everything: validation, loader, success/error messages, and even displays submitted data.

---

## 📄 What It Does

This project allows users to:

* Enter:

  * Full Name
  * Email Address
  * Phone Number (optional)
  * Date of Birth
  * Select a Course
* Validate fields before submission
* Submit form data to a Google Sheet
* Display success or error messages
* View submitted data instantly

---

## 📂 Files Included

* `main.html` – Main form file
* `style.css` – Styling for layout and responsiveness
* `download.jpg` – Logo image (optional)
* `5128380.jpg` – Background image (optional)

---

## 👨‍💼 How It Works

1. User fills out the form.
2. On submit, JavaScript validates fields.
3. Shows a loader animation.
4. Uses `fetch()` to send data to a Google Apps Script URL.
5. If successful:

   * Shows success message
   * Displays submitted data
   * Resets form
6. If failed:

   * Displays error message
   * Allows retry

---

## 🎨 JavaScript Features

```javascript
const url = 'https://script.google.com/macros/s/your-script-id/exec';
const form = document.querySelector('#form');
const loader = document.querySelector('#loader');
const result = document.querySelector('#result');
const displayData = document.querySelector('#submittedData');

form.addEventListener("submit", async (e) => {
  e.preventDefault();

  result.textContent = "";
  displayData.innerHTML = "";
  loader.style.display = "block";

  const formData = new FormData(form);
  const name = formData.get("name");
  const email = formData.get("email");
  const dob = formData.get("dob");
  const course = formData.get("course");

  if (!name || !email || !dob || !course) {
    loader.style.display = "none";
    result.textContent = "Please fill in all required fields.";
    result.style.color = "red";
    return;
  }

  try {
    const response = await fetch(url, {
      method: 'POST',
      body: formData
    });
    const text = await response.text();
    result.textContent = "Registration successful!";
    result.style.color = "green";

    displayData.innerHTML = `
      <p><strong>Name:</strong> ${name}</p>
      <p><strong>Email:</strong> ${email}</p>
      <p><strong>DOB:</strong> ${dob}</p>
      <p><strong>Course:</strong> ${course}</p>
    `;
    form.reset();
  } catch (error) {
    result.textContent = "Submission failed. Please try again.";
    result.style.color = "red";
  } finally {
    loader.style.display = "none";
  }
});
```

---

## ⚖️ Google Apps Script Backend

1. Go to [Google Apps Script](https://script.google.com)
2. Create a new project and paste this:

```javascript
function doPost(e) {
  var sheet = SpreadsheetApp.getActiveSpreadsheet().getSheetByName("Sheet1");
  sheet.appendRow([
    e.parameter.name,
    e.parameter.email,
    e.parameter.phone,
    e.parameter.dob,
    e.parameter.course
  ]);
  return ContentService.createTextOutput("Success");
}
```

3. Deploy > Manage Deployments > New Deployment
4. Choose "Web App" and set access to **Anyone**
5. Use the deployment URL in your JS file

---

## 🎨 UI Features

* Styled with CSS: rounded inputs, buttons, spacing
* Background image and logo
* Mobile responsive
* Loader animation using a simple div or spinner gif
* Messages show up below the form

---

## 📚 Tech Stack

* ✅ HTML5
* ✅ CSS3
* ✅ JavaScript (Vanilla)
* ✅ Google Apps Script
* ✅ Google Sheets

---

## 🚀 How to Use

1. Clone or download the repo
2. Replace the Google Script URL in the JS
3. Open `index.html` in a browser
4. Fill the form and see it in action

---

