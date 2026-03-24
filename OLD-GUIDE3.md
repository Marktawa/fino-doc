# How to build a Loan Management App

## Introduction
The purpose of this guide is to show you how to build a Loan Management App. A Loan Management app is useful to lending companies like Microfinance companies. It helps them manage their funds, keep track of transactions and so much more.

In emerging markets like Zambia, microfinance companies are crucial in providing financing to the working class. Typical financing options include personal loans, gadget financing, salary advances, construction financing, backup power financing and so much more. Some microfinance companies get by using spreadsheet software like Excel or Google Sheets to keep track of all their transactions. An alternative approach to spreadsheet software is a Loan Management system software.

Loan Management Apps can help in:
- Auditing (Internal and External)
- Data consistency
- Data security
- Back up system
- Tax calculation

## Key features of a Loan Management App
Loan Management apps include a list of borrowers. Borrowers are people who want to borrow money from a microfinance company. The Loan Management App must allow an Admin to add a borrower to the company's database. Key information of the Borrower includes:
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

The Loan Management App should add loans for each borrower. The Admin creates a loan associated with a borrower. Key information to include in a loan:
- Loan Number
- Release Date (Date when loan funds were disbursed to the borrower)
- Maturity Date (Date when loan is expected to be paid off)
- Principal (Loan amount given to the borrower)
- Interest Rate (Interest Rate to be charged on the Principal. Usually a percentage over a period like a year or montH)
- Interest (Interest = Principal x Interest Rate)
- Fees
- Penalty
- Amount (Amount = Principal + Interest + Fees + Penalty)
- Receipt (Option to print receipt of the repayment made. Given to the borrower)

The Loan Management App must allow an Admin to manage the total funds of the company. The Admin must be able to keep track of the Total Capital. If capital is injected into the company, the admin must be able to edit the Total Capital. Next is the Current Balance. The current balance represents the funds available to disburse right now. Next is the Total Interest. The Total Interest is the total amount of interest for each repayment. Next is Tax to be paid. We will assume that tax is paid monthly under the 5% tax regime. So that at the end of the month 5% of the Total Interest received from all loan repayments is noted down.

## Other features of a Loan Manangement App
As mentioned earlier a digital Loan Management system could also have these features:
- Audit logs
- Invoicing and receipts
- Loan statements for Borrowers
- Company Financial Statements
- Back ups for data recovery
- Admin Authentication system
- Data consistency
- Offline functionality
- Version control
- Recycle bin
- Multi Factor Authentication

## Steps to be taken in designing the Loan Management App
The procedure we will use in designing this Loan Management App is a bottom up procedure. We will create a database for the company. This database will store a table for borrowers. The admin must be able to access the list of borrowers via a web app. From there, the admin can add a new borrower, delete a borrower and edit the information for a borrower.

The next step is to add authentication to the app. The admin must be able to login using their email and password to access the app. The admin should also be able to log out of the app when needed. A table for the user will also be created in the database.

After adding authentication, the next step is to add a table for loans in the database. There should be a relation between the table for loans and the table for borrowers. A borrower can have many loans but a loan can only be assigned to one borrower. In the web app, the admin should be able to create a loan in a borrower's page. The admin should be able to view all loans associated with a borrower, edit a loan and delete a loan if needed.

Next comes a table for repayments. There should be a relation between the table for loans and the table for repayments. A loan can have many repayments but a repayment can only be for one loan. In the web app, the admin should be able to create a repayment for a loan, view all repayments, edit and delete a repayment.

Then we will create a table for company funds. This table relies on the values from the loan table and the repayments table. Each time a loan is disbursed the current balance column in the funds table decreases and the amount disbursed increases. Each time a repayment is made the current balance increases, the Total interest increases, and the Tax sum increases. Each time a tax payment is made, the Total Capital and the Current Balance decreases.

Next, we will find a way to handle the funds monthly, such that the tax to be remitted on a month to month basis is known. Maybe a table to handle the tax is needed. Where each row represents a month with columns like Total Interest for that month then the Tax to be remitted for that month.

## Prerequisites
To follow along with this guide, you will need the following:
* Knowledge of programming. Languages to know are HTML, CSS, JavaScript, Go, and SQL
* Familiarity with concepts like deploying an app, authenticating an app, testing and monitoring.
* System design concepts can be helpful in making you visualize how the app works
* Go installed on your computer workstation. [Download and Install Go using this link](https://go.dev/doc/install)
* SQLite installed on your computer workstation. 
* Knowledge of web programming concepts like Server Side Rendering, Client Side Rendering, Middleware and so on can help as well

## Hello, World Go

Go or Golang is growing in popularity as a server-side language. Developers use it as an alternative to more common technologies like PHP, Node.js, Ruby, Python, and so on. One of the appeals of Go is its low resource usage. Developers on a budget can build powerful apps and host them cheaply on low-end infrastructure if they use Go.

To get started with Go, open up your work folder and create a new file named `main.go`. Add the following code to `main.go`:

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

Congratulations! You have created your first program using Go. Pause and absorb the nice feeling. :) Now back to work

## Display a Borrower <!--Create a table for Borrowers -->

A little word about the approach we are taking. We are creating programs in the CLI using Go to help us ascertain the logic involved in the Loan App. This is a great way to wrap our heads around all the features we need to make before looking at other requirements like User Interface design or HTML interaction or HTTP methods.

Next, comment out the code you just wrote in `main.go`. At the top of your comment block, give it a ubiquitous name like "Step 1". This is good for revision purposes.

<!--In this step, we will create a table for borrowers (array or slice) and print out the details for each borrower. -->

In this step, we intend to display a list of borrowers, but we will assume that the company has only one borrower for now.

Above the comment block, update `main.go` with the following code:
```go
package main

import "fmt"

func main() {
	var borrower string = "Andrew Phiri"
	fmt.Println("Welcome to the Loan Management App\n")
	fmt.Println("Here is the list of borrowers")
	fmt.Println("Borrowers")
	fmt.Println("---------")
	fmt.Println(borrower)   
}
```

Run `main.go`:
```shell
go run main.go 
```

You should see the following output:
```txt
Welcome to the Loan Management App

Here is the list of borrowers
Borrowers
---------
Andrew Phiri
```

## Display a list of Borrowers - Name only

Next, we will display the list of borrowers, sourcing the list from an array.

Comment out the code you just wrote in `main.go`. At the top of your comment block, give it a ubiquitous name like "Step 2".

Here is how you declare an array in Go:

```go
var borrowers [2]string
var borrowers[0] = "Andrew Phiri"
var borrowers[1] = "Jane Banda"
//OR
borrowers := [2]string{"Andrew Phiri", "Jane Banda"}
```

Update `main.go` with the following code:
```go
package main

import "fmt"

func main() {
	var borrowers = [2]string{"Andrew Phiri", "Jane Banda"}
	var i int
	fmt.Println("Borrowers\n---")
	for i = 0; i < len(borrowers); i++ {
		fmt.Println(borrowers[i])
	}
}
```

You should see the following output:
```txt
Borrowers
---
Andrew Phiri
Jane Banda
```

## Display a list of Borrowers - Name and Borrower Number

We have to use slices this time. Matter of fact a slice of a slice. More details on that later.

Essentially we want to include the Borrower Number of the borrower as well. Therefore each borrower will have the name display as well as the Borrower Number.

Update `main.go` with the following code:
```go
package main

import "fmt"

var borrowers = [][]string{
	{"Andrew Phiri", "BN0001"},
	{"Jane Banda", "BN0002"},
}

func main() {
	fmt.Println("Borrowers\n-----")
	fmt.Println("Name,Borrower Number")
	var i int
	for i = 0; i < len(borrowers); i++ {
		fmt.Printf("%s,%s\n",borrowers[i][0],borrowers[i][1])
	}
}

/* //Step 3
```

Run `main.go` in your terminal:
```shell
go run main.go 
```

You should see the following output in your terminal:
```txt
Borrowers
-----
Name,Borrower Number
Andrew Phiri,BN0001
Jane Banda,BN0002
```