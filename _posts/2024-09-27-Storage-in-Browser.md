---
layout: post
title: "Storage in Browser"
description: "Introduction to 3 ways to store data in browser"
date: 2024-09-27
feature_image: images/frontend.jpg
tags: ['frontend', 'web-development']
---

Cookies, local storage, and session storage are most common ways to store data in browser. And they have different use cases.

<!--more-->

# Cookies 🍪

- **Definition and Usage**:
  - Cookies are small pieces of text information sent by the server to the user's browser and stored locally. When the browser makes another request to the same server, it automatically includes this cookie.
  - They are primarily used for maintaining user session state, personal settings, and other non-sensitive data.
- **Characteristics**:
  - The total size limit for cookies per domain is typically around 4KB.
  - You can set an expiration time; if no expiration time is set, the cookie will be deleted when the browser is closed.
  - They are less secure because they can be accessed via client-side scripts and are sent with every request, which can lead to bandwidth waste.
  - They support cross-origin sharing (by setting the `domain` attribute).
- **Usage Example**:
  ```javascript
  // Set a cookie named "username" that expires in 1 day
  document.cookie = "username=John Doe; expires=" + new Date(Date.now() + 86400000).toUTCString();

  // Read all cookies
  function getCookie(name) {
    let match = document.cookie.match(new RegExp('(^| )' + name + '=([^;]+)'));
    if (match) return match[2];
  }

  console.log(getCookie("username")); // Outputs "John Doe"
  ```

# LocalStorage 📁
- **Definition and Usage**:
  - LocalStorage provides a persistent local storage solution. The data stored in LocalStorage does not expire unless explicitly deleted.
  - It is suitable for storing non-sensitive user preferences or application state.
- **Characteristics**:
  - The storage capacity is usually around 5MB, but this can vary depending on the browser.
  - Data is stored as key-value pairs, where both keys and values must be strings.
  - It does not support cross-origin access.
  - Data is shared across different windows/tabs.
- **Usage Example**:
  ```javascript
  // Store data
  localStorage.setItem("theme", "dark");

  // Retrieve data
  let theme = localStorage.getItem("theme");
  console.log(theme); // Outputs "dark"

  // Remove data
  localStorage.removeItem("theme");

  // Clear all data in LocalStorage
  localStorage.clear();
  ```

# SessionStorage 📁
- **Definition and Usage**:
  - SessionStorage is very similar to LocalStorage, but its lifetime is limited to the current session.
  - The data in SessionStorage is cleared when the page or tab is closed.
  - It is useful for temporarily storing data during a single browsing session.
- **Characteristics**:
  - Data is also stored as key-value pairs, and only string types are supported.
  - The storage capacity is typically around 5MB.
  - It does not support cross-origin access.
  - Even within the same domain, SessionStorage is independent across different windows/tabs.
- **Usage Example**:
  ```javascript
  // Store data
  sessionStorage.setItem("pageVisited", "true");

  // Retrieve data
  let visited = sessionStorage.getItem("pageVisited");
  console.log(visited); // Outputs "true"

  // Remove data
  sessionStorage.removeItem("pageVisited");

  // Clear all data in SessionStorage
  sessionStorage.clear();
  ```

# Summary

Each of these methods has its own strengths and weaknesses, and the choice depends on your specific needs. For example, if you need to maintain user state and share data across multiple pages, you might consider using LocalStorage or Cookies. If you just need to store some temporary information, then SessionStorage would be more appropriate.

| Feature/Method            | Cookie                                                | LocalStorage                                                | SessionStorage                                     |
| ------------------------- | ----------------------------------------------------- | ----------------------------------------------------------- | -------------------------------------------------- |
| **Storage Capacity**      | Typically around 4KB                                  | Typically around 5MB                                        | Typically around 5MB                               |
| **Persistence**           | Can set expiration; cleared at session end by default | Persistent until explicitly deleted                         | Only valid for the current session                 |
| **Data Format**           | Key-value pairs, values as strings                    | Key-value pairs, values as strings                          | Key-value pairs, values as strings                 |
| **Cross-Origin Access**   | Supported (by setting `domain` attribute)             | Not supported                                               | Not supported                                      |
| **Security**              | Lower, accessible via client-side scripts             | Higher, but still accessible via JavaScript                 | Higher, but still accessible via JavaScript        |
| **Data Sharing**          | Shared across all windows/tabs                        | Shared across all windows/tabs                              | Independent per window/tab                         |
| **Primary Use Case**      | Maintaining user state, personal settings, etc.       | Storing non-sensitive user preferences or application state | Temporary storage during a single browsing session |
| **Read/Write Operations** | Via `document.cookie`                                 | Via `localStorage` API                                      | Via `sessionStorage` API                           |
