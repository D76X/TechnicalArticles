# Passkeys

[PASSKEYS - What they are, why we want them and how to use them! John Savill's Technical Training](https://www.youtube.com/watch?v=RWcXKQcwBRY&t=147s)    

[Entra Passkey Registration Campaign John Savill's Technical Training](https://www.youtube.com/watch?v=10Se9jR-cR0&t=203s)  

[Enable passkeys in Authenticator](https://learn.microsoft.com/en-us/entra/identity/authentication/how-to-enable-authenticator-passkey)  

[Automatic Passkey Rollout Update John Savill's Technical Training](https://www.youtube.com/watch?v=hAm_DcqH0nY)  

[What is Phishing Resistant Authentication John Savill's Technical Training](https://www.youtube.com/watch?v=Rzt30uytQs4)   

[Entra Synced Passkeys and Passkey Profiles John Savill's Technical Training](https://www.youtube.com/watch?v=e0FPn-gJeO4&t=19s)   

[Synced Passkeys in Microsoft Entra for Phishing-resistant MFA Microsoft Mechanics](https://www.youtube.com/watch?v=36nIaSBJ7_U)  

[How to enable passkeys (FIDO2) in Microsoft Entra ID](https://learn.microsoft.com/en-us/entra/identity/authentication/how-to-authentication-passkeys-fido2)  

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

# What is SIM swapping?

SIM swapping is a type of fraud where a criminal tricks a mobile phone carrier 
into moving your phone number to a new SIM card that they control. [1] 

## How It Works

* Data Gathering: Scammers collect your personal information (like your name, address, or date of birth) from data breaches, phishing, or social media. [2, 3] 
* Impersonation: The criminal contacts your mobile provider, pretends to be you, and claims they lost or damaged their phone. [1, 2, 4] 
* Number Transfer: If the carrier falls for the trick, they deactivate your real SIM card and link your phone number to the scammer's SIM card. Your phone instantly loses service. [2, 3, 4, 5] 

## Why It Is Dangerous

* Intercepting Codes: The scammer receives all your phone calls and text messages, including SMS text-based two-factor authentication (2FA) codes and password reset links. [1, 3, 6] 
* Account Takeover: With these security codes, they can log into your email, bank accounts, and cryptocurrency wallets, locking you out and stealing your funds or identity. [2, 3] 

## How to Protect Yourself

* Avoid SMS 2FA: Use authenticator apps (like Google Authenticator or Authy) or physical security keys instead of text messages for two-factor authentication.
* Add a PIN/Passcode: Set up a verbal password or extra PIN with your mobile carrier so they cannot change your SIM without it.
* Limit Personal Info Online: Share fewer personal details on social media to reduce the data scammers can use to impersonate you. [2, 7, 8] 


[1] [https://www.youtube.com](https://www.youtube.com/watch?v=aRfi66WK2U0&t=50)
[2] [https://www.trendmicro.com](https://www.trendmicro.com/en_us/what-is/cyber-attack/types-of-cyber-attacks/sim-swapping-scams.html)
[3] [https://www.youtube.com](https://www.youtube.com/watch?v=bs7v6Me5rHg)
[4] [https://www.youtube.com](https://www.youtube.com/watch?v=ZjSVdOSMGMs&t=59)
[5] [https://www.bbva.it](https://www.bbva.it/blog/cybersicurezza/consigli-sulla-sicurezza-/sim-swapping-e-come-difendersi.html)
[6] [https://en.wikipedia.org](https://en.wikipedia.org/wiki/SIM_swap_attack)
[7] [https://www.fastweb.it](https://www.fastweb.it/fastweb-plus/digital-dev-security/sim-swap-cose-quali-pericoli-come-difendersi/)
[8] [https://www.aba.com](https://www.aba.com/advocacy/community-programs/consumer-resources/protect-your-money/sim-swapping-scams)


---

# How does a phishing email work?

A phishing email works by tricking you into trusting a fake message 
so you will share personal details or open harmful files. [1] 
· 1970 M01 1

## The Phishing Process

Phishing attacks generally follow a simple four-step cycle:

* Impersonation: The scammer copies the look, logo, and style of a real company like a bank, a delivery service, or your workplace. [1] 

* Urgency: The message invents a false emergency, such as a locked account, a missed package, or an urgent bill, to make you panic and act fast without thinking. [2, 3] 

* The Trap: The email includes a button, a web link, or an attached file. [4] 

* The Theft: If you click the link, it takes you to a fake website that looks real but steals your password when you try to log in. If you open an attachment, it can download harmful software (malware) onto your device. [1, 2, 5] 

## Common Tactics

* Fake Links: The text on the screen looks like a real web address, but it actually points to a scam site.

* Malicious Attachments: Files like invoices or PDFs hide scripts that install viruses or ransomware.

* Spear Phishing: Some emails are heavily customized using details from your social media to target you specifically. [1, 3, 6, 7] 

## How to Stay Safe

* Check the Sender: Look closely at the actual email address, not just the display name, for misspelled words or strange domains.

* Do Not Panic: Scammers rely on urgency to bypass your good judgment.

* Go Direct: Never click links in unexpected emails. Instead, open your browser and go straight to the official website or app to check your account status. [3, 4, 7, 8] 


[1] [https://www.microsoft.com](https://www.microsoft.com/en-us/security/business/security-101/what-is-phishing-email)
[2] [https://www.youtube.com](https://www.youtube.com/watch?v=x0xn3s7a-Rc)
[3] [https://www.youtube.com](https://www.youtube.com/watch?v=Xy-z-q37p2s&t=246)
[4] [https://me-en.kaspersky.com](https://me-en.kaspersky.com/resource-center/preemptive-safety/phishing-email)
[5] [https://www.youtube.com](https://www.youtube.com/watch?v=Sg1s-MCZowQ)
[6] [https://www.reddit.com](https://www.reddit.com/r/hacking/comments/1eq0uzs/how_does_phishing_really_work/)
[7] [https://www.securitymetrics.com](https://www.securitymetrics.com/blog/7-ways-recognize-phishing-email)
[8] [https://support.google.com](https://support.google.com/mail/answer/8253?hl=en)

---

# What is a phishing proxy?

A phishing proxy is a malicious intermediary server that intercepts communication 
between a user and a real website to steal login credentials and session tokens 
in real time. [1] 

## How a Phishing Proxy Works

* The Interception: The server acts as a middleman. It is also known as an Adversary-in-the-Middle (AiTM) attack.

* Real-Time Relaying: The proxy shows the user the real, legitimate login page for services like Microsoft 365 or Google.

* Data Capture: As the user types their username, password, and multi-factor authentication (MFA) code, the proxy records them.

* Session Hijacking: The proxy forwards the login data to the real site, lets the user log in successfully, and steals the resulting session cookie. [1, 2, 3, 4, 5] 

## Why Phishing Proxies Are Dangerous

* Bypasses MFA: Standard two-factor codes or push notifications are defeated because the user completes them normally, handing a valid session key straight to the attacker. [4] 

* No Lookalike Needed: Victims visit a page that seamlessly loads the real destination, making security warnings and fake-domain detectors harder to trigger. [1, 4] 

* Industrialized Tools: Cybercriminals use commercial toolkits like [EvilProxy](https://www.resecurity.com/blog/article/evilproxy-phishing-as-a-service-with-mfa-bypass-emerged-in-dark-web), Modlishka, and Muraena to launch these attacks at scale. [4, 6] 

## How to Protect Against Them

* Use Phishing-Resistant MFA: Switch to hardware security keys like YubiKeys that use FIDO protocols, which do not work on fake proxy endpoints. [2] 

* Monitor Session Anomalies: Watch for sudden changes in user location combined with valid session tokens. [5] 

* Browser Security: Deploy advanced security tools that analyze network traffic and block suspicious proxy domains before login forms load. [2, 4] 


[1] [https://codesealer.com](https://codesealer.com/blog/phishing-proxies)
[2] [https://www.menlosecurity.com](https://www.menlosecurity.com/blog/evilproxy-phishing-attack-strikes-indeed)
[3] [https://www.onoratoinformatica.it](https://www.onoratoinformatica.it/exploit/evilproxy-phishing-tutto-quello-che-devi-sapere/)
[4] [https://www.memcyco.com](https://www.memcyco.com/stop-reverse-proxy-phishing/)
[5] [https://www.doppel.com](https://www.doppel.com/doppel-pedia/what-reverse-proxy)
[6] [https://www.resecurity.com](https://www.resecurity.com/blog/article/evilproxy-phishing-as-a-service-with-mfa-bypass-emerged-in-dark-web)
