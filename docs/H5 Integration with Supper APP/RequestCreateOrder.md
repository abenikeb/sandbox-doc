---
sidebar_position: 3
---

# RequestCreateOrder
To create an order, you must first obtain a token from applyFabricToken method and set it in the header before you can place a request.

### Request Parameters

#### HEADER PARAMETERS
|Parameter|  Data Type|  M/O|Description|
|:----|:----|:----|:----|
|X-APP-Key|  String|  M|Fabric App ID, provided by fabric portal of Ethio telecom|
|Authorization |  String|  M|App Token for authentication|

#### REQUEST BODY SCHEMA
-- Table here --

#### Example Http Request
-- Table here --

### Response Parameters
-- Table here --

#### HEADER PARAMETERS
-- Table here --

#### RESPONSE BODY SCHEMA
-- Table here --

#### Example HTTP Response (200: Processed successful)
-- Table here --

#### Example Http Response (405: Invalid input)
-- Table here --



Create a file at `blog/2021-02-28-greetings.md`:

```md title="blog/2021-02-28-greetings.md"
---
slug: greetings
title: Greetings!
authors:
  - name: Joel Marcey
    title: Co-creator of Docusaurus 1
    url: https://github.com/JoelMarcey
    image_url: https://github.com/JoelMarcey.png
  - name: Sébastien Lorber
    title: Docusaurus maintainer
    url: https://sebastienlorber.com
    image_url: https://github.com/slorber.png
tags: [greetings]
---

Congratulations, you have made your first post!

Feel free to play around and edit this post as much you like.
```

A new blog post is now available at [http://localhost:3000/blog/greetings](http://localhost:3000/blog/greetings).
