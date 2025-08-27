## Reflection

Assigned reading at https://blog.doyensec.com/2025/01/30/oauth-common-vulnerabilities.html did not cover some required answers - e.g. there is no mention of "state" in the text for CSRF.

Generally, nothing is an issue given time and clear documentation.  Sometimes people won't have much time, sometimes there won't be clear documentation.

1.  CSRF and the state Parameter: In your own words, explain how an attacker could perform a Cross-Site Request Forgery (CSRF) attack on an OAuth flow. How does using the state parameter, as recommended, prevent this specific attack?



2.  Redirect URI Attacks: The article mentions that validating a redirect_uri by simply checking the domain or allowing subdomains is a common mistake. Describe a hypothetical scenario where a “leaky” redirect_uri validation (e.g., one that allows any path on a valid domain) could be exploited to steal an authorization code.



3.  User Experience vs. Security: Adding a third-party login option like “Login with Google” is a significant user experience improvement. However, it also introduces complexity and new potential security risks. Based on the article and your own thoughts, describe one key trade-off a development team must consider when deciding to implement OAuth. (For example, think about the balance between convenience for the user and the responsibility of the application to protect user data).

## Resources

https://blog.doyensec.com/2025/01/30/oauth-common-vulnerabilities.html
https://owasp.org/www-community/attacks/csrf