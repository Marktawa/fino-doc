# Fino
Fino is a Finance Management app for Microfinance companies. It helps manage funds seamlessly

## Approach
The approach we will adopt in creating Fino the Loan Management App involves a lot of things. Firstly, it will  involve writing a `GUIDE.md` file which will act as a step by step guide detailing how to build the Loan Management App. It will include everything I need to think about the rationale behind decisions. This guide is going to be written in such a way that a beginner can follow along and gain something from it.

My guess right now is that I will take a bottom up approach to build the Loan Management App. The first phase is to come up with something that is good enough for a demo. In this case hardcoded HTML is good enough. No business logic, no databases, no APIs, no HTTP methods, just hardcoded HTML to help visualize the workflows. After the HTML then comes the CSS. The CSS is just a minimal, structural CSS to help represent the data effectively. My guess is the CSS design to be implemented will be one with a side bar targeted for desktop users. Thus we don't care about color palettes, fonts, hover effects, animations, box shadows, gradients and so on. This CSS is there to help us visualize and help with effective prototyping for the client.

Considerations will be made on publishing this guide daily like chapters from a light novel to the LinkedIn and Facebook community. Of course, I may publish on Firebase using a developer blog documentation framework like Astro Starlight. More on that later.

---

## Rough points
Financial Management System

- Tax (Deduct tax from interest)
- Deductions
- Client Management Process
- Pending, Paid, In Arrears

## Starting point

- Hello, World! Nuxt
- Create client
- `Loan Management App`: `Hello Admin`
- `Create client`
- `Read clients`
- Enter client name
- Enter client email
- Enter requested amount
- Enter approval status (pending, rejected, approved)
- Amount given
- Amount due
- Save

## Date field for Client collection
Consider including a field for dates in the Client collection. Might be multiple dates. Like the date when the loan was disbursed. The due date for payment and many other potential date fields to deal with the complexity of managing loan funds

## Unique Identity Number field for Client collection
Later on include a `UID` number for each client. Do not use the `documentID` most likely the Loan Management company has one for each client. It cannot be the NRC since two people can have the same NRC number and it cannot be the TPIN as not everyone has one and it's for ZRA purposes. But I will consider including them in the fields for a client.


## Starting point

Finance company has a balance say 100,000. Someone comes and asks for a loan for 10,000. to be paid back in 1 month as 15,000. Loan company approves loan. They give client 10,000. They remain with 90,000. After 1 month, they receive 15,000 from the client. The company's balance becomes 105,000.

Company creates an account for client. Client requests for 10,000 loan amount. Loan is approved. Client is given 10,000. Amount is deducted from Total money not yet given out. Amount is increased for Total money given out. Total funds remains the same. total money received remains the same.

App is managed by the admin. Admin creates an account for a client. Within the client's page, you can see the amount owed, the total amount they borrowed, how much they have paid so far, the interest charged.

There has to be a list of clients. Within each client's page you will then see their information

In the company account page, the admin sees the total balance. Total money not yet given out. Total money given out. Total money received. Total money owed. Tax amount to pay.

Starting point: Create a client. We just want name. 

