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
   1. See below.
1. This is not a comprehensive document but will improve security.


## More on Time-dependent variables

Hash or SALT any credential transmitted between a server and a client (whether SSL or HTTPS is used). When encrypting the credential, mix in a time-dependent *nonce*. When the credential is decrypted and checked, the time dependent variable can be extracted to determine whether it's (still) valid or not.

Here are three versions. They can also be combined.

### First way:

1. Start with three strings: **password**, a time-dependent **nonce** or **timestamp**, and a **unique identifier**. Assume the presence of a time-dependent attribute associated with the credential in the backend.
1. Concatenate these into one string and apply a SALT, then an encryption method.
1. Decrypt the string by applying the decryption method (the "reverse" of the encryption method), then breaking the string into its original three parts. 
1. You can then validate the timestamp since the last check-in. You can force the user to reauthenticate (even if the password is still valid) or refresh, if the supplied credential is beyond the allowed time-frame. The credential is also inherently time-dependent (the randomly generated **nonce** which exists in a state of perfect secrecy clientside). The **nonce** itself can be validated against the timestamp.
1. The **unique identifier** (an IP address, client key, location, etc.) is used to validate the credential in addition to the password. In many systems the **unique identifier** is a public key or client credential.

### Second way:

1. Start with three strings: **password**, a **randomly generated string "key"**, and the **last "key"**. Assume the presence of a time-dependent attribute associated with the credential in the backend.
1. Concatenate these into one string and apply a SALT, then an encryption method.
1. Decrypt the string by applying the decryption method (the "reverse" of the encryption method), then breaking the string into its original three parts. 
1. You can then validate the timestamp (since the last check-in. The credential is also inherently time-dependent (the randomly generated "key" which exists in a state of perfect secrecy clientside) and can trigger a reauthentication attempt if it's not valid. 
1. An intermediate server stores the last key. Keys are validated in addition to the password and timestamp.

### Third way (most common - see the Marvel Comic API):

1. Start with three strings: **password**, a time-dependent **nonce** or **timestamp**, a **secret key**. 
1. Concatenate these into one string and apply a SALT, then an encryption method.
1. Decrypt the string by applying the decryption method (the "reverse" of the encryption method), then breaking the string into its original three parts. 
1. You can then validate the timestamp since the last check-in. You can force the user to reauthenticate (even if the password is still valid) or refresh, if the supplied credential is beyond the allowed time-frame. The credential is also inherently time-dependent (the randomly generated **nonce** which exists in a state of perfect secrecy clientside). The **nonce** itself can be validated against the timestamp.
1. The **secret key** is used to validate the credential in addition to the password. 

### All ways:

1. The core idea above is that someone's accessing a credential that's transmitted over HTTPS doesn't automatically get access. Knowing what the encrypted credential is becomes much more difficult since the time-dependent variable is unknown. Furthermore, knowing the password doesn't immediately grant access since there are other validation requirements (and aside from two-factor, etc.) to just gain password-based authorization.
1. You can use (and probably should use) an intermediate server to handle submitted user inputs since some SALT or encryption method would be revealed in the browser. The intermediate server has access to the protected resource through an API. The protected resource returns the protected content to the client. The intermediate server can verify that IP addresses match both when being supplied a credential (going to the protected resource) and when the protected resource responds.
1. By 2, above, I mean that there are two layers of decryption. One from server to intermediate server, one from intermediate server to protected resource. Most data systems hash their persisted data anyway. 
1. The general approach described in the examples above is often used in tandem with OAuth 2.0 or as part of an OAuth security architecture (minus tokens, etc.). In other words, the general structure of the approaches above should be familiar to those who have worked with OAuth security architectures.
1. You can leverage both OAuth 2.0, your own custom approach, and two-factor to vastly improve client-access security.

A posssible, though untested approach, mitigates some of the user complexity above by using IP addresses, email addresses, or phone numbers to prevent any required user input. Instead of a password, the fixed unique identifier (not just an *identifier* but a *delivery system*) is used. This is used in magic-passwords, password recovery, and some kinds of two-factor authentication. It can be used to **supplement** other security systems and mitigate access through password hacks. If a password is cracked without two-factor, all information is obtained. This is purely hypothetical at this point. You might, for instance, get two SMS messages from two seperate services both with slightly different validation requirements.