# CS-PasswordServiceInfo
SwatCS Password Service is a CS password service web application for CS students at Swarthmore College.

# PasswordService

Our new [CS self-service password app](https://password.cs.swarthmore.edu/), 
written with python and [flask](https://flask.palletsprojects.com/en/2.0.x/).


## About

This is a [flask](https://flask.palletsprojects.com/en/2.0.x/) web app to allow our users to:
 - reset their own password
 - change their login shell 
 - change their gecos information (their DisplayName)
Additionally, special users with admin priviledges 
can reset passwords for *other* users and see logging info on how the
service is being used. 

All user information is stored in an 
[LDAP server](https://jeffknerr.github.io/ldap/linux/debian/ubuntu/2021/04/28/ldap-for-small-department.html).
First-time users are shown a "user agreement form"
that they have to read and agree to *before* setting their 
initial account password.

**All of this is based on 
Miguel Grinberg's *awesome*
[Flask Mega-Tutorial](https://blog.miguelgrinberg.com/post/the-flask-mega-tutorial-part-i-hello-world).**
Miguel's tutorial covers logging in users, password security, resetting passwords,
database models, and a whole lot more. The main difference is we don't need user
blog posts, and our user data is stored in an LDAP server.
Our app talks to the LDAP server using the 
[FlaskLDAP3Login extension](https://flask-ldap3-login.readthedocs.io/en/latest/).

This web app was written by 
[Emma Jin](https://github.com/EmmaJin0210)
under the supervision of [Jeff Knerr](https://jeffknerr.github.io/)
during the summer
of 2020 (during a global pandemic!). We have been using it for our department
ever since. This has been *very* useful while many of our students are now
studying from remote locations.

## Details

- users can either login (if they know their password) and change certain things:
  their current password, their shell, their display name (e.g., Jeff instead of Jeffrey)
- or they can request a password reset email if they have forgotten their current password
- if resetting their password, they will get an email with a link sent to their
  campus email address (which is stored in the LDAP server)
- clicking on the link in the email will take them to a route in the app that
  allows them (assuming they have the correct token) to pick a new password
- when entering passwords, users are required to satisfy certain strength requirements
- password strength is shown using a 
  [python implementation of zxcvbn](https://github.com/dwolfhub/zxcvbn-python)
- all actions (changing password, requesting password reset, admin resetting a
  user's password, changing shell, etc) are logged
- admin users can see logging info in the app
- admin users can change/set a password for another user (in case the user
  is having trouble with the password reset email or something else)
- users are shown their "Last Login", and that information is stored in LDAP
  using a custom LDAP attribute
- new accounts, having a special "Last Login" date (1970), are required to view
  and accept a "user agreement form" before being allowed to set their password
- we use [flask-limiter](https://flask-limiter.readthedocs.io/en/stable/) to limit 
  brute-force attempts on certain routes (e.g., login attempts per day, password
  reset emails per hour, etc)
