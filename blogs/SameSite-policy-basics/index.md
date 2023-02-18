
# An issue with inappropriate *SameSite* value

    metadata:
    date: 18/02/2023
    tags: security, front-end security, OWASP, SameSite, browser security
    author: Ronan

## Context

I was working on a ticket to embed a knowledge center application to a main application via micro-frontend. No matter on local or staging environment, the knowledge center app didn't show actual content but a message that requested user to log in. While it didn't work inside the main application, the same link worked fine if open in a new tab.

From page requests, I saw a few redirects and nothing special and no errors. But with a closer look, the request header showed browser's yellow warning sign mentioned that SameSite was not fit in current scenario.

***todo: add the image without sensitive infor***

## Root cause

The knowledge center application is not in the same domain with the main app, so all requests initiated by the knowledge center app are treated as cross site requests. By default, SameSite policy is set to Lax, which means no cookie will be sent along with requests until user navigates to the target domain. So when the knowledge center server receives those requests, no user information is included. It can't recognize the identify of the user, so redirect user to not authorized page.

## Solutions

Ask knowledge center team to update their set-cookie header to be `SameSite=None; Secure`.

## Potential issues

Exposing the knowledge center app in threat of CSRF, because any external link can get cookies from education app authenticated user now.
*todo: to test*
potentially can be restricted to only main domain? with set-cookies "domain" field?

## Further readings/questions
- CSRF (Cross Site Request Forgery)
***todos:
set up a quick demo/prototype to include the not working and working scenarios
update article to omit sensitive info and publish to github***

## References
- https://owasp.org/www-community/SameSite#:~:text=SameSite%20prevents%20the%20browser%20from,none%20%2C%20lax%20%2C%20or%20strict%20
- https://developer.mozilla.org/en-US/docs/Web/HTTP/Headers/Set-Cookie/SameSite