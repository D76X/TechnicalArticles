# Passkeys

[PASSKEYS - What they are, why we want them and how to use them! John Savill's Technical Training](https://www.youtube.com/watch?v=RWcXKQcwBRY&t=147s)    

[Entra Passkey Registration Campaign John Savill's Technical Training](https://www.youtube.com/watch?v=10Se9jR-cR0&t=203s)  

[Enable passkeys in Authenticator](https://learn.microsoft.com/en-us/entra/identity/authentication/how-to-enable-authenticator-passkey)  

[Automatic Passkey Rollout Update John Savill's Technical Training](https://www.youtube.com/watch?v=hAm_DcqH0nY)  

[Entra Synced Passkeys and Passkey Profiles John Savill's Technical Training](https://www.youtube.com/watch?v=e0FPn-gJeO4&t=19s)   

[What is Phishing Resistant Authentication John Savill's Technical Training](https://www.youtube.com/watch?v=Rzt30uytQs4)   

---

## What is the WebAuthn component?

WebAuthn (Web Authentication) is a core web standard that allows 
users to log into websites and apps securely without using passwords. 
It uses `public-key cryptography (PKI)`to verify a user's identity.

The WebAuthn architecture works through three main components:

## 1. Relying Party (RP)

- What it is: 
The website, web application, or server that wants to authenticate the user.

- What it does: 
It asks the user to log in and **stores only the public key (never a password or rivate key)**.

## 2. Client (User Agent)

- What it is: 
The web browser running on your device (such as Chrome, Safari, Firefox, or Edge).

- What it does: It acts as a messenger. 
It runs the `WebAuthn API (navigator.credentials)` to pass messages between the website and your authenticator.

## 3. Authenticator

- What it is: 
The hardware or software tool that proves who you are. It holds your secret private key.

- Types of authenticators:

    - Platform authenticators: 
    Built directly into your device (like `Apple Touch ID/Face ID, Windows Hello, or Android biometrics`).
    
    - Roaming authenticators: 
    External hardware keys you plug in or connect (like a `USB security key` or `Bluetooth token`).

## How the Components Work Together

1. Registration: 

- You sign up. 
- The authenticator creates a key pair. It keeps the private key safe and sends the public key to the server (Relying Party).

2. Authentication: 

- You log in. 
- The server sends a test challenge. 
- Your browser asks your authenticator to sign the challenge using your private key.  
- The server checks the signature using the public key and lets you in.

---

# Why is the Microsoft Authenticator App not phishing resistant?

By default, standard `push notifications` and `TOTP (time-based one-time password)` codes in the Microsoft Authenticator app are not classified as phishing-resistant because they do not cryptographically bind the user's login session to the specific website domain being visited. 

This allows advanced `adversary-in-the-middle (AitM) phishing kits (like EvilProxy)` to intercept credentials and relay prompts.

> Note: 

The Microsoft Authenticator app can be used in a phishing-resistant manner if it is explicitly configured to store and use FIDO2-compliant passkeys, but standard push notifications with number matching remain vulnerable to sophisticated interception.

## Why Standard Modes Fail Against Phishing

- No Origin Binding: 

Traditional push prompts and numbers-matching prompts verify what number you are clicking, but **they do not verify where the browser connection actually goes**. If a user visits a fake lookalike login page, i.e. `*.mincrosoft.com`, the `proxy site` **forwards the prompt** to the user's real phone, and the user approves it blindly thinking it is legitimate.

- Session Token Interception: 

Once the user taps the matching number on their app, the malicious proxy captures the resulting `session/access tokens` instantly, granting the attacker full entry despite number-matching protections.

- Lack of Hardware Cryptography: 

Traditional push notifications rely on a cloud signal rather than a localized cryptographic challenge-response tied directly to the trusted origin domain.

## How to Make It Phishing-Resistant

- Upgrade configuration settings to utilize Microsoft Entra passkeys inside the app, which **employ FIDO2 standards to cryptographically lock the credential to the precise URL domain**.

- Enforce strict Conditional Access policies requiring hardware keys or platform-bound credentials rather than software-based push notifications.

---
