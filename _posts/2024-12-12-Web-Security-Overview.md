---
layout: post
title: "Web Security Overview"
description: "Overview of web security"
date: 2024-12-12
feature_image: images/security.png
tags: ['web-development', 'security']
---

Web security is a broad topic that covers a wide range of topics related to the security of web applications. It involves the protection of websites, web applications, and web services from unauthorized access, use, disclosure, disruption, modification, or destruction. It also involves the protection of user data and ensuring the integrity of the system by preventing unauthorized access and malicious activities.

<!--more-->

Web security is important because web applications are often the primary interface between an organization and its customers, partners, or employees. Web applications are also the primary target of attackers, as they can provide access to sensitive information and systems. Therefore, it is essential to ensure that web applications are secure and can withstand various types of attacks.

## Table of Contents
- [Web Basics](#web-basics)
  - [HTML, CSS, and JavaScript](#html-css-and-javascript)
    - [HTML (HyperText Markup Language)](#html-hypertext-markup-language)
    - [CSS (Cascading Style Sheets)](#css-cascading-style-sheets)
    - [JS (JavaScript)](#js-javascript)
  - [HTTP, HTTPS, and TLS/SSL](#http-https-and-tlsssl)
    - [HTTP (HyperText Transfer Protocol)](#http-hypertext-transfer-protocol)
    - [SSL (Secure Sockets Layer) and TLS (Transport Layer Security)](#ssl-secure-sockets-layer-and-tls-transport-layer-security)
    - [HTTPS (HyperText Transfer Protocol Secure)](#https-hypertext-transfer-protocol-secure)
  - [Cookies, JWT, and Session Management](#cookies-jwt-and-session-management)
    - [Cookies](#cookies)
    - [JSON Web Tokens (JWT)](#json-web-tokens-jwt)
    - [Session Management](#session-management)
- [Attack Scenarios and Countermeasures](#attack-scenarios-and-countermeasures)

---

# Web Basics

Before we dive into web security, let's understand the basics of web development. Basically, a web page is a collection of **HTML, CSS, and JavaScript** that creates a dynamic and interactive experience for users. And beyond that, transporting data between the server and the client using **HTTP, HTTPS, and TLS/SSL**. In addition, **cookies, JWT, and session management** are also important mechanisms for identifying and managing state in web applications.

## HTML, CSS, and JavaScript

Web development is a diverse field that encompasses many different technologies. HTML, CSS, and JavaScript are three of the most commonly used technologies used in web development.

{% include image_caption.html imageurl="/images/web-basics.png" title="Web Basics" caption="web basics" %}

Together, HTML, CSS, and JavaScript form the core technologies essential for web development. They each serve distinct purposes and when combined, enable the creation of engaging, functional, and visually appealing web applications. Each technology has its own unique features and capabilities that allow developers to create dynamic and interactive web pages.

### HTML (HyperText Markup Language)

HyperText Markup Language is the standard markup language for creating the **structure and content** of web pages. It uses **tags to define elements** such as headings, paragraphs, links, and multimedia components like images and videos. HTML can also embed scripts and additional media through plugins, enhancing the page with dynamic content and interactive features. To summarize its functions:

- Define the **semantic structure** of web page content.
- Contains **multimedia elements** such as pictures, videos.
- Use **forms** to collect user input.
- **Additional media types** (e.g., PDF files, Flash animations) can be embedded via plug-ins.

### CSS (Cascading Style Sheets)

Cascading Style Sheets is used to **style and layout** web pages by controlling the **visual presentation** of HTML or XML documents. It separates content from design, making it easier to maintain and update the appearance of websites. CSS supports **responsive design**, ensuring that web pages look good on all devices and screen sizes. Developers often use **preprocessors** like SASS or LESS to write more efficient and modular CSS code. To summarize its functions:

- **Control** the layout, colors, fonts, and other **visual attributes** of your web pages.
- Content can be **separated from performance** for easy maintenance and design updates.
- Support **responsive design** to adapt pages to different screen sizes.
- Allows Developers to write more concise, modular code using **preprocessors** (E.G., SASS, LESS).

### JS (JavaScript)

JavaScript is a **versatile**, **multi-paradigm** programming language primarily used for web development. Initially designed to **run in client-side browsers**, it has since evolved to server-side applications (e.g., Node.js). JavaScript allows for **dynamic content updates** and interaction with the user without reloading the entire page. It can **handle events**, manipulate the Document Object Model (DOM), and communicate asynchronously with servers via AJAX. Summarizing its functions:
- **Dynamically modify** web page content and styles.
- **Handle** and react to user **events** (e.g., clicks, scrolls).
- **Send** an asynchronous **request** to the server (AJAX) to update part of a web page without reloading the entire page.
- **Manipulate the DOM** (Document Object Model), which is a structured representation of an HTML document.
- Implement complex **interactions and features**, such as games, real-time communication, and more.
- Build **server-side applications** with Node.js.
  
However, due to its ability to interact with the user's environment, it poses potential security risks if not properly managed, such as **Cross-Site Scripting (XSS)** attacks and **clickjacking**. 

## HTTP, HTTPS, and TLS/SSL

In the realm of web communication, data transfer is essential. Various protocols facilitate the exchange of information between clients and servers, ensuring seamless connectivity and interaction across the internet. From HTTP to HTTPS, these protocols play a crucial role in ensuring secure and reliable data transfer. Here's an overview of the protocols involved in web communication:

### HTTP (HyperText Transfer Protocol)

HyperText Transfer Protocol is an application-layer protocol used for data exchange between clients and servers on the internet. It operates primarily over TCP and typically uses port 80 by default.

- **Characteristics**
  - **Stateless**: HTTP is stateless, meaning **each request is independent**, and the server does not retain any information about previous requests. <u>Each new request must carry all necessary information because the server does not remember prior interactions.</u>
  - **Simple and Fast**: Designed to be simple and easy to implement, allowing quick connections and scalability.
Flexible: Supports the transmission of any type of data object, provided both parties know how to handle the data.
Request-Response Model: All communications are initiated by the client making a request, followed by the server responding with data or actions.
  - **Plaintext Transmission**: Early versions of HTTP transmitted data in **plaintext**, making it **susceptible to eavesdropping and tampering**, which poses risks for sensitive information.
- **Applications**: HTTP is widely used for **communication** between web browsers and web servers, as well as for **API calls** and other network services.

### SSL (Secure Sockets Layer) and TLS (Transport Layer Security)

SSL (Secure Sockets Layer), originally developed by Netscape, has been succeeded by TLS (Transport Layer Security), standardized by IETF. TLS versions have evolved from TLS 1.0 to the latest TLS 1.3, improving security and performance with each iteration.

**Working Principles**, SSL/TLS ensures secure communication through several mechanisms:
- **Encryption**: Combines symmetric encryption (e.g., AES) and asymmetric encryption (e.g., RSA or ECDH) to **ensure data confidentiality**. During the handshake process, the client and server **negotiate a shared secret** (master key) used to encrypt subsequent communications.
- **Authentication**: Verifies the server's **identity** using **digital certificates** issued by trusted third-party **Certificate Authorities** (CAs). These certificates contain **public keys** and other identifying information.
- **Integrity**: Prevents data tampering during transmission using Message Authentication Codes (MAC) or HMAC (Hash-based MAC).
- **Handshake Protocol**: A critical step in **establishing a secure connection**, involving parameter exchanges necessary for encrypted communication, such as selecting encryption algorithms and exchanging pre-master secrets.
- **Record Protocol**: Once the handshake is successful, all application data is packaged into TLS records for ransmission. Each record includes version numbers, lengths, types, encrypted payload data, and other control information.

Futhermore, TLS provides **enhanced security features** such as:
- **Forward Secrecy**: Modern key exchange algorithms like DHE and ECDHE provide forward secrecy, ensuring that even if long-term stored keys are compromised in the future, past sessions cannot be decrypted.
- **Certificate Transparency**: Increases transparency in certificate issuance and management, aiding in the timely detection and response to improperly issued certificates.
- **OCSP Stapling**: Allows the server to pass the latest certificate revocation status directly to the client, speeding up the handshake process and reducing load on CAs.

To reduce the overhead of full handshakes on every connection, TLS supports faster handshake methods like Session Tickets and Session IDs. These mechanisms allow resuming previous sessions without repeating the entire handshake process, accelerating connection establishment.

Here is an example of a SSL certificate from Blizard:

{% include image_caption.html imageurl="/images/SSL-certificate.jpg" title="SSL Certificate" caption="SSL Certificate for Blizard" %}

### HTTPS (HyperText Transfer Protocol Secure)
HTTPS adds SSL/TLS to HTTP to provide encrypted communication and ensure data integrity. HTTPS typically uses port 443 and offers higher security and privacy protection compared to HTTP. The characteristics of HTTPS include:

- **Encrypted Communication**: Encrypts HTTP messages using SSL/TLS, preventing eavesdropping and tampering during data transmission. This involves combining symmetric and asymmetric encryption techniques.
- **Authentication**: Not only encrypts data but also verifies the server's identity, allowing clients to confirm the server's authenticity via digital certificates, thus preventing man-in-the-middle attacks.
- **Integrity**: Ensures data integrity, so even if data is intercepted, attackers cannot modify it without detection.
- **Performance Overhead**: Encryption and decryption consume computational resources, introducing additional performance costs compared to HTTP. However, modern hardware significantly mitigates this impact.
- **SEO Advantage**: Search engines favor HTTPS websites, considering them more secure and reliable, which can boost search rankings.

HTTPS is a **secure and reliable communication protocol** that provides enhanced security and privacy for web traffic. Servers can optionally instruct browsers to **access the site exclusively via HTTPS using HSTS policies**, enhancing security further. 

The HTTPS handshake involves the TLS/SSL handshake, including:
- **ClientHello**: The client sends a ClientHello message containing supported TLS versions, cipher suites, random numbers, and possibly session IDs.
- **ServerHello**: The server selects a mutually supported cipher suite and responds with a ServerHello message, also containing a random number.
- **Certificate Exchange**: The server sends its digital certificate to the client, which contains the public key for verifying the server's identity and subsequent key exchanges.
ClientKeyExchange: The client generates a pre-master secret and sends it to the server, encrypted with the server’s public key.
- **ChangeCipherSpec**: Both client and server send ChangeCipherSpec messages indicating they will switch to using negotiated keys and encryption algorithms for communication.
Finished: Both sides send Finished messages, encrypted with the new keys, to verify the successful completion of the handshake.
- **Application Data Transmission**: After the handshake, client and server communicate using the negotiated encryption parameters.

{% include image_caption.html imageurl="/images/SSL-TLS.jpg" title="SSL/TLS" caption="SSL/TLS handshake" %}

Although HTTPS remains a **stateless protocol**, web applications often use **additional mechanisms to maintain session states** across multiple requests. Methods include setting **Cookies**, using **Session IDs**, or **Tokens** (such as JWT), which pass state information between requests, enabling the application layer to track user sessions.

In summary, HTTPS provides enhanced security and privacy for internet communications, despite introducing some complexity and minor performance costs. In today’s increasingly security-conscious environment, HTTPS has become the standard configuration for network services.

## Cookies, JWT, and Session Management

In modern web applications, maintaining user state and authentication information is crucial. **Cookies** and **JSON Web Tokens (JWT)** are three essential mechanisms used for this purpose.

### Cookies

Cookies are small pieces of data sent by a server to a client's browser, which then sends them back with every subsequent request to the same server. This allows the server to identify specific users.

- **Working Principle**:
  - **Setting Cookies**: The server sets cookies using the Set-Cookie header in its HTTP response.
  - **Storing Cookies**: Browsers store cookies based on attributes like path, domain, and expiration time.
  - **Sending Cookies**: Browsers automatically include stored cookies in the headers of future requests to the same domain.
  - **Deleting Cookies**: Cookies can be deleted by setting their expiration time to a past date or sending a Set-Cookie header with Max-Age=0.
- **Security Features**:
  - **HttpOnly**: Prevents access via JavaScript, reducing XSS risks.
  - **Secure**: Ensures cookies are only sent over HTTPS connections.
  - **SameSite**: Limits cookie behavior in cross-site requests, helping prevent CSRF attacks.
- **Use Cases**:
  - **Authentication**: Used to verify user identity and maintain session state.
  - **Personalization**: Stores user preferences, such as language or theme.
  - **Analytics**: Tracks user behavior and usage patterns.

### JSON Web Tokens (JWT)
JWT is a compact and self-contained method for securely transmitting information between parties as a JSON object. It is commonly used for authentication and information exchange.

- **Structure**: A JWT consists of three parts separated by dots (.)
  - **Header**: Contains metadata about the token, including the type and signing algorithm.
  - **Payload**: Contains claims, such as user ID and permissions.
  - **Signature**: Verifies the integrity of the token by signing the header and payload with a secret or private key.
- **Workflow**:
  - **Generating Token**: After successful authentication, the server generates a JWT and sends it to the client.
  - **Storing Token**: Clients store the JWT locally, often in local storage, session storage, or as an HttpOnly cookie.
  - **Sending Token**: Clients include the JWT in the Authorization header of each request (`Authorization: Bearer <token>`).
  - **Verifying Token**: The server parses and verifies the JWT’s signature and claims before processing the request.
- **Advantages**:
  - **Stateless**: Reduces server load as no session data needs to be stored.
  - **Cross-Domain Support**: Suitable for cross-origin requests.
  - **Easier Debugging**: Self-descriptive tokens allow developers to inspect their contents.
- **Disadvantages**:
  - **Size**: Larger than simple session IDs, especially with extensive payloads.
  - **Revocation Challenges**: Harder to revoke or update once issued unless short-lived.

### Session Management
Session management tracks user interactions across multiple requests, providing personalized services despite HTTP being stateless.

- **Implementation**:
  - **Cookie-Based Sessions**: The server generates a unique Session ID and sends it to the client via Set-Cookie. The client includes this ID in subsequent requests.
  - **Token-Based Sessions**: Similar to JWT but typically refers to short-lived, opaque tokens for brief session states.
Session Storage:
  - **Server-Side**: Store session data in memory, databases, or distributed caches (e.g., Redis) for scalability.
Client-Side: Occasionally encode session data in URLs or hidden form fields, though this is less common due to security concerns.
- **Security Practices**:
  - **Regularly Refresh Session IDs** to mitigate long-lived session hijacking risks.
  - **Secure Transmission**: Always use HTTPS to send session identifiers.
  - **Limit Session Lifespan**: Implement timeouts to automatically terminate idle sessions.
  - **Multi-Factor Authentication**: Enhance security with additional verification steps.

| Feature         | Cookie-Based Sessions                                                                              | Token-Based Sessions (e.g., JWT)                                                                                                      |
| --------------- | -------------------------------------------------------------------------------------------------- | ------------------------------------------------------------------------------------------------------------------------------------- |
| **Storage**     | Server-Side (Session data stored on server, session ID stored client-side in a cookie)             | Client-Side (Token stored in local storage, session storage, or HttpOnly cookie)                                                      |
| **Security**    | Vulnerable to CSRF if not properly configured; can mitigate XSS with `HttpOnly` and `Secure` flags | Self-contained and tamper-proof, but vulnerable to token theft if not properly secured (e.g., using HTTPS and short expiration times) |
| **Size**        | Generally smaller (only storing a session ID)                                                      | Larger due to payload (includes user claims and metadata)                                                                             |
| **Revocation**  | Easier (server can invalidate session data)                                                        | More difficult (requires blacklisting tokens or short-lived tokens)                                                                   |
| **Scalability** | Potentially less scalable due to server-side storage requirements                                  | More scalable as it reduces server load by being stateless                                                                            |
| **Stateless**   | Not stateless (server must store session data)                                                     | Stateless (server does not need to store session data)                                                                                |
| **Use Cases**   | General-purpose session management, tracking user sessions                                         | Authentication, authorization, and information exchange in stateless applications                                                     |

---

# Attack Scenarios and Countermeasures