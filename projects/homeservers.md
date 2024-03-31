# HomelabOS

Project for your home server.

Activity is declining - one commit per month. Most of the roles are 2-3 years old (not updated unless they break). Luckily, the project
got some traction and lured volunteers that provide new roles of questionable quality.

The project has set up quite nicely authentication, mail and optional user management with Authelia, OpenLDAP and mailu. The current
problem is no interconnection of those services - no central user management. There are apps, that support proxy authentication
(such as Miniflux) but it isn't built in the Homelabs' role. Most of the roles are very basic. Luckily, the central user management
can be added.

What I see as the biggest downside of HomelabOS is its execution model. Everything is run by root hence security is easily breached.
The other is wasting with resources - there is ingenious folders sharing but no database and other programmable sotrages sharing.
Every role must launch its own DB/S3 to support its needs.
