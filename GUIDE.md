# How to build a Loan Management App

## Introduction

The purpose of this guide is to show you how to build a Loan Management System. 

A Loan Management System is a system that is used to keep track of loans. This can also include keeping track of other details like when payments are due, the amount of interest, extra fees, penalties and so on.

A Loan Management System is useful to lending companies like Microfinance companies. It helps them manage their funds, keep track of transactions and so much more.

In emerging markets like Zambia, microfinance companies are crucial in providing financing to the working class. Typical financing options include:
* Personal loans
* Gadget financing
* Salary advances
* Construction financing
* Back up power financing

That's not an exhaustive list by any means, but it can help paint a picture of the current financing environment in emerging markets.

Some microfinance companies get by using spreadsheet software like Excel or Google Sheets as a Loan Management system. An alternative approach to spreadsheet software is a dedicated Loan Management system application.

<!--
Here are a few areas where Loan Management Apps can help:
* Auditing (Both Internal and External)
* Data consistency checks
* Data security (Encryption, Authentication)
* Back up system (Offline, Cloud-based, Data loss prevention)
* Tax calculation
-->

## Key features of a Loan Management App

When a user accesses a Loan Management app, they should be able to see a list of loans. A loan is money given to a borrower by a lender which should be returned at some point in the future. 

The key details to include in a loan record are as follows:
* Date 
* Amount
* Name of Borrower

The user must be able to create a new loan, update the information of a loan and delete a loan.

More features will be discussed as we continue with the guide.

## Prerequisites

To follow along with this guide, you will need the following:
* Knowledge of programming. Languages to know are HTML, CSS, JavaScript, and Go
* Go installed on your computer workstation. [Download and Install Go using this link](https://go.dev/doc/install)
* Python installed on your machine (or any other tool that provides a web server.)

## Steps to be taken in designing the Loan Management System

The procedure to be taken in designing the Loan Management System is a bottom up procedure. The simplest form of a Loan Management System is a list of loans with just the name of the borrower and the loan amount. This will be our starting point for the app. We will then build out features one by one as we move.

## Create Loan Management System Home page

The first step to be taken is designing the home page for the Loan Management App. The home page is the first page the user sees when accessing the system. The home page will include the title of the app. In this case, I have decided to name my app "Fino". I will come up with a fitting motto later. The home page should also have a short description of the app followed by the list of loans.

Open up your working directory in your computer's terminal. Create a file named `index.html`.
```shell
touch index.html
```

The file `index.html` is for our system's home page. Open `index.html` in your code editor and add the following code:
```html
<h1>Hello, World!</h1>
```

To test out this code, start a web server in your working directory. Use the following command in your terminal:
```shell
python -m http.server 8080
```

This will start a web server on port `8080` on your machine. This server is good enough for testing locally. For live production this kind of server is not recommended

Visit the URL: http://localhost:8080 in your browser, you should see the text "Hello, World!".
<!-- Add Screenshot -->

That was a just warm up to make sure everything is running smoothly. As mentioned earlier, we want our home page to display the title of the app not "Hello, World!". We also want to include a short description of the app in a nice welcoming message.

Update `index.html` with the following code:
```html
<h1>Fino</h1>
<p>Welcome to Fino! Your trusted Loan Management System. Manage your funds all in one neat and tidy system</p>
<h2>Loans</h2>
<p>Andrew Phiri - $500</p>
<p>Jane Banda - $1000</p>
```

Refresh the Loan Management System home page in your browser and you should see the updated text.
<!-- Add Screenshot -->

You should see the name of the app "Fino - Loan Management System" and a nice welcome message below that. Lastly, you will see the list of loans. 

<!--That completes our Loan Management System. Next we need to outline instructions on how to create, delete and edit loan entries.-->

<!--
## Add a Little Styling

This part is optional, you can skip if you like. I am just going to update the home page for Fino with some styling. Nothing fancy though.
-->

## Create Loan entry form

To create a loan entry using this system you simply need to edit the `index.html` file. Go to the list of loans denoted by `<h2>Loans</h2>`. Add a line below the heading. Add a loan entry like this: `<p>Name of person - Amount to be loaned</p>`. Replace "Name of person" and "Amount to be loaned" with sample entries e.g. `<p>George Vuvu - $10000</p>`

Of course that works but we want the user to be able to add loan entries using the web User Interface (U.I). We will add a section below the "Loans" section called "Add Loan". It will contain a form where the user can enter a name and the amount to be loaned out.

Update `index.html` with the following code:
```html
<h1>Fino</h1>
<p>Welcome to Fino! Your trusted Loan Management System. Manage your funds all in one neat and tidy system.</p>
<h2>Loans</h2>
<p>Andrew Phiri - $500</p>
<p>Jane Banda - $1000</p>
<h2>Add Loan</h2>
<form action="/post" method="GET">
	<label for="name">Name:</label>
	<input type="text" id="name" name="name" required>
	<label for="name">Amount:</label>
	<input type="text" id="name" name="name" required>
	<input type="submit" value="Submit">
</form>
```

Refresh http://localhost:8080 in your browser and you should see a section with the title "Add Loan" with a form requiring the user to enter their "Name" and the "Amount".

The form does not work. We will need to configure a backend for that. For our backend we will use Go.

## Hello, World Go

Go or Golang is growing in popularity as a server-side language. Developers use it as an alternative to more common technologies like PHP, Node.js, Ruby, Python, and so on. One of the appeals of Go is its low resource usage. Developers on a budget can build powerful apps and host them cheaply on low-end infrastructure if they use Go.

Before we incorporate Go into our Loan Management System let's get started with a bit of mental warm up.

Open up your work folder and create a new file named `main.go`. Add the following code to `main.go`:

```go
package main

import "fmt"

func main() {
	fmt.Println("Hello, World!")
}
```

Save the file and open up a new terminal session in your work directory. Run the following command:
```shell
go run main.go
```

You should see the following output:
```txt
Hello, World!
```

Now that we got Go up and running, let's move on to the next step: creating a web server using Go.

## Hello, World, Go on the Web

Now let's print out "Hello, World!" on a web page using Go. So how do we do it?

First off, comment out the code you just wrote in `main.go`. At the top of your comment block, give it a ubiquitous name liee "Step 1". Stop the python server running on port 8080 by using `CTRL` + `C` on your keyboard.

Above the comment block, add the following code:
```go
package main

import "net/http"
import "fmt"

func main() {
	http.HandleFunc("/", func(w http.ResponseWriter, r *http.Request){
		fmt.Fprintf(w, "<h1>Hello, World!</h1>")
	})

	fmt.Println("Server running at http://localhost:8080/")
	http.ListenAndServe(":8080", nil)
}
```

Save the file and run it:
```shell
go run main.go
```

Visit http://localhost:8080 in your browser and you should see the text "Hello, World!" on a webpage.
<!-- Add screenshot -->

## Serve the Loan Management System using Go web server

We want to


