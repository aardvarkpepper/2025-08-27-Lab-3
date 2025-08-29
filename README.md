## Reflection

If anything's substantially wrong below, I'd appreciate feedback.  Read some conflicting articles, so more or less this is 'best guess' given a limited time frame.

1.  CSRF and the state Parameter: In your own words, explain how an attacker could perform a Cross-Site Request Forgery (CSRF) attack on an OAuth flow. How does using the state parameter, as recommended, prevent this specific attack?

Once a user is authenticated (typically with encrypted password), 'authorization' may involve the server taking some non-sensitive user info, encrypting it, then passing that user info along with the encryption in a token back to the user.  From that point on, the user sends the token back to the server each time a request is made.

From that point, instead of the server verifying an encrypted password (requiring server to communicate with database), the server compares the encrypted user info to user info that was passed along with the encryption; if there's a mismatch then requests won't execute.

This process offers a little more security, as even if an attacker knows someone else's non-sensitive data (username and email, say), the attacker hopefully doesn't know how the server sets up its encrypt of that data.  The attacker won't normally be able to get the encrypt by passing in the username and email either, as the attacker would need to have the user's sensitive information (password) to get the token from the server in the first place.

However, there are still ways for an attacker to break in.  The user has locally stored the token, and now that token is used to authenticate any actions the user takes - the password is no longer checked.  So the attacker could get the user to activate a bit of code - could be by opening an email or opening a legitimate-seeming website, or viewing a picture - lots of possibiilities - then that causes the user to initiate a request.  Then either the attacker's request gets passed along with the authorization token, or perhaps the attacker intercepts the authorization token; in either case, the user unknowingly executes actions they didn't intend to do.  CSRF seems to be the former only, where an attacker doesn't have access to information sent back to the user; the attacker simply tries to get the user to authorize whatever code the attacker blindly attempted to have the user execute.

I think OAuth's state involves the user sending a random string as state to the app, then the app sending the random string back.  Only if those strings match, is an authentication response approved.  (It's another authentication, not authorization).  Otherwise it is denied.

So when the attacker sends their code to be executed, it doesn't match the random string specified by the user, so the authentication response is denied - and the attacker's code won't execute.

2.  Redirect URI Attacks: The article mentions that validating a redirect_uri by simply checking the domain or allowing subdomains is a common mistake. Describe a hypothetical scenario where a “leaky” redirect_uri validation (e.g., one that allows any path on a valid domain) could be exploited to steal an authorization code.

Where it comes to redirect URI attacks, 'simply' checking domain / allowing subdomains is not how I would describe the issue.  More, the general issue is, I'd say, code may be written so it can work with dynamic data - which may mean it handles errors well enough, but not deliberate attacks.

Typically users will look at the first part of a URL to see if they're on a trusted site.  But this does not account for parameters or queries.

So an attacker might set up some sort of redirect through the actual app, so after a user goes through authentication (encrypted, so ideally safe), they still send their authorization as described previously, which goes to the attacker's site, so the attacker gets the authorization code.

To avoid this issue, arguments and actions need to be strictly controlled with security kept in mind.

3.  User Experience vs. Security: Adding a third-party login option like “Login with Google” is a significant user experience improvement. However, it also introduces complexity and new potential security risks. Based on the article and your own thoughts, describe one key trade-off a development team must consider when deciding to implement OAuth. (For example, think about the balance between convenience for the user and the responsibility of the application to protect user data).

Say someone wants to use OAuth with Google so users can log in to their app with their Google account.  The user can log in with Google with a single button press, rather than having to type out and track username and password.

But now when they want to sign up to the app, they may see this program is now authorized to look at their Google pictures, their Google files, monitor their online activity - or even if that level of access is not allowed, they understand on some level their activity online can be tracked.  Users with such concerns may end up not using the app at all.

There's a trade-off right there; you get some users attracted to convenience, you lose some users that were worried about data security and/or privacy from linking their accounts.

If someone's using OAuth, the data's sort of protected, but not really protected.  It's not like fully encrypted actions requiring physical access to terminals in a secure installation.  Any data that is useful enough to protect is useful enough for someone to want to steal.  I'd say typically the decision on how much encryption to use isn't up to the developers, it's up to whoever is telling the developers what to do - legal, financial, marketing, design, and so forth.  If a developer is asked to act in those capacities, or has any say in the situation, I'd say developers should try to make sure minimal user data is used, and what is used is monitored in the authentication process, keeping security in mind.

## Resources 

https://blog.doyensec.com/2025/01/30/oauth-common-vulnerabilities.html
https://owasp.org/www-community/attacks/csrf
https://portswigger.net/web-security/csrf
https://auth0.com/intro-to-iam/what-is-oauth-2
https://auth0.com/docs/secure/attack-protection/state-parameters
https://security.stackexchange.com/questions/278234/oauth-2-0-why-is-the-state-parameter-needed-in-order-to-prevent-csrf-at-author
https://developers.google.com/identity/protocols/oauth2


## Note

Assigned reading at https://blog.doyensec.com/2025/01/30/oauth-common-vulnerabilities.html did not cover some required answers - e.g. there is no mention of "state" in the text for CSRF.

Used Gemini AI for research purposes only (no writing).  LLMs are useful for getting an overall idea and finding terms for further research, but everything has to be independently verified.  Frankly, LLMs can get things horribly wrong.