# Lab: Visible Error-Based SQL Injection

![screen](images/lab13_1.png)

---
## Objective
Exploit a SQL injection vulnerability to:
- Trigger database errors that reveal sensitive data
- Extract the administrator password from error messages
- Log in as the administrator user

---

## Lab Overview

In this lab:
- The application returns **visible database error messages**
- These errors include **query results inside them**
- We can exploit this to directly extract sensitive data

## This is known as **Visible Error-Based SQL Injection**

---

## Step 1: Identify Injection Point

Intercept the request and locate the `TrackingId` cookie:


### Test basic injection:
![screen](images/lab13_1.png)

### Result:
- Application returns a **database error**

### Explanation:
The query becomes invalid:
because we added  ' 

### visible error- based sql injection  confirmed

---

### Extract Data using CAST() function

#### CAST() function used to convert from data type to other datatype
#### CAST(200 AS CHAR) convert from integer to string 

#### CAST('hi' AS INT) here will give us an input syntax error:
ERROR: invalid input syntax for type integer: "hi"

#### so we noticed that we can use cast and make error to extract sensitive data

![screen](images/lab13_2.png)

![screen](images/lab13_3.png)

### so lets extract the password from users table for administrator user:

![screen](images/lab13_4.png)

#### again sql error returned 
#### notice that our query truncated because of character limit 

#### so lets empty id value 

![screen](images/lab13_5.png)

--- 

### we have another way:

#### lets get username for the first row since it often the first row administrator user
#### PAYLOAD

##### ' OR CAST(SELECT username FROM users LIMIT 1)=1--

![screen](images/lab13_6.png)

after knowing the first row username was administrator we can get the password

![screen](images/lab13_7.png)

---

### Login

![screen](images/lab13_8.png)



