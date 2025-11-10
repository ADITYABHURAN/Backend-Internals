# 🛡️ Authentication & Authorization Concepts for Web Devs

This document summarizes key concepts in web authentication, covering traditional methods like sessions and modern standards like OAuth and OpenID Connect (OIDC).

***

## 1. Stateful Authentication & Sessions

These concepts are about the server remembering who you are after you log in.

### Sessions (Stateful Authentication)

* **Stateful Auth:** The **server remembers** the user's login status (or "state") after they enter a username and password.
    * *Analogy:* A concierge remembers your name and room number after you check into a hotel.
* **Session:** A temporary, active link between the user's browser and the server. The server stores user data (the "state"), and the browser holds a unique **Session ID** (usually in a cookie).
    * *Analogy:* A **ticket stub** the server gives your browser. For every new request, the browser shows the stub, and the server looks up the stub's details.

***

## 2. Distributed Systems & Sticky Sessions

When a web application uses multiple servers (**Load Balancing**) to handle heavy traffic, a problem can arise.

### Sticky Sessions (Session Affinity)

* **Load Balancer:** A traffic cop that sends user requests to different backend servers to share the work.
    * *Analogy:* A cop at a busy intersection directing cars to 10 different parking lots.
* **Sticky Session:** A rule that forces the Load Balancer to **always send the same user to the same server** for every request in their session.
    * *Analogy:* The cop knows you parked with **Attendant Bob** and *only* sends you back to Bob, because only Bob has your keys.
* **Necessity:** It's only needed if a server stores the session data **locally** (e.g., in its own memory) and can't be seen by other servers.
* **Drawback:** **Poor Scaling.** If one server is very popular, it gets overworked while others are idle. If that server crashes, all its "stuck" users lose their session.

### Solution for Sticky Sessions

* **Centralized Session Store:** Store all session data in one **shared database** (like Redis) that **all** servers can see and access.
    * *Analogy:* All attendants use one giant, shared, organized **Key Locker**. Now, any attendant can quickly get your keys, and the traffic cop can send you to *anyone*.

***

## 3. Authorization & Identity Protocols

These standards let you use one account (like Google) to log in to many different apps.

### OAuth 2.0 (Open Authorization)

OAuth is an **Authorization** framework—it's about granting permissions, not just logging in.

* **Core Function:** Allows a "Client App" (e.g., a photo printer) to access *specific* user data (like your Google Photos) **without ever seeing your password.**
    * *Analogy:* Giving someone a **valet key** to your car (limited, specific access) instead of your master key (password).
* **Access Token:** A short-lived, encrypted key given to the Client App. It is the proof that the user granted permission to access a specific set of data (**Scope**).
    * *Analogy:* The **valet key**. The Client App presents this key to the Resource Server to open a specific door (e.g., the photo album).
* **Refresh Token:** A long-lived, secure key that the Client App uses to ask the Authorization Server for a **brand new Access Token** after the old one expires.
    * *Analogy:* A special wristband that allows the Client App to skip the login line and quickly get a fresh valet key when the old one stops working.

### OpenID Connect (OIDC) and SAML

| Protocol | Purpose | Simple Explanation |
| :--- | :--- | :--- |
| **OpenID Connect (OIDC)** | **Modern Authentication** built on top of **OAuth 2.0**. Used for "Sign in with Google/Facebook." | Provides a digital **ID Card** (**ID Token**) that proves who the user is, in addition to granting permission (Access Token). |
| **SAML** | **Older, XML-based** protocol used primarily for **Enterprise Single Sign-On (SSO)**. | An older, more formal **Passport** used mostly when linking a company network to cloud services (like corporate email). |
| **Core Difference** | OIDC is simpler, uses modern **JSON** format, and is perfect for mobile and modern web apps. SAML is complex, uses **XML**, and is often for B2B/enterprise connections. |