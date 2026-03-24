# How to build a Loan Management App

## Introduction
The purpose of this guide is to show you how to build a Loan Management App. A Loan Management app is useful to Microfinance companies. It helps them manage their funds, keep track of transactions and so much more. 

In emerging markets like Zambia, microfinance companies are crucial in providing financing to the working class. Typical financing includes personal loans, gadget financing, salary advances, construction financing, backup power financing and so much more. Some microfinance companies get by using spreadsheet software like Excel or Google Sheets to keep track of all their transactions. An alternative to spreadsheet software is a Loan Management App.

Loan Management Apps can help in:
- Auditing (Internal and External)
- Data Consistency
- Data Security
- Back up system
- Tax calculation

## Key features of a Loan Management App
Loan Management Apps include a list of Borrowers. Borrowers are people who want to borrow money from the Microfinance company. The Loan Management app must allow an Admin to add a borrower to the company's database. Key information of the Borrower includes:
- Name
- Email
- Phone Number
- Address
- ID Number
- Company
- Bank name
- Unique Identifier Number (UID) (Each Borrower must have a unique number for internal record keeping)
- Date of Account Creation (Create Date)
- Date of Birth

The ability to add Loans. Each borrower has a loan associated with them. The Loan Management App must allow the admin to add a loan associated with a borrower. Key information to include in a Loan:
- Loan Number
- Release Date (Date when loan funds were disbursed to borrower)
- Maturity Date (Date when loan is expected to be paid off)
- Principal (Loan Amount given to borrower)
- Interest Rate (Interest Rate to be charged on the Principal. Usually a percentage and annual)
- Interest (Interest = Principal x Interest Rate)
- Fees 
- Penalty
- Due (Total Amount of Money that must be repaid by the Borrower = Principal + Interest + Fees + Penalty)
- Paid (Total Amount of Money that the Borrower has paid so far)
- Balance (Balance = Due - Paid)
- Status (The status of the loan. Either Active or Closed)

The Loan Management App must allow an Admin to add Repayments. Each Loan must include a repayment. Key information to include in a Repayment:
- Collection Date
- Payment Method
- Principal
- Interest
- Fees
- Penalty
- Amount (Amount = Principal + Interest + Fees + Penalty)
- Receipt (Option to print receipt of the repayment made. Given to the borrower)

The Loan Management App must allow an Admin to manage the total funds of the company. The Admin must be able to keep track of the Total Capital. If capital is injected into the company, the admin must be able to edit the Total Capital. Next is the Current Balance. The current balance represents the funds available to disburse right now. Next is the Total Interest. The Total Interest is the total amount of interest from each repayment. Next is Tax to be paid. We will assume that tax is paid monthly under the 5% tax regime. So at the end of each month 5% of the Total Interest received from all loan repayments is noted down.

## Other Features of Loan Management App
As mentioned earlier a digital Loan Management system could also have these features:
- Audit logs (if system has multiple admins)
- Invoicing and Receipts
- Loan Statements for Borrowers
- Company Financial Statements
- Back ups for data recovery
- Admin Authentication system
- Data consistency
- Offline functionality
- Version control
- Recycle bin
- Multi Factor Authentication

## Steps to be taken to in designing the Loan Management App
The procedure we will use in designing this Loan Management App is a bottom up procedure. We will create a database for the company. Firstly, the database will store a table for borrowers. Next, the admin must be able to access the list of borrowers via a web app. From there, the admin can add a new borrower, delete a borrower and edit the information for a borrower.

The next step is to add authentication to the app. The admin must be able to login using their email and password to access the app. The admin should also be able to log out of the app when needed. A table for the user will also be created in the database.

After adding authentication, the next step is to add a table for loans in the database. There should be a relation between the table for loans and the table for borrowers. A borrower can have many loans but a loan can only be assigned to one borrower. In the web app, the admin should be able to create a loan in a borrower's page. The admin should be able to view all loans associated with a borrower, edit a loan and delete a loan if needed.

Next comes adding a table for repayments. There should be a relation between the table for loans and the table for repayments. A loan can have many repayments but a repayment can only be for one loan. In the web app, the admin should be able to create a repayment for a loan, view all repayments, edit and delete a repayment.

Then we will create a table for company funds. This table relies on the values from the loan table and the repayments table. Each time a loan is disbursed the current balance column in the funds table decreases and the amount disbursed increases. Each time a repayment is made the current balance increases, the total interest increases, and the tax sum increases. Each time a tax payment is made, the Total Capital and the Current Balance decreases.

Next, we will find a way to handle the funds monthly, such that the tax to be remitted on a month to month basis is known. Maybe a table to handle the tax is needed. Where each row represents a month with columns like Total Interest for that month then the Tax to be remitted for that month.

## Prerequisites
To follow along with this guide, you will need the following:
* Knowledge of programming. Languages to know are HTML, CSS, JavaScript, and SQL
* Familiarity with concepts like deploying an app, authenticating an app, testing, and monitoring
* System design concepts can be helpful in making you visualize how the app works
* Node.js installed on your machine. The latest LTS version to be precise.
* Knowledge of Strapi the Content Management System we will use to build the app. A Quickstart guide will suffice.
* Knowledge of Vue.js and its meta-framework Nuxt is helpful.
* Web concepts like Server Side Rendering, Client Side Rendering, and Middleware can help as well.

## Install Strapi
Open up your working directory on your computer workstation in your terminal. Install Strapi using the following command:
```shell
npx create-strapi-app@latest backend \
 --skip-cloud \
 --skip-db \
 --no-example \
 --js \
 --use-npm \
 --install \
 --no-git-init \
```

This command will create a folder named `backend` where your Strapi app will sit.

## Create an Admin user for the Strapi app
Change directories to the `backend` directory where Strapi is installed:
```shell
cd backend
```

Run the following command to create an admin user. 
```shell
npm run strapi admin:create-user -- --firstname=Kai --lastname=Doe --email=chef@strapi.io --password=Gourmet1234
```
Take note of the email `chef@strapi.io` and the password `Gourmet1234`. We will need these later when we want to log in to Strapi.

## Log in to your Strapi app
Start your Strapi server in development mode using the following command:
```shell
npm run develop
```

Open your browser and visit the admin page at `http://localhost:1337/admin`. Login using the credentials we took note of earlier. The email is `chef@strapi.io` and the password is `Gourmet1234`

![Strapi Admin Dashboard](https://res.cloudinary.com/craigsims808/image/upload/v1750250272/strapi/sasn/basic-strapi-overview_fhjixd.png)

## Basic overview of Strapi Admin Dashboard
Strapi as mentioned earlier is a Content Management System. It provides us with the database and the tools to create the business logic for our database. It also helps in building APIs to help access the database using a web app for example.

<!-- Add Screenshot -->
Strapi's Admin Dashboard has the following features:
- Content Manager: For adding entries to collections (tables) for our database. Entries can be thought of as rows in a table for a database.
- Content-Type Builder: For adding collections for our database. This includes all the necessary configurations (schema) that match with our use case. Collections can be thought of as tables in a database.
- Media Library Plugin: For adding media or file uploads to our Strapi app.
- Settings: For configuring authentication, email providers and so many other configurations to make the Strapi app work the way you want.
- Marketplace: For adding plugins to your Strapi app. Plugins made by third parties. This is for features that are not found in the default Strapi app.

Now that we got some idea of what the Strapi app is all about, let's move on to creating collections for our Loan Management App.

## Create Borrower collection
Open the **Content-Type Builder** to create a `Borrower` collection.This is for storing borrower entries in our database.
<!-- Add Screenshot -->

Click **+** next to **COLLECTION TYPES** and enter `Borrower` then select **Add fields**. The fields to be added to the `Borrower` collection are as follows:
- A Text field (Long) called `borrower_number`.
- A Text field (Long) called `name`.
- A Text field (Long) called `email`.
- A Text field (Long) called `phone_number`.
- A Text field (Long) called `address`.
- A Text field (Long) called `id_number`.
- A Text field (Long) called `company`.
- A Text field (Long) called `bank`.
- A Text field (Long) called `creation_date`.
- A Text field (Long) called `date_of_birth`.
<!--Add Screenshot-->

After adding the fields click **Save** and wait for your Strapi server to restart.

## Add Permissions for Borrower collection
We need to add permissions to be able to read, create, delete and update entries in the Borrower collection. For now we will configure Public permissions, just to see how it looks.

Go to **Settings** in your Strapi Dashboard. Click **Roles** under **Permissions**. Select **Public** in the modal that appears.
<!-- Add Screenshot -->

Scroll down to the **Borrower** collection Public user permissions. Click **Select all** then click **Save**. 
<!-- Add Screenshot -->

This means any public user can access the Borrower API then read the list of borrowers, create a borrower, delete a borrower and update the information for a borrower.

## Add sample entries for Borrower collection

Click the **Content Manager** icon in the Strapi Dashboard. Select the **Borrower** collection. Here we want to add sample entries for the **Borrower** collection. These sample entries are for testing purposes.
<!-- Add Screenshot -->

Click **Create entry** then fill in the details for each borrower. Use the following example below as guidance:

```json
{
    "borrower_number": "BN0001",
    "name": "Andrew Phiri",
    "email": "andrewphiri@email.com",
    "phone_number": "+260 970 000 001",
    "address": "1 Kaunda St, Lusaka, Zambia",
    "id_number": "00 00 00 00 1",
    "company": "Zamgold",
    "bank": "Zanaco",
    "creation_date": "01 March 2026",
    "date_of_birth": "01 January 2000"
}
```

Add 3 or so entries to play around with.
<!-- Add Screenshot -->

## Test API

Now that we have our Borrower collection populated with entries, it's time to test the API and see if we can read, create, update and delete Borrowers.

### Test `find`
Open your terminal and run the followng command to test if we can read the list of Borrowers we just created:
```shell
curl -X GET http://localhost:1337/api/borrowers \
-H "Content-Type: application/json"
```

For your result you should see a list of borrowers in json file format as seen below:
```json
{
    "data": [
        {
            "id": 1,
            "documentId": "p9qth3ydp8segnkulfrjd0fv",
            "borrower_number": "BN0001",
            "name": "Andrew Phiri",
            "email": "andrewphiri@email.com",
            "phone_number": "+260 970 000 001",
            "address": "1 Kaunda St, Lusaka, Zambia",
            "id_number": "00 00 00 00 1",
            "company": "Zamgold",
            "bank": "Zanaco",
            "creation_date": "01 March 2026",
            "date_of_birth": "01 January 2000",
            "createdAt": "2025-09-23T18:27:45.292Z",
            "updatedAt": "2025-09-23T18:27:45.292Z",
            "publishedAt": "2025-09-23T18:27:45.287Z"
        },
        {
            "id": 2,
            "documentId": "qxls4t0s7tnyq42vhm4z7zxl",
            "borrower_number": "BN0002",
            "name": "Jane Banda",
            "email": "janebanda@email.com",
            "phone_number": "+260 970 000 002",
            "address": "2 Kaunda St, Lusaka, Zambia",
            "id_number": "00 00 00 00 2",
            "company": "Zamsilver",
            "bank": "Stanbic Bank",
            "creation_date": "02 March 2026",
            "date_of_birth": "02 January 2000",
            "createdAt": "2025-09-23T18:28:14.746Z",
            "updatedAt": "2025-09-23T18:28:14.746Z",
            "publishedAt": "2025-09-23T18:28:14.744Z"
        }
    ],
    "meta": {
        "pagination": {
            "page": 1,
            "pageSize": 25,
            "pageCount": 1,
            "total": 2
        }
    }
}    
```

### Test `findOne`

Now that we can read the list of borrowers, we should be able to retrieve one entry from the Borrower collection. We can use this command in the terminal:
```shell
curl -X GET http://localhost:1337/api/borrowers/p9qth3ydp8segnkulfrjd0fv \
-H "Content-Type: application/json"
```

For your result, you should see the requested borrower entry data in json file format as seen below:
```json
{
    "data": {
        "id": 1,
        "documentId": "p9qth3ydp8segnkulfrjd0fv",
        "borrower_number": "BN0001",
        "name": "Andrew Phiri",
        "email": "andrewphiri@email.com",
        "phone_number": "+260 970 000 001",
        "address": "1 Kaunda St, Lusaka, Zambia",
        "id_number": "00 00 00 00 1",
        "company": "Zamgold",
        "bank": "Zanaco",
        "creation_date": "01 March 2026",
        "date_of_birth": "01 January 2000",
        "createdAt": "2025-09-23T18:27:45.292Z",
        "updatedAt": "2025-09-23T18:27:45.292Z",
        "publishedAt": "2025-09-23T18:27:45.287Z"
    },
    "meta": {}
}
```

### Test `create`

Next, we want to test the `create` method to determine if we can create a borrower remotely. Under the hood the `create` method is the HTTP `POST` method which will send a `POST` request to the `borrower` API.

Run the following command in your terminal:
```shell
curl -X POST http://localhost:1337/api/borrowers \
-H "Content-Type: application/json" \
-d '{
        "data": {
            "borrower_number": "BN0002",
            "name": "Mutinta Sitali",
            "email": "mutintasitali@email.com",
            "phone_number": "+260 970 000 003",
            "address": "3 Kaunda St, Lusaka, Zambia",
            "id_number": "00 00 00 00 3",
            "company": "Zambronze",
            "bank": "Absa",
            "creation_date": "03 March 2026",
            "date_of_birth": "03 January 2000",
        }
    }'
```

The result is confirmation of the creation of a borrower added to the list of borrowers in your Strapi backend. You should see the details of the borrower you just created plus other Strapi specific fields like `id`, `documentId`, `createdAt`, `updatedAt`, and `publishedAt` values. The result shows the borrower in json file format as seen below:

```json
{
    "data": {
        "id": 3,
        "documentId": "v8an9b0iav2ia1f8si2d5b16",
        "name": "Mutinta Sitali",
        "email": "mutintasitali@email.com",
        "phone_number": "+260 970 000 003",
        "address": "3 Kaunda St, Lusaka, Zambia",
        "id_number": "00 00 00 00 3",
        "company": "Zambronze",
        "bank": "Absa",
        "creation_date": "03 March 2026",
        "date_of_birth": "03 January 2000",
        "createdAt": "2025-09-23T19:10:44.740Z",
        "updatedAt": "2025-09-23T19:10:44.740Z",
        "publishedAt": "2025-09-23T19:10:44.738Z"
    },
    "meta": {}
}
```

### Test `delete`

The `delete` method is used to delete an entry in a collection. The Strapi backend needs the `documentId` of the entry to be deleted. To test whether we can delete entries in our Borrower collection, we need to run the following command:

```shell
curl -X DELETE http://localhost:1337/api/borrowers/ia1f8si2d5b16
```

The result you will get is a HTTP `204` code from the Strapi server. This confirms that the entry has been deleted.

### Test `update`

The `update` method is used to update an entry's fields. To test whether we can update an entry, we pick an entry and select the fields we want to update like `email` or `address`. Once again, the Strapi server will need the entry's `documentId`.

Run the following command in your terminal to update a Borrower entry's fields
```shell
curl -X PUT http://localhost:1337/api/borrowers/sa0dnznt6xr78qprs07dkds3 \
-H "Content-Type: application/json" \
-d '{
      "data": {
        "address": "4 Chiluba Street, Lusaka, Zambia"
      }
    }'
```

The result  you will get is a json payload showing all the fields for the entry you have just updated
```json
{
    "data": {
        "id": 4,
        "documentId": "sa0dnznt6xr78qprs07dkds3",
        "borrower_number": "BN0004",
        "name": "Mwansa Chilufya",
        "email": "mwansachilufya@email.com",
        "phone_number": "+260 970 000 004",
        "address": "4 Chiluba St, Lusaka, Zambia",
        "id_number": "00 00 00 00 4",
        "company": "Zamdiamond",
        "bank": "Capital Bank",
        "creation_date": "04 March 2026",
        "date_of_birth": "04 January 2000",
        "createdAt": "2025-09-23T19:08:31.874Z",
        "updatedAt": "2025-09-23T19:16:25.917Z",
        "publishedAt": "2025-09-23T19:16:25.915Z"
    },
    "meta": {}
}
```

That's it with regards to testing the `borrower` API we just created. Next, we need to create the Nuxt frontend app. This is the web app that will interface with the Strapi backend server to display the list of borrowers and so on and so forth.

## Download and Install Nuxt

So what is Nuxt? Nuxt is a meta-framwework for a JavaScript frontend library called Vue.js. It is meant to provide a framework for building web apps through opinionated configurations like composables, methods and so on.

On your local machine, create a new folder named `frontend`. This will be your Nuxt project directory.
```shell
mkdir frontend
```

Open `frontend`.
```shell
cd frontend
```

Create a `package.json` file inside `frontend`.
```shell
touch package.json
```

Add the following to `package.json`:
```json
{
  "scripts": {
    "build": "nuxt build",
    "dev": "nuxt dev",
    "generate": "nuxt generate",
    "preview": "nuxt preview",
    "postinstall": "nuxt prepare",
    "start": "node .output/server/index.mjs"
  },
  "dependencies": {
    "nuxt": "latest",
    "vue": "latest"
  }
}
```

Install the dependencies listed in the `package.json` file by running the following command:
```shell
npm install
```

## Create a Hello World Nuxt app

To warm up our brains we will start off by writing a simple program that prints the text "Hello, World!" when you visit the web page in the browser. This confirms to us that Nuxt installed successfully and is working as expected.

Create a `pages` folder:
```shell
mkdir pages
```

Create an `index.vue` file inside the `pages` folder:
```shell
touch pages/index.vue
```

Add this code:
```vue
<template>
    <h1>Hello, World!</h1>
</template>
```

Run the Nuxt development server using the following command:
```shell
npm run dev
```

Visit `http://localhost:3000` in your browser to view your site. You should see a web page with the text "Hello, World!".
![Nuxt page Hello World](https://res.cloudinary.com/craigsims808/image/upload/v1751151422/strapi/getting-started-with-nuxt/static-hello-world-web-page_rjkxnm.png)

## Create Loan Management App Home page

Now that we know our Nuxt frontend is working as expected, let's create the home page for our Loan Management App. The home page will show the title of the app "Loan Management App". Let's give it a name shall we? I think "Fino" works so the full title for the app is "Fino - Loan Management App"

Update the `pages/index.vue` file in your code editor with the following code:
```vue
<template>
    <h1>Fino - Loan Management App</h1>
    <p>Welcome to Fino! Your trusted Loan Management App</p>
    <a href="/borrowers">Click here to view your borrowers</a>
</template>
```

Run the Nuxt development server using the following command:
```shell
npm run dev
```

Visit `http://localhost:3000` in your browser to view your updated home page.
<!-- Add Screenshot -->

## Create page to view your list of Borrowers

The first thing we will tackle to confirm if the connection between our Nuxt frontend web app and the Strapi backend server is ok. We will do this by retrieving the list of borrowers from the Strapi backend then display them on the Nuxt frontend web app.

In your Nuxt `frontend` folder, create a new folder named `borrowers` right inside the `pages` folder:

```shell
mkdir pages/borrowers
```

Inside the `pages/borrowers` directory, create a new file named `index.vue`:
```shell
touch pages/borrowers/index.vue
```

Add the following code to `pages/borrowers/index.vue`:
```vue
<script setup>
    const { data: borrowers } = await useFetch('http://localhost:1337/api/borrowers')
</script>

<template>
    <h1>Fino - Loan Management App</h1>
    <h2>Borrower List</h2>
    <ul>
        <li v-for="borrower in borrowers.data">{{ borrower.borrow_number }} - {{ borrower.name }}</li>
    </ul>
</template>
```

The `v-for` Vue.js method will loop through every borrower retrieved from the Strapi API then print out the `borrower_number` and the `borrower_name`.

Run the Nuxt development server:
```shell
npm run dev
```

Visit `http://localhost:3000/borrowers` in your browser to view your Borrower list page.
<!-- Add Screenshot -->

You should see a list of borrowers. Just the `borrower_number` and `name`. This confirms that we can access the Strapi backend using the Nuxt frontend web app.


## Create an Individual Borrower page

An individual page to view the details about a Borrower is the next thing we will create.

Inside the `pages/borrowers` directory, create a new file named `[id].vue`:

```shell
touch pages/borrowers/[id].vue
```

Add the following code to `pages/borrowers/[id].vue`:
```vue
<script setup>
  const route = useRoute()
  const { data: borrower } = await useFetch(`http://localhost:1337/api/borrowers/${route.params.id}`)
</script>

<template>
  <h1>Fino - Loan Management App</h1>
  <h2>{{ borrower.data.borrower_number }}</h2>
  <p>Name: {{ borrower.data.name }}</p>
  <p>D.O.B: {{ borrower.data.date_of_birth }}</p>
  <p>Email: {{ borrower.data.email }}</p>
  <p>Phone Number: {{ borrower.data.phone_number }}</p>
  <p>Address: {{ borrower.data.address }}</p>
  <p>ID Number: {{ borrower.data.id_number }}</p>
  <p>Company: {{ borrower.data.company }}</p>
  <p>Create Date: {{ borrower.data.creation_data }}</p>
  <NuxtLink to="/borrowers">Back to Borrower List</NuxtLink>
</template>
```

Update the Borrower list page to add links for each individual borrower. This way we can click on a borrower and then access their page.


Update `pages/borrowers/index.vue` with the following code:
```vue
<script setup>
  const { data: borrowers } = await useFetch(`http://localhost:1337/api/borrowers`)
</script>

<template>
  <h1>Fino - Loan Management App</h1>
  <h2>Borrower List</h2>
  <ul>
    <li v-for="borrower in borrowers.data">
      <h3>{{ borrower}}