---
sidebar_position: 2
---

# AuthToken

This is for systems that use auto Login functionality. The request for this need to originate from within the super app as it requires the access_token which is generated from with the super app via function call from the H5. For more information check the demo provided.

### Request Parameters

#### HEADER PARAMETERS
|Parameter|  Data Type|  M/O|Description|
|:----|:----|:----|:----|
|X-APP-Key|  String|  M|Fabric App ID, provided by fabric portal of Ethio telecom|
|Authorization |  String|  M|App Token for authentication|

#### REQUEST BODY SCHEMA:
-- Table here -- 

#### Example Http Request
-- Table here --

### Response Parameters
-- Table here --

#### Example Http Response (200: Processed Successful)
-- Table here --

#### Example Http Response (405: Invalid Input)
-- Table here --

- a **sidebar**
- **previous/next navigation**
- **versioning**

## Create your first Doc

Create a Markdown file at `docs/hello.md`:

```md title="docs/hello.md"
# Hello

This is my **first Docusaurus document**!
```

A new document is now available at [http://localhost:3000/docs/hello](http://localhost:3000/docs/hello).

## Configure the Sidebar

Docusaurus automatically **creates a sidebar** from the `docs` folder.

Add metadata to customize the sidebar label and position:

```md title="docs/hello.md" {1-4}
---
sidebar_label: 'Hi!'
sidebar_position: 3
---

# Hello

This is my **first Docusaurus document**!
```

It is also possible to create your sidebar explicitly in `sidebars.js`:

```js title="sidebars.js"
module.exports = {
  tutorialSidebar: [
    'intro',
    // highlight-next-line
    'hello',
    {
      type: 'category',
      label: 'Tutorial',
      items: ['tutorial-basics/create-a-document'],
    },
  ],
};
```
