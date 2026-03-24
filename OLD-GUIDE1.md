- Clone repo
```sh
git clone https://github.com/Marktawa/fino.git
```

- Change directory
```sh
cd fino
```

- Create Strapi app
```shell
npx create-strapi-app@latest backend \
 --skip-cloud \
 --skip-db \
 --no-example \
 --js \
 --use-npm \
 --install \
 --no-git-init
```

```shell
cd backend
```

Create an admin user.
```shell
npm run strapi admin:create-user -- --firstname=Kai --lastname=Doe --email=chef@strapi.io --password=Gourmet1234
```

Start your Strapi server.
```shell
npm run develop
```

## Create Nuxt frontend app

### Download and Install Nuxt

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

### Create Hello World Nuxt app

Create a `pages` folder:
```shell
mkdir pages
```

Create an `index.vue` file inside the `pages` folder:
```shell
touch pages/index.vue
```

Add this:
```vue
<template>
    <h1>Hello, World!</h1>
</template>
```

Run the development server:
```shell
npm run dev
```

Visit `http://localhost:3000` to view your site. You should see a web page with the text "Hello, World!".

- go to Strapi Dashboard
- Create a client collection with the following fields:
- Name, text
- Email, email
- Requested Amount, number
- Approval Status, enum (pending, rejected, approved)
- Amount Given, number
- Amount Due, number

```
name, Text
email, Email
requested_amount, Number
loan_status, Enum
amount_given, Number
amount_due, Number
```

- Create 4 entries in Dashboard

- Update permissions `public`
- `find`, `findOne`, `create`, `delete`, `update`
- Test `read`
```shell
curl -X GET http://localhost:1337/api/clients > test1.json
```

- Create Loan Management App Home page
Update `pages/index.vue` with the following code:
```vue
<template>
    <h1>Loan Management App</h1>
    <p>Welcome to the Loan Management App</p>
    <a href="/clients">Click here to view your clients</a>
</template>
```
Run the development server:
```shell
npm run dev
```
- Nuxt read clients
- Create a new folder named `clients` inside the `pages` folder:
```shell
mkdir pages/clients
```

Inside the `pages/clients` directory, create a new file named `index.vue`:
```shell
touch pages/clients/index.vue
```

Add the following code to `pages/clients/index.vue`:
```vue
<script setup>
  const { data: clients } = await useFetch('http://localhost:1337/api/clients')
</script>

<template>
  <h1>Loan Management App</h1>
  <h2>Clients List</h2>
  <ul>
    <li v-for="client in clients.data">{{ client.name }}</li>
  </ul>
</template>
```
done

- Create Individual Client page
Inside the `pages/clients` directory, create a new file named `[id].vue`:

```shell
touch pages/clients/[id].vue
```

Add the following code to `pages/clients/[id].vue`:
```vue
<script setup>
    const route = useRoute()
    const { data: client } = await useFetch(`http://localhost:1337/api/clients/${route.params.id}`)
</script>

<template>
    <h1>Loan Management App</h1>
    <h2>{{ client.data.name }}</h2>
    <p>Email: {{ client.data.email}}</p>
    <p>Request Amount: {{ client.data.requesed_amount }}</p>
    <p>Approval Status: {{ client.data.loan_status }}</p>
    <p>Amount Given: {{ client.data.amount_given }}</p>
    <p>Amount Due: {{ client.data.amount_due }}</p>
    <NuxtLink to="/clients">Back to Clients List</NuxtLink>
</template>
```

- Update `pages/clients/index.vue` to add links to individual clients:
```vue
<script setup>
  const { data: clients } = await useFetch(`http://localhost:1337/api/clients`)
</script>

<template>
  <h1>Loan Management App</h1>
  <h2>Clients List</h2>
  <ul>
    <li v-for="client in clients.data">
      <h3>{{ client.name }}</h3>
      <NuxtLink :to="`/clients/${client.documentId}`">
        <button>View Client</button>
      </NuxtLink>
    </li>
  </ul>
</template>
```

- Visit `http://localhost:3000`, `http://localhost:3000/clients`, `http://localhost:3000/clients/documentId`

- Create Client
- Create `pages/clients/create.vue`
```shell
touch pages/clients/create.vue
```

Add the following code to `create.vue`:
```vue
<template>
  <h1>Loan Management App</h1>
  <h2>Create a New Client</h2>

  <form action="/api/clients/create" method="POST">
    <div>
      <label for="name">Name:</label>
      <input type="text" name="name" id="name" required />
    </div>

    <div>
      <label for="email">Email:</label>
      <input type="email" name="email" id="email" required />
    </div>

    <label for="requested_amount">Requested Amount (ZMW)</label>
        <input
          type="number"
          id="requested_amount"
          name="requested_amount"
          min="0"
          step="100"
          required
        />

    <label for="amount_given">Amount Approved (ZMW)</label>
        <input
          type="number"
          id="amount_given"
          name="amount_given"
          min="0"
          step="100"
          required
        />

    <label for="amount_due">Amount Due (ZMW)</label>
        <input
          type="number"
          id="amount_due"
          name="amount_due"
          min="0"
          step="100"
          required
        />

    <label for="loan_status">Loan Status</label>
        <select id="loan_status" name="loan_status" required>
          <option value="">-- Select Status --</option>
          <option value="PENDING">Pending</option>
          <option value="ACTIVE">Active</option>
          <option value="CLOSED">Closed</option>
        </select>
    
    <button type="submit">Create Client</button>
  </form>
</template>
```

- Nuxt Internal API route
Create the `server`, `server/api`, and `server/api/clients` directories as follows:
```shell
mkdir -p server/api/clients
```
Create a file named `create.client.js` inside the `server/api/clients` directory:
```shell
touch server/api/clients/create.client.js
```

Add the following code to `server/api/clients/create.client.js`:
```js
export default defineEventHandler(async (event) => {
    // Read form data sent from the browser
    const body = await readBody(event)

    try {
        //Send data to Strapi
        const strapiResponse = await $fetch('http://localhost:1337/api/clients', {
            method: 'POST',
            body: {
                data: {
                    name: body.name,
                    email: body.email,
                    requested_amount: body.requested_amount,
                    amount_given: body.amount_given,
                    amount_due: body.amount_due,
                    loan_status: body.loan_status
                }
            }
        })

        //Handle Success
        if (strapiResponse.data) {
            const noteId = strapiResponse.data.documentId
            return sendRedirect(event, `/clients/created?success=1&id=${noteId}`)
        }

        // Fallback if no data returned
        return sendRedirect(event, '/clients/created?success=0')
    
    } catch (error) {
        // Handle Error
        console.error('Failed to create a client:', error)
        return sendRedirect(event, '/notes/client?success=0')
    }
})
```

Next, create a `created.vue` file inside the `pages/clients` directory:
```shell
touch pages/clients/created.vue
```

#### Create page to view Client Creation Status

Inside the `pages/clients` directory, create a new file named `created.vue`:

Add the following code to `pages/clients/created.vue`:
```vue
<script setup>
    const route = useRoute()
    const isSuccess = route.query.success === '1'
    const clientId = route.query.id
</script>

<template>
    <h1>Loan Management App</h1>

     <!-- Success State -->
     <div v-if="isSuccess">
        <h2>Client Created Successfully!</h2>
        <p>Your client has been saved to the database</p>

        <!-- Link to the individual note using the ID from the query param -->
        <NuxtLink :to="`/clients/${clientId}`">
            <button>View Created Client</button>
        </NuxtLink>

        <NuxtLink to="/clients">Back to Client List</NuxtLink>
     </div>

     <!-- FAILURE STATE -->
     <div v-else>
        <h2>Client Creation Failed</h2>
        <p>There was an error creating your client. Please try again</p>

        <NuxtLink to="/clients/create">
            <button>Try Again</button>
        </NuxtLink>

        <NuxtLink to="/clients">Back to Clients List</NuxtLink>
     </div>
</template>
```