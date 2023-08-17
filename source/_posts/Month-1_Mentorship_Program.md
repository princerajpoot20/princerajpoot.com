---
title: Month 1 | Crafting an API Endpoint | AsyncAPI Mentorship Program
date: 2023-08-10
tags: ["AsyncAPI"]
---

First and foremost, I want to extend my gratitude to my mentor, **David Pereira**, for his invaluable guidance throughout this journey. I'm immensely grateful for his assistance and mentorship. Working with him has been a fantastic opportunity for me. Additionally, I’d like to extend my heartfelt thanks to my sister, **Princy Rajpoot**, for her tremendous support throughout this journey.

Now, let’s delve into the details of the project.

## My Project Idea

My project idea is to develop an endpoint, tentatively named `help/{command}`, which will provide users with instructions corresponding to a given command. The primary `help` endpoint would list all available endpoints, whereas specific commands like `help/generate` would return detailed information about that particular command, such as available templates and parameters.

## Getting Started with `server-api`
My journey began with a deep dive into the `server-api` project. Analyzing the codebase, I identified six existing endpoints:
`/validate, /parse, /generate, /convert, /bundle, /diff`

Upon familiarizing myself with these endpoints, I proceeded to set up the project locally. Interestingly, all existing endpoints employ the `POST` method. Hence, to invoke them, one must use `POSTMAN`. Since they all use the `POST` method, sending a `request body` is essential to obtain a response.

Having successfully set up the project on my local machine, I delved deeper into understanding its codebase, the overarching project structure, and its inherent workflow.

## Designing the `/help` Endpoint: Key Decisions
I began thinking about the structure of the `/help` endpoint. Given that our objective is to provide help for multiple endpoints, let's start by considering how we would pass data to the `/help` endpoint. 
The project idea already suggests utilizing the `URL approach`, as **indicated by** `help/{command}`.
You might be wondering why the project idea leaned in that direction, especially when the `request body` is such a viable option. Let's first understand the differences between the two, and then delve into why we chose `URL` over the `request body`.

- **`URL-based` data transmission** is ideal when:
   - The data amount is small, ensuring URLs don’t get too lengthy.
   - The information isn't sensitive. For context, transmitting sensitive data, like passwords, in URLs can expose them.
   
- **`Request body` data transmission** becomes crucial when:
   - Data size exceeds the URL’s character limit (usually around 2048 characters).
   - There's a need to send confidential information, which won't be visible in the URL.

Considering both the non-sensitive nature of the data and its compactness, using the `URL method` is the best choice. 

## Enhance User Experience

 Enhance user experience by **providing relative links** to all the endpoints directly within the `/help` section, which contains a list of all the endpoints. Here is a snippet of `/help` response:

```
[
    ...
    
    {
        "command": "parse",
        "url": "/help/parse"
    },
    {
        "command": "generate",
        "url": "/help/generate"
    }
    
    ...
]
```

Users can now conveniently click on links like `/help/generate` to access specific functionalities, making navigation straightforward and user-friendly.


## Ensuring Flexibility

During discussions with my mentor **David**, he highlighted that **hardcoding data for the help command wasn’t ideal.** Instead, sourcing it from a GitHub repository would facilitate easier updates and maintenance. Therefore, I decided to source our `/help` data from GitHub repo.

## Debugging Dilemmas: The Unexpected Rate Limit Riddle

After researching and building the MVP structure, I delved deep into the testing and debugging stage. Full of confidence in my code, I encountered an unexpected hurdle. Initially, I was met with an error, and while troubleshooting it, another error cropped up — all within the confines of my previous codebase. The scenario was mystifying. It was like playing a game of digital whack-a-mole. 🤔

I decided to step back for a moment. After a calming tea break, I revisited the issue with a renewed mindset. Much to my surprise, the root cause wasn't a glitch in my code but an external limitation I hadn't accounted for: the **GitHub API's rate limit**. I had inadvertently overlooked GitHub's rate restriction of 60 requests per hour for each user. Amid my enthusiasm to refine the endpoint, I quickly consumed these requests, resulting in the unforeseen errors.

**But here's the twist**: I was in a race against time, needing to test repeatedly. While I couldn't beat time, I found a way to switch my user identity. 😄 Wondering how? By using a `VPN`, I was able to mask my IP address. This trick allowed me to simulate different users, bypassing the rate limit. A little creative problem-solving, and I was back on track, proving once again that there’s always a way around a roadblock, sometimes just by changing lanes!😌

## Current Progress 🏗️

As of now, I'm in the process of finalizing the **structure** and **defining the data** for all the commands.

To facilitate this, I've employed `axios.get()` to fetch the `openapi.yml` file of the project, which contains essential data for the help command.

Here's a brief on the tools and libraries in use:

- **Axios**: It's a promise-based HTTP client that's both lightweight and efficient. I've specifically used it to make an HTTP request to GitHub's API and retrieve the raw contents of the `openapi.yml` file.

- **js-yaml**: YAML is a human-friendly data format widely adopted for configuration files and data storage/transmission. The `js-yaml` library facilitates the parsing and stringifying of YAML content. With its `yaml.load()` function, I've converted the raw YAML content (fetched using `Axios`) into a workable JavaScript object. This transformed object now serves as the foundation for subsequent operations in the endpoint, such as **resolving references** and **pinpointing matching paths**.

## What’s next?

In my next blog post, I’ll explore different challenges faced. As this phase wraps up, my attention will shift towards `creating unit tests.`

Till next time,
Prince