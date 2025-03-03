---
id: auth-gotrue
title: 'Part Four: GoTrue'
description: 'Supabase Deep Dive Part 4: Gotrue Overview'
---

### About

How to restrict table access to authenticated users, row level policies, and email domain based access.

### Watch

<div class="video-container">
<iframe src="https://www.youtube.com/embed/neqfYym_84k" frameBorder="1" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture" allowFullScreen></iframe>
</div>

### Gotrue Server

Gotrue is an auth API server written in Go by the Netlify team, find the Supabase fork here: https://github.com/supabase/gotrue. The list of available API endpoints is available [here](https://github.com/supabase/gotrue#endpoints).

When you deploy a new Supabase project, we deploy a new instance of this server alongside your database, and also inject your database with the required `auth` schema.

It makes it super easy to, for example, send magic link emails which your users can use to login:

```bash
# replace <project-ref> with your own project reference
# and SUPABASE_KEY with your anon api key
curl -X POST 'https://<project-ref>.supabase.co/auth/v1/magiclink' \
-H "apikey: SUPABASE_KEY" \
-H "Content-Type: application/json" \
-d '{
  "email": "someone@email.com"
}'
```

Gotrue is responsible for issuing access tokens for your users, sends confirmation, magic-link, and password recovery emails (by default we send these from a Supabase SMTP server, but you can easily plug in your own inside the dashboard at Auth > Settings) and also transacting with third party OAuth providers to get basic user data.

The community even recently built in the functionality to request custom OAuth scopes, if your users need to interact more closely with the provider. See the scopes parameter here: [https://github.com/supabase/gotrue#get-authorize](https://github.com/supabase/gotrue#get-authorize).

So let's say you want to send emails on behalf of a user via gmail, you might request the gmail.send scope by directing them to:

```
https://sjvwsaokcugktsdaxxze.supabase.co/auth/v1/authorize?provider=google&https://www.googleapis.com/auth/gmail.send
```

You'll have to make sure your google app is verified of course in order to request these advanced scopes.

[Gotrue-js](https://github.com/supabase/gotrue-js) (and also [gotrue-csharp](https://github.com/supabase/gotrue-csharp), [gotrue-py](https://github.com/j0/gotrue-py), [gotrue-kt](https://github.com/supabase/gotrue-kt), and [gotrue-dart](https://github.com/supabase/gotrue-dart)) are all wrappers around the gotrue API endpoints, and make for easier session management inside your client.

But all the functionality of gotrue-js is also available in supabase-js, which uses gotrue-js internally when you do things like:

```jsx
const { user, session, error } = await supabase.auth.signIn({
  email: 'example@email.com',
  password: 'example-password',
})
```

If you want to request a feature, or contribute to the project directly, just head to https://github.com/supabase/gotrue and open some issues/PRs, we're always open to help.

In the next guide we'll be looking at how to setup external OAuth providers: Watch [Part Five: Google Oauth](../../learn/auth-deep-dive/auth-google-oauth)

### Resources

- JWT debugger: https://jwt.io​

### Next steps

- Watch [Part One: JWTs](../../learn/auth-deep-dive/auth-deep-dive-jwts)
- Watch [Part Two: Row Level Security](../../learn/auth-deep-dive/auth-row-level-security)
- Watch [Part Three: Policies](../../learn/auth-deep-dive/auth-policies)
<!-- - Watch [Part Four: GoTrue](../../learn/auth-deep-dive/auth-gotrue) -->
- Watch [Part Five: Google Oauth](../../learn/auth-deep-dive/auth-google-oauth)
- Sign up for Supabase: [app.supabase.com](https://app.supabase.com)
