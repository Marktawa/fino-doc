# How to build a Loan Management App

## Introduction
The purpose of this guide is to show you how to build a Loan Management System. A Loan Management System is useful to lending companies like Microfinance companies. It helps them manage their funds, keep track of transacations and so much more.

In emerging markets like Zambia, microfinance companies are crucial in providing financing to the working class. Typical financing options include:
* Personal loans
* Gadget financing
* Salary advances
* Construction financing
* Back up power financing

That's not an exhaustive list by any means, but it can help paint a picture of the current financing environment in emerging markets.

Some microfinance companies get by using spreadsheet software like Excel or Google Sheets to keep track of all their transactions. An alternative approach to spreadsheet software is a Loan Management system software

Here are a few areas where Loan Management Apps can help:
* Auditing (Both Internal and External)
* Data consistency checks
* Data security (Encryption, Authentication)
* Back up system (Offline, Cloud based storage)
* Tax calculation

## Key features of a Loan Management App
When a user logs in to access a Loan Management app, they should be able to see a list of borrowers. Borrowers are people who have borrowed money from the lending company. The Loan Management system must allow the logged in user (the admin) to add a borrower who wants funds from the company. The typical information of the Borrower includes:
- Borrower Number
- Name
- Date of Birth
- Email
- Phone Number
- Address
- ID Number
- Company
- Bank
- Create Date
<!-- Add diagram -->

The Loan Management system should add loans for each borrower. The admin creates a loan associated with a borrower. Key information to include when adding a loan is:
- Loan Number
- Release Date (Date when loan funds were disbursed to the borrower)
- Maturity Date (Date when loan is expected to be paid off)
- Principal (Loan amount given to the borrower)
- Interest Rate (Interest Rate to be charged on the Principal. Usually a percentage over a period like a year of month)
- Interest (Interest = Principal x Interest Rate)
- Fees
- Penalty
- Due (Total Amount of Money that must be repaid by the Borrower, Due = Principal + Interest + Fees + Penalty)
- Paid (Total Amount of Money that the Borrower has paid back so far)
- Balance (Balance = Due - Paid)
- Status (Status of the loan. Is it Current or is it Paid?)
<!-- Add diagram -->

The Loan Management System must allow an Admin to add Repayments. Each Repayment made is associated with a Loan. The key information to include in a Repayment is as follows:
- Collection Date
- Payment Method
- Principal
- Interest
- Fees
- Penalty
- Amount (Amount = Principal + Interest + Fees + Penalty)
- Receipt(Option to print a receipt for each repayment made. Given to the borrower)
<!-- Add diagram -->

The Loan Management App must allow an Admin to manage the total funds of the company. The Admin must be able to keep track of the Total Capital. If capital is injected into the company, the admin must be able to edit the Total Capital. Next is the Current Balance. The Current Balance represents the funds available to disburse right now. Next is the Total Interest. The Total Interest is the total amount of interest from each repayment. Next is Tax to be paid. We will assume that tax is paid monthly under the 5% tax regime. So at the end of each month, 5% of the Total Interest received from all loan repayments is noted down.

## Other Features of a Loan Management App
Here are some other features that should be considered for digital Loan Management System:
* Audit logs 
* Invoicing and Receipts
* Loan Statements for Borrowers
* Company Financial Statements
* Back ups for data recovery
* Admin Authentication system
* Data consistency (checksums, quality control)
* Offline functionality (airgapped systems)
* Version control
* Recycle bin
* Multi Factor Authentication 

## Prerequisites
To follow along with this guide, you will need the following:
* Knowledge of programming. Languages to know are HTML, CSS, and JavaScript
* Python installed on your machine. (or any other tool that provides a web server)

## Steps to be taken in designing the Loan Management System
The procedure to be used in designing the Loan Management App is a bottom up procedure. This is where we start with a simple definition of how a practical minimum viable product (M.V.P) of a Loan Management System. Prior to that, we at least need to come up with a simple demo product. The difference between a demo product and an MVP is that a demo product is not for live use while an MVP is for live use.

For the demo, the simplest approach would be to use HTML and CSS with hardcoded values for illustration purposes. We don't have wreck our brains yet with complex concepts like business logic, databases, APIs, HTTP and so on. We are just using plain basic HTML to help visualize the workflows and user stories. The HTML is for the hardcoded content or text that appears, the CSS will define a minimal visual structure that helps represent the content or data effectively. My guess is the CSS design to be implemented will be one with a side bar targeted for desktop users. Most typical Loan Management systems have such a look. Thus we don't care about color palettes, fonts, hover effects, animations, box shadows, gradients, and so on. This is CSS is there to help us visualize and help with effective prototyping that can help with iterating quickly when interfacing with a client.

We can provisionally use this short description as map to follow. More steps will be revealed as we continue with the project.

## Create Loan Management System Home page
The first step to be taken is designing the home page. The home page is the first page that the admin sees when accessing the system. The home page will include the title of the app. In this case, I have decided to name my app "Fino". I will come up with a befitting motto later. The home page should also have a short description of the app and a link to visit the login page.

Open up your working directory on your computer and open the terminal. Create a file named `index.html`:
```shell
touch index.html
```

The file `index.html` is for our system's home page. Open `index.html` in your code editor and add the following code:
```html
<!DOCTYPE html>
<html lang="en">
<head>
	<meta charset="UTF-8">
	<meta name="viewport" content="width=device-width, initial-scale=1.0">
	<title>Fino - Loan Management System</title>
</head>
<body>
	<h1>Hello, World!</h1>
</body>
</html>
```

For now we are only concerned with the text that will sit between the opening and closing `body` tags.

To test out this code, start a web server in your working directory. Use the following command in your terminal:
```shell
python -m http.server 8080
```

This will start a web server on port 8080 on your machine.

Visit the URL: http://localhost:8080 in your browser, you should see the text "Hello, World!".
<!-- Add Screenshot -->

As mentioned earlier, we want our home page to display the title of the app not "Hello, World!". We also want to include a short description of the app in a nice welcoming message.

Update `index.html` with the following code:
```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Fino - Loan Management App</title>
</head>
<body>
    <h1>Fino - Loan Management System</h1>
    <h2>Welcome to Fino!</h2>
    <p>Your trusted Loan Management App. Manage your funds all in one neat and tidy system.</p>
</body>
</html>
```

Refresh the Loan Management System home page in your browser and you should see the updated text.
<!-- Add Screenshot -->

## Create Loan Management System Login page
Next we will create a login page for our system. This is where a user will enter a username and a password to access the system. What we need to display is the title of the app first, then comes the title of the page. Maybe "Login to Access System" or "Admin Login". We will see. A short description that informs the user to enter their username and password will follow. Below that, a we will place a form that will receive the inputs and a login button. The login button should take the user to the page displaying the list of borrowers on success.

Create a new file named `login.html` 
```shell
touch login.html
```

And add the following code to `login.html`:
```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Login - Fino - Loan Management App</title>
</head>
<body>
    <h1>Fino - Loan Management System</h1>
    <h2>Admin Login</h2>
    <p>Please enter your username and password to access the system.</p>
    <form>
    	<label>Username:</label>
    	<input type="text" />
    	<label>Password:</label>
    	<input type="password" />
    </form>
    <button type="submit"><a href="index.html">Log in</a></button>
</body>
</html>
```

Next update the home page `index.html` with a link to the login page:
```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Fino - Loan Management App</title>
</head>
<body>
    <h1>Fino - Loan Management System</h1>
    <h2>Welcome to Fino!</h2>
    <p>Your trusted Loan Management App. Manage your funds all in one neat and tidy system.</p>
    <a href="login.html">Click here to Log in</a>
</body>
</html>
```

In your browser refresh the home page, you should see a link to the login page written as "Click here to Log in".
<!-- Add Screenshot -->

Click the link and you should see the login page. 
<!-- Add Screenshot -->

If you click the "Log in" button, you should be taken back to the home page. We don't want that behaviour. To fix that, we will create the authenticated landing page. More in the next step.

## Create Loan Management System Logged in Landing page

In this step we will create a page called `admin.html`. This is the landing page where the admin is taken after successfully logging in. You could think of it as a dashboard landing page. In future it will contain links to the list of borrowers, links to the list of loans and so much more.

For now, we just want it to act as confirmation of successful log in. A welcome message would be appropriate.

Create a new file named `admin.html`:
```shell
touch admin.html
```

Add the following code to `admin.html`:
```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Admin - Fino - Loan Management App</title>
</head>
<body>
    <h1>Fino - Loan Management System</h1>
    <h2>Welcome Admin to Fino</h2>
    <p>Your trusted Loan Management App. Manage your funds all in one neat and tidy system.</p>
    <a href="/">Click here to Log out</a>
</body>
</html>
```

Update the `login.html` file with the link to `admin.html`:
```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Login - Fino - Loan Management App</title>
</head>
<body>
    <h1>Fino - Loan Management System</h1>
    <h2>Admin Login</h2>
    <p>Please enter your username and password to access the system.</p>
    <form>
    	<label>Username:</label>
    	<input type="text" />
    	<label>Password:</label>
    	<input type="password" />
    </form>
    <a href="/admin.html"><button type="submit">Log in</button></a>
</body>
</html>
```

Now refresh your browser and visit http://localhost:8080/login.html. Click the "Log in" button. It should take you to the Admin landing page http://localhost:8080/admin.html confirming successful login.
<!-- Add screenshot -->

## Create Loan Management System Borrower list page

In this step we will create a page called `borrowers.html`. This page will contain a list of borrowers.

Here is the information we want to include for each borrower in the list:
* Borrower Number
* Name
* Create Date

Create a new file named `borrowers.html`:
```shell
touch borrowers.html
```

Add the following code to `borrowers.html`:
```html
<!DOCTYPE html>
<html lang="en">
<head>
	<meta charset="UTF-8">
	<meta name="viewport" content="width=device-width, initial-scale=1.0">
	<title>Borrowers - Fino - Loan Management System</title>
</head>
<body>
	<h1>Fino - Loan Management System</h1>
	<h2>Borrower List</h2>
	<p>Here is a list of borrowers who have received loans.</p>
	<h3>BN0001</h3>
	<p>Andrew Phiri</p>
	<p>Creation Date: 01 March 2026</p>
	<hr>
	<h3>BN0002</h3>
	<p>Jane Banda</p>
	<p>Creation Date: 02 March 2026</p>
	<hr>
	<h3>BN0003</h3>
	<p>Mutinta Sitali</p>
	<p>Creation Date: 03 March 2026</p>
	<hr>
	<h3>BN0004</h3>
	<p>Mwansa Chilufya</p>
	<p>Creation Date: 04 March 2026</p>
</body>
</html>
```


