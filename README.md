# Equipment rental system built using Strapi and Nuxt

## Introduction

In this article you will learn how to build a minimal equipment rental system using Strapi and Nuxt. We will create a directory page that displays available equipment with photos, pricing, and contact details, allowing customers to connect directly with equipment owners. 

## Prerequisites

- Node.js (LTS v20 or v22) installed on your local machine
- GitHub/GitLab/Google Account to create a Strapi Cloud Account
- Cloudflare Account

## Create a Nuxt project locally

### Install Nuxt

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

### Create a Static Landing Page for your Equipment Rental site

Create a `public` folder:
```shell
mkdir -p public
```

Create a file named `index.html` inside the `public` folder:
```shell
touch public/index.html
```

Add the following code to `index.html`:
```html
<!DOCTYPE html>
<html lang="en">

<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Equiprent</title>
</head>

<body>
    <h1>Equiprent</h1>
    <h2 id="menu">Menu</h2>
    <ul>
        <li><a href="#home">Home</a></li>
        <li><a href="#listings">Listings</a></li>
        <li><a href="#about">About</a></li>
        <li><a href="#contact">Contact</a></li>
    </ul>

    <h2 id="home">Welcome to Equiprent</h2>
    <p>Welcome to Equiprent. This is your one stop equipment and tools rental marketplace. Want to rent a tool, let us help you find a supplier. Want to rent out a tool, let us help you find customers.</p>
    <a href="about">Learn More</a>

    <h2 id="listings">Listings</h2>
    <p>Check out equipment listed for rent from the various suppliers who listed with us.</p>
    <ul>
        <li>
            <h3>5kVA Generator</h3>
            <img src="/generator5.jpg" alt="5kVA Generator" width="300" />
            <p>$20 per day</p>
            <p>Compact tubular-frame generator powered by a petrol engine. Features 2×15A 220V plugs, 20A circuit breaker, and weighs 80–90kg. Dimensions: 800×600×550mm.</p>
            <p>Crucialx Hire</p>
            <p><a href="tel:+1234567890">+1 234 567 890</a></p>
        </li>
        <li>
            <h3>Vacuum Cleaner</h3>
            <img src="/vacuum.jpg" alt="Vacuum Cleaner" width="300" />
            <p>$40 per day</p>
            <p>30L vacuum cleaner with a powerful 1200W 220V motor.</p>
            <p>Regalx Hire</p>
            <p><a href="tel:+44123456789">+44 123 456 789</a></p>
        </li>
        <li>
            <h3>Carpet Dryer</h3>
            <img src="/carpetdryer.jpg" alt="Carpet Dryer" width="300" />
            <p>$15 per day</p>
            <p>High-efficiency air blower with 3-speed cold air settings, designed for fast carpet and floor drying.</p>
            <p>Mainstayx Hire</p>
            <p><a href="tel:+91123456789">+91 123 456 789</a></p>
        </li>
    </ul>

    <h2 id="about">About Us</h2>
    <p>Equiprent is a tool and equipment rental marketplace. Users who visit this site can view listings of equipment and tools for hire. Merchants can list equipment for hire on our platform. To list an item for rent please <a href="#contact">get in touch with us.</a></p>

    <h2 id="contact">Contact</h2>
    <p>Get in touch with us if you would like to list an item for rent, have questions or any other concerns.</p>
    <h3>Contact Details</h3>
    <ul>
        <li>Call: <a href="tel:+260965982075">+260 965 982 075</a></li>
        <li>SMS: <a href="sms:+260965982075">+260 965 982 075</a></li>
        <li>Whatsapp: <a href="https://wa.me/260965982075">+260 965 982 075</a></li>
        <li>Email: <a href="mailto:markmunyaka@gmail.com">mail@equiprent.com</a></li>
    </ul>
</body>

</html>
```

Add the images, `vaccuum.jpg`, `generator5.jpg`, and `carpetdryer.jpg` to your `public` folder.
```sh
curl -o public/vaccuum.jpg https://raw.githubusercontent.com/marktawa/equiprent/main/frontend/public/vaccuum.jpg
curl -o public/generator5.jpg https://raw.githubusercontent.com/marktawa/equiprent/main/frontend/public/generator5.jpg
curl -o public/carpetdryer.jpg https://raw.githubusercontent.com/marktawa/equiprent/main/frontend/public/carpetdryer.jpg
```

Run your Nuxt app development server:
```shell
npm run dev
```

This starts the Nuxt development server on port `3000`. Visit `localhost:3000` in your browser to view the static landing page you just created:
![Equipment Rental Static Landing Page](https://res.cloudinary.com/craigsims808/image/upload/v1752405414/strapi/equiprent/equiprent-landing_xgtofz.png)

## Build your Nuxt app

Stop your Nuxt project development server by pressing the keyboard combo `CTRL` + `C` in your terminal.

Run the `build` command
```shell
npm run build
```

This creates an `.output` directory with all your your application, server and dependencies ready for production.

Run the following command to test the production version of your landing page:
```shell
npm run start
```

Visit `http://localhost:3000` to view your site.
![Production: Equipment Rental Static Landing Page](https://res.cloudinary.com/craigsims808/image/upload/v1752405414/strapi/equiprent/equiprent-landing_xgtofz.png)

## Deploy Nuxt frontend to Cloudflare Workers

Create a new file named `.gitignore` in your Nuxt project folder:
```shell
touch .gitignore
```

Add the following to `.gitignore`:
```.gitgnore
# Nuxt dev/build outputs
.output
.data
.nuxt
.nitro
.cache
dist

# Node dependencies
node_modules

# Logs
logs
*.log

# Misc
.DS_Store
.fleet
.idea

# Local env files
.env
.env.*
!.env.example
```

Create a `nuxt.config.ts` file in the root of your Nuxt project folder, `frontend`:
```shell
touch nuxt.config.ts
```

Add the following code to `nuxt.config.ts`:
```ts
export default defineNuxtConfig({
    compatibilityDate: "2025-05-15",
    nitro: {
      preset: "cloudflare_module",
      cloudflare: {
        deployConfig: true,
        nodeCompat: true
      }
    }
})
```

First make sure to be logged into your Cloudflare account:
```shell
npx wrangler login
```

Deploy the application:
```shell
npx wrangler deploy --name=equiprent
```

After deployment, you will receive a Cloudflare Worker URL for your project, visit it in your browser.

## Create Mock API in Nuxt frontend

Create `server/api/` folders in your Nuxt project folder:
```shell
mkdir -p server/api
```

Create a `server/api/external.js` to host your mock API 
```shell
touch server/api/external.js
```

Add the following code to `server/api/external.js`:
```js
let payload = 
{
    "data": [
        {
            "id": 1,
            "name": "5KVA Generator",
            "image": "https://equiprent.markmunyaka.workers.dev/generator5.jpg",
            "price": "$20 per day",
            "description": "Compact tubular-frame generator powered by a petrol engine. Features 2×15A 220V plugs, 20A circuit breaker, and weighs 80–90kg. Dimensions: 800×600×550mm.",
            "merchant": "Crucialx Hire",
            "contact": "+1 234 567 890"
        },
        {
            "id": 2,
            "name": "Vacuum Cleaner",
            "image": "https://equiprent.markmunyaka.workers.dev/vacuum.jpg",
            "price": "$40 per day",
            "description": "30L vacuum cleaner with a powerful 1200W 220V motor.",
            "merchant": "Regalx Hire",
            "contact": "+44 123 456 789"
        },
        {
            "id": 3,
            "name": "Carpet Dryer",
            "image": "https://equiprent.markmunyaka.workers.dev/carpetdryer.jpg",
            "price": "$15 per day",
            "description": "High-efficiency air blower with 3-speed cold air settings, designed for fast carpet and floor drying.",
            "merchant": "Mainstayx Hire",
            "contact": "+91 123 456 789"
        }
    ]
}

export default defineEventHandler(() => {
    return payload
})
```

Delete the `public/index.html` file as it is no longer needed:
```shell
rm public/index.html
```

In the root of your Nuxt project folder, create a `pages` folder:
```shell
mkdir pages
```

Create a `pages/index.vue` file:
```shell
touch pages/index.vue
```

Add the following code to `pages/index.vue`:
```vue
<template>
    <h1>Equiprent</h1>
    <h2 id="menu">Menu</h2>
    <ul>
        <li><a href="#home">Home</a></li>
        <li><a href="#listings">Listings</a></li>
        <li><a href="#about">About</a></li>
        <li><a href="#contact">Contact</a></li>
    </ul>

    <h2 id="home">Welcome to Equiprent</h2>
    <p>Welcome to Equiprent. This is your one stop equipment and tools rental marketplace. Want to rent a tool, let us help you find a supplier. Want to rent out a tool, let us help you find customers.</p>
    <a href="about">Learn More</a>

    <h2 id="listings">Listings</h2>
    <p>Check out equipment listed for rent from the various suppliers who listed with us.</p>
    <ul>
      <li v-for="item in items.data" :key="item.id">
        <h3>{{ item.name }}</h3>
        <img :src="item.image" :alt="item.name" width="300" />
        <p>{{ item.price }}</p>
        <p>{{ item.description }}</p>
        <p>{{ item.merchant }}</p>
        <p><a :href="`tel:${item.contact}`">{{ item.contact }}</a></p>
      </li>
    </ul>

    <h2 id="about">About Us</h2>
    <p>Equiprent is a tool and equipment rental marketplace. Users who visit this site can view listings of equipment and tools for hire. Merchants can list equipment for hire on our platform. To list an item for rent please <a href="#contact">get in touch with us.</a></p>

    <h2 id="contact">Contact</h2>
    <p>Get in touch with us if you would like to list an item for rent, have questions or any other concerns.</p>
    <h3>Contact Details</h3>
    <ul>
        <li>Call: <a href="tel:+260965982075">+260 965 982 075</a></li>
        <li>SMS: <a href="sms:+260965982075">+260 965 982 075</a></li>
        <li>Whatsapp: <a href="https://wa.me/260965982075">+260 965 982 075</a></li>
        <li>Email: <a href="mailto:markmunyaka@gmail.com">mail@equiprent.com</a></li>
    </ul>
</template>

<script setup>
const { data: items } = await useFetch('/api/external')

useHead({
  title: 'Equiprent',
  meta: [
    { name: 'viewport', content: 'width=device-width, initial-scale=1.0' }
  ]
})
</script>
```

Run your Nuxt app development server:
```shell
npm run dev
```
Visit `localhost:3000` in your browser to view your Nuxt app front page you just created:
![Landing Page with Mock API](https://res.cloudinary.com/craigsims808/image/upload/v1752405414/strapi/equiprent/equiprent-landing_xgtofz.png)

Build your updates:
```shell
npm run build
```

Deploy your updates:
```shell
npx wrangler deploy --name=equiprent
```

## Create a Strapi project locally

Open your working directory in a terminal and run the following command to create a Strapi project:
```bash
npx create-strapi-app@latest backend
```

Answer the prompts as follows:
```
- Ok to proceed? y
```

When prompted, select `Login/Sign up`, then copy the redirect URL and open it in your browser.

Ensure the confirmation code in your browser matches the one in your terminal, then click **Confirm**. Complete the login process, then return to your terminal.

Continue with the following responses:
```
? Do you want to use the default database (sqlite) ? Yes
? Start with an example structure & data? No
? Start with Typescript? No
? Install dependencies with npm? Yes
? Initialize a git repository? No
```

Change into the new Strapi project directory named `backend`:
```shell
cd backend
```

<!-- Strapi Vite set up in Gitpod

Create a `src/admin/vite.config.js` file from the given `src/admin/vite.config.example.js` file:

```shell
cp src/admin/vite.config.example.js src/admin/vite.config.js
```

```shell
vi src/admin/vite.config.js
```

```js
const { mergeConfig } = require('vite');

module.exports = (config) => {
  // Important: always return the modified config
  return mergeConfig(config, {
    resolve: {
      alias: {
        '@': '/src',
      },
    },
    server: {
      allowedHosts: true
    },
  });
};
``` -->

## Register an Admin User

Create an admin user:
```shell
npm run strapi admin:create-user -- --firstname=Kai --lastname=Doe --email=chef@strapi.io --password=Gourmet1234
```

<!--
## Create an `Equipment` collection type

Use the Strapi CLI to generate a new collection type called `Equipment`:

```bash
npm run strapi generate collection-type equipment
```

This will create the basic collection structure. Next, you'll need to define the schema by editing the generated schema file at `src/api/equipment/content-types/equipment/schema.json`:

```json
{
  "kind": "collectionType",
  "collectionName": "equipments",
  "info": {
    "singularName": "equipment",
    "pluralName": "equipments",
    "displayName": "Equipment"
  },
  "options": {
    "draftAndPublish": false
  },
  "pluginOptions": {},
  "attributes": {
    "name": {
      "type": "string"
    },
    "price": {
      "type": "string"
    },
    "merchant": {
      "type": "string"
    },
    "contact": {
      "type": "string"
    },
    "description": {
      "type": "text"
    },
    "image": {
      "type": "string"
    }
  }
}
```

After updating the schema file, restart your Strapi development server:
```bash
npm run develop
```

The server will automatically detect the schema changes and apply them to your database.
-->

<!--
## Create `Equipment` collection-type manually

Create folder and schema:
```shell
mkdir -p src/api/equipment/content-types/equipment
```

Inside the `src/api/equipment/content-types/equipment` folder, create a file named `schema.json`:
```shell
touch src/api/equipment/content-types/equipment/schema.json
```

Open `schema.json` in a text editor and add the following JSON content:
```json
{
  "kind": "collectionType",
  "collectionName": "equipments",
  "info": {
    "singularName": "equipment",
    "pluralName": "equipments",
    "displayName": "Equipment"
  },
  "options": {
    "draftAndPublish": false
  },
  "pluginOptions": {},
  "attributes": {
    "name": {
      "type": "string"
    },
    "price": {
      "type": "string"
    },
    "merchant": {
      "type": "string"
    },
    "contact": {
      "type": "string"
    },
    "description": {
      "type": "text"
    },
    "image": {
      "type": "string"
    }
  }
}
```

After updating the schema file, restart your Strapi development server:
```bash
npm run develop
```

The server will automatically detect the schema changes and apply them to your database.
-->

## Create an `Equipment` collection type

Click **Content-Type Builder** and create a new collection called `Equipment`.
![Create Equipment collection](https://res.cloudinary.com/craigsims808/image/upload/v1751543550/strapi/strapi-landing/create-contact-collection-type_oyi0f9.png)

Click **+ Add new field** and add the following fields to your `Equipment` collection:
- `name`: text field(short)
- `description`: text field(long)
- `image`: text field(short)
- `price`: text field(short)
- `merchant`: text field(short)
- `contact`: text field(short)
![Contacts collection with fields](https://res.cloudinary.com/craigsims808/image/upload/v1751529958/strapi/strapi-landing/contact-collection-with-fields_z4lzsz.png)

Click **Save** and wait for your Strapi server to restart.
