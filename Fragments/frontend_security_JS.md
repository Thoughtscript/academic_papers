# React Security Notes

1. I don't recommend using *localStorage* nor even *sessionStorage* for credentials.
1. I recommend using an *encapsulated state store*:
   1. Don't let the variable be GLOBAL. It will then be nested in the app.
   1. Uglify the variable names so it'd difficult to discern what they are.
   1. It's not in *localStorage* so its location isn't predictable before-hand (or at least it's less so).
   1. It's in-memory, so garbage collection of the variable is predictable.
1. However, it's essential to note that this is still not **100% totally hack-proof and invulnerable by itself**. 
   * Given the number of moving parts within complex client applications, it's highly probably that there's some vulnerability even if we do everything perfectly. 
   * In general, the best philosophy is to strive for total security but to also take the steps necessary to mitigate any potential breaches. This a recommended approach I think others have called the *dual-approach* to security. 
     *  "Don't just aim for perfection, prepare for failure." (Me)
1. Supplementary requirements:
   1. **Always HTML escape user-inputted text/characters**. 
   1. **Always escape characters for any user input (for any language - e.g. SQL)**.
   1. **Always validate user-inputted text (e.g. for executable keywords in SQL scripts or when parsing text to be or that might be executed as JavaScript)**.
   1. Doing the above, prevents XSS vulnerabilities.
   1. Wipe all in-memory and browser stores on application close and after a certain time period of user-inactivity.
   1. Hash or SALT any credential transmitted between a server and a client (whether SSL or HTTPS is used). When encrypting the credential, mix in a time-dependent *nonce*. When the credential is decrypted and checked, the time dependent variable can be extracted to determine whether it's still valid or not. Here's an algorithm:
       1. Start with three strings: **password**, a **fixed symbol**, and a **timestamp**. 
       1. Concatenate these into one string and apply a SALT, then an encryption method.
       1. Decrypt the string by applying the decryption method (the "reverse" of the encryption method), then breaking the string into its original three parts.
       1. You can use an intermediate server to handle submitted user inputs since some SALT or encryption method would be revealed in the browser. The intermediate server has access to the protected resource
       1. The general approach here is often used in tandem with OAuth 2.0 or part of an OAuth security architecture (minus tokens, etc.).
       1. You can leverage both OAuth 2.0 and your own custom approach, plus two-factor to vastly improve client-access security.
    1. A posssible, though untested approach, mitigates some of the user complexity above by using IP addresses, email addresses, or phone numbers to prevent any required user input. Instead of a password, the fixed unique identifier (not just an *identifier* but a *delivery system*) is used. This is used in magic-passwords, password recovery, and some kinds of two-factor authentication. It can be used to **supplement** other security systems and mitigate access through password hacks. If a password is cracked without two-factor, all information is obtained. 
1. This is not a comprehensive document but will improve security.