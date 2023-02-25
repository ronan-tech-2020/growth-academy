# Basic Introduction of SameSite Policy

[Miro link](https://miro.com/app/board/uXjVPM_Rg8s=/?moveToWidget=3458764546587041112&cot=14)

## Context

I work on a ticket to embed a knowledge center application (an iframe) to the main application. When the app runs on the staging environment, no actual content has been showed but a message requests user to log in. However, the link of the knowledge center app works fine if I open it in a separate tab.

This makes me think why. From page requests, I see a few redirects but nothing special. While I go into details, there's a yellow sign mentions that SameSite is not fit in current scenario.

## Root cause

The knowledge center is not in the same domain with the main app, so all requests initiated by it are treated as cross site requests. By default, SameSite value is set to `Lax,` which means no cookie will be sent along if request is not initiated by the host app. So when the knowledge center server receives those requests, no user cookie included. The user will be labelled as unauthenticated, and redirect to the log in page.

## Solutions

Set-cookie header should include `SameSite=None; Secure`.

## Potential issues

Expose apps in the threat of CSRF, as any external link can get cookies from the app.

## References

- https://owasp.org/www-community/SameSite#:~:text=SameSite%20prevents%20the%20browser%20from,none%20%2C%20lax%20%2C%20or%20strict%20
- https://developer.mozilla.org/en-US/docs/Web/HTTP/Headers/Set-Cookie/SameSite
- https://web.dev/samesite-cookies-explained/

## TODOs

- Set up a quick PoC to include both not working and working scenarios
- Think about can we further restrict which domain the cookie can be sent along

## Further readings/questions:

- CSRF (Cross Site Request Forgery)
