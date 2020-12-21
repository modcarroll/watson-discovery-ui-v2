# Run locally

This document shows how to run the `watson-discovery-ui` application on your local machine using the V2 API.

## Steps

1. [Clone the repo](#1-clone-the-repo)
1. [Create your Watson Discovery service](#2-create-your-watson-discovery-service)
1. [Load Discovery files and configure collection](#3-load-discovery-files-and-configure-collection)
1. [Add Watson Discovery credentials](#4-add-watson-discovery-credentials)
1. [Run the application](#5-run-the-application)

## 1. Clone the repo

```bash
git clone https://github.com/modlanglais/watson-discovery-ui
```

## 2. Create your Watson Discovery service

To create your Watson Discovery service:

  1. Click **Create resource** on your IBM Cloud dashboard.

  2. Search the catalog for `discovery`.

  3. Click the **Discovery** tile to launch the create panel.

![create-service](https://raw.githubusercontent.com/IBM/pattern-utils/master/watson-discovery/discover-service-create.png)

~~From the panel, enter a unique name, a region and resource group, and a plan type (select the default **lite** plan). Click **Create** to create and enable your service.~~
> The V2 SDK currently only works with the Discovery Premium plan. 

## 3. Load Discovery files and configure collection

Launch the **Watson Discovery** tool. Create a new data collection by selecting the **Update your own data** option. Give the data collection a unique name.

When prompted to get started by **uploading your data**, select and upload the json documents located in your local `data/airbnb` directory. Once uploaded, you will then visit the **Enrichments** tab to add enrichments. Add the following enrichments:

Sentiment of Document:
- `text`
- `enriched_text`
- `enriched_text.entities`
- `enriched_text.keywords`
- `enriched_text.keywords.text`
- `enriched_text.sentiment`
- `enriched_text.sentiment.score`

Entities:
- `text`

Keywords:
- `text`

> Note: All of these are not actually needed, but I wanted to add extras just in case.

Once the enrichments are selected, use the **Apply changes to collection** button to apply your changes. **Warning** - this make take several minutes to complete.

## 4. Add Watson Discovery credentials

Next, you'll need to add the Watson Discovery credentials to the .env file.

1. From the home directory of your cloned local repo, create a .env file by copying it from the sample version.

```bash
cp env.sample .env
```

2. Locate the service credentials listed on the home page of your Discovery service and copy the `API Key` and `URL` values.

![get-creds](https://raw.githubusercontent.com/IBM/pattern-utils/master/watson-discovery/get-creds.png)

3. From your Discovery service collection page, select the `Integrate and Deploy` menu option on the right. Select the `API Information" tab to retrieve your Project ID.

4. Use the following curl command to retrieve your Project ID: 

```bash
`curl -H "Authorization: Bearer {token}" "https://{cpd_cluster_host}:{port}/discovery/{release}/instance/{instance_id}/api/v2/projects/{project_id}/collections?version=2019-11-29`
```

5. Take the copied values and paste them into the `.env` file:

```bash
# Copy this file to .env and replace the credentials with
# your own before starting the app.

# Watson Discovery
DISCOVERY_URL=<add_discovery_url>
DISCOVERY_COLLECTION_ID=<add_discovery_collection_id>
DISCOVERY_PROJECT_ID=<add_discovery_project_id>
DISCOVERY_APIKEY=<add_discovery_iam_apikey>

# Run locally on a non-default port (default is 3000)
# PORT=3000
```

## 6. Run the application

Install [Node.js](https://nodejs.org/en/) runtime or NPM.

Then run:

```bash
npm install
npm start
```

The application will be available in your browser at `http://localhost:3000`.

> Note: server host can be changed as required in app.js and `PORT` can be set in the `.env` file.

[![return](https://raw.githubusercontent.com/IBM/pattern-utils/master/deploy-buttons/return.png)](https://github.com/IBM/watson-discovery-ui#deployment-options)
