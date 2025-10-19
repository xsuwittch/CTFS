# PicoCTF — Crack-the-Gate-1 — Easy
**Date:** 2025-10-20  
**Author:** Ammar Rana | GitHub: @xsuwittch

---

## TL;DR
Found a developer comment containing a ROT13-encoded note that revealed a temporary bypass header. Adding `X-Dev-Access: yes` to the login request returned the flag.  

---

## Challenge Description
We’re investigating a person of interest who may be hiding sensitive data inside a restricted web portal. The email `ctf-player@picoctf.org` is known, but the password is not. The prompt hints that the developer may have left a “secret way in.” The instance provides the web login form and extra details after launching the challenge.

**Objective:** Obtain the flag from the restricted portal response.

---

## Lab / Environment 
- Access method: Challenge instance (launched via PicoCTF UI / local instance)    
- Tools used: Burp Suite (intercept & inspect), Firefox (browser), dCode/online ROT13 tool.

---

## Recon / Initial steps
1. Launched the challenge instance and opened the login page in Firefox.  
2. Attempted a normal login with `ctf-player@picoctf.org` + random password (e.g., `123`) to observe the request/response.  
3. Intercepted the request in Burp Suite to inspect server responses and any hidden fields/comments.

## Enumeration / Decoding

While inspecting the HTTP response (page source) in Burp I found a developer comment that looked like gibberish:

```text
ABGR: Wnpx - grzcbenel olcnff: hfr urnqre "K-Qri-Npprff: lrf"
```
I then used dCode to analyze the encryption which turned out to be ROT-13 and decrypted it to:
```text
NOTE: Jack - temporary bypass: use header "X-Dev-Access: yes"
```
## Exploitation / Getting the Flag
1. Open the login page in Firefox and submit the form once to generate the POST request.  
2. Open **Developer Tools → Network**, find the login POST entry, right-click it and choose **Edit and Resend** (or use a header-editing extension).  
3. Add the header:
4. Resend the request. The server returned a `200 OK` response and the response body contained the flag.
```text
Response: {"success":true,"email":"ctf-player@picoctf.org","firstName":"pico","lastName":"player","flag":"picoCTF{brut4_f0rc4_49d1d186}"}
```

