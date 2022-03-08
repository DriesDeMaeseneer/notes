# TODO:
- "Gewone gebruikers kunnen alles doen
buiten een nieuwe dataset toevoegen, dat is enkel toegestaan voor admins."


# User 

## Abilities:
- can view profile.
- can view list of current user environments.
- can edit user settings.
- can [recover](https://cheatsheetseries.owasp.org/cheatsheets/Forgot_Password_Cheat_Sheet.html) user password.
- can edit user password.
- can create new environment.
- can delete existing environment(cascade).
- can [upload](https://cheatsheetseries.owasp.org/cheatsheets/File_Upload_Cheat_Sheet.html) data set to environment.
- can stop upload of the data set.
- can view data set entries.
- can create new algorithm.
- can edit excising algorithm.
- can remove user defined algorithm.
- can start A/B test.
- can stop A/B test in progress.
- can view A/B test result page.
- can remove A/B test(cascade).

## Permissions:
- should be state-full([enforce least privileges](https://cwe.mitre.org/data/definitions/272.html)).
- embedded in jsonwebtoken.

## Limits:
- can only access data from current user.
- has timeouts:
	+ for creating databases(aka data set).
	+ amount of concurrent A/B tests.
- has limits:
	+ amount of databases.
	
## When Created:
- hash password with salt and pepper([argon2id](https://cheatsheetseries.owasp.org/cheatsheets/Password_Storage_Cheat_Sheet.html#argon2id)).
- create sample environment with H&N data set.

## When Logged In:
- encrypt jsonwebtoken cookie([rocket feature](https://api.rocket.rs/v0.4/rocket/http/enum.Cookies.html#method.add_private)).
- decrypt user datasets and user created algorithms using user [secret](https://www.postgresql.org/docs/10/encryption-options.html).

# Admin

## Abilities
- can view all user profile.
- can view list of all environments.
- can view specific user settings.
- can view user passwords.
- can delete existing environment(cascade).
- can view all current data set uploads.
- can stop upload of the data set.
- can view data set entries.
- can edit excising algorithm.
- can remove excising algorithm.
- can view all A/B tests in progress.
- can stop A/B test in progress.
- can view A/B test result page.

## Permissions:

- should be state-full([enforce least privileges](https://cwe.mitre.org/data/definitions/272.html)).
- embedded in jsonwebtoken.

## Limits:
- can access all data from every user.
- has no timeouts.
- has no limits.

# Requests:
- Every request:
	+ require [https](https://cheatsheetseries.owasp.org/cheatsheets/REST_Security_Cheat_Sheet.html#https).
	+ check [permissions](https://cheatsheetseries.owasp.org/cheatsheets/Authorization_Cheat_Sheet.html#validate-the-permissions-on-every-request)(jwt/middle-ware).
	+ [log](https://cheatsheetseries.owasp.org/cheatsheets/Logging_Cheat_Sheet.html) everything.
	+ when recieving user input, [validate content](https://cheatsheetseries.owasp.org/cheatsheets/REST_Security_Cheat_Sheet.html#input-validation).
	+ when accessing the database, use per user [permissions](https://cheatsheetseries.owasp.org/cheatsheets/Database_Security_Cheat_Sheet.html#permissions).
	+ communicate [securely](https://cheatsheetseries.owasp.org/cheatsheets/Database_Security_Cheat_Sheet.html#connecting-to-the-database) with the database.
	+ send [CSRF token](https://cheatsheetseries.owasp.org/cheatsheets/Cross-Site_Request_Forgery_Prevention_Cheat_Sheet.html#synchronizer-token-pattern) with rust [CSPRNG](https://docs.rs/rand/0.5.0/rand/prng/chacha/struct.ChaChaRng.html).