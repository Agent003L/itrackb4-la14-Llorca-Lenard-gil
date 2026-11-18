# LA14: Locking the Front Door

## F1: Authentication vs authorisation

This lesson gave my movies app authentication: visitors can register, log in and log out, so the app knows who they are. What is still missing is authorisation: right now any signed-in user can edit or delete any movie, and nothing decides what each user is allowed to do.

## F2: session()->regenerate()

After a successful login, `regenerate()` gives the visitor a new session id. If I skipped it, someone who planted a known session id on a visitor's browser before login would still hold the id of the now signed-in session (session fixation).

## F3: One message for both login failures

My login shows the same message for a wrong password and an unknown email. If the messages differed, a stranger could test email addresses and learn which ones are registered in my app.

## F4: Public index and show

`index` and `show` stay public because browsing the movie catalogue is the point of the app and changes nothing. `create`, `edit`, `update` and `destroy` change data, so they use `auth` to make sure only a signed-in user can reach them.

## F5: Hiding the buttons

Hiding Add, Edit and Delete only removes the links; the addresses still exist, and a guest can type `/movies/create` directly (my screenshot 19). What actually stops a stranger is the `auth` middleware on those routes, which redirects them to login.

## F6: Why the route must be named login

The `auth` middleware sends guests to the route named `login`. When I renamed my GET login route to `signin` (screenshot 20), the redirect failed with "Route [login] not defined", so the name has to match exactly.