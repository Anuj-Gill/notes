#tech-notes 

**Supabase Auth internally uses GoTrue, which always issues these two tokens Refresh & Access Token**

Why this approach??
because JWT can not be invalidated easily, so make the JWT short lived and then use refresh token to invalidate both access token(jwt) & refresh token


#### OAuth
Has 2 main flows: Implicit and PKCE

#### Implicit flow
- The provider redirects back to your site **with the access token directly in the URL fragment** (`#access_token=...`).
- Having the Access token exposed int he URL is not safe.

#### PKCE (Authorization Code + Code Verifier)
- The provider redirects back with a **temporary authorization code**, not the token.  
- Your app must then exchange the code for real tokens.
	###### Steps
	- Your client creates a **code_verifier** (random string)
	- Creates a **code_challenge** = hash of the verifier
	- Sends `code_challenge` in the initial OAuth redirect
	- Provider returns `code`
	- Browser calls `supabase.auth.exchangeCodeForSession(code)`
	- Supabase server validates code + verifier and returns access+refresh tokens
The whole PKCE flow also happens on client side only, it's safe because of the very short live Auth code and the verifier