# HomelabOS

Project for your home server.

Activity is declining - one commit per month. Most of the roles are 2-3 years old (not updated unless they break). Luckily, the project
got some traction and lured volunteers that provide new roles (of questionable quality).

The project has set up quite nicely authentication, mail and optional user management with Authelia, OpenLDAP and mailu. The current
problem is no interconnection of those services - no central user management. There are apps, that support proxy authentication
(such as Miniflux) but it isn't built in the Homelabs' role. Most of the roles are very basic. Luckily, the central user management
can be added.

There are two biggest downside of the project and both comprise its execution model

1. Everything is run by root hence security is in stake (easily fixable IMHO)
2. Wasting resources - there is an ingenious folders sharing but no database and other programmable storages sharing. Every role must launch its own DB/S3 to support its needs (hardly fixable - without lot of additional changes that brings the project much closer to Lokal architecture).

The biggest drawback is the lack of higher quality roles. Since we need a multi-user server then we would use OpenLDAP and Authelia
to start with and any role that supports external user accounts either via OAuth, LDAP or proxy authentication. Having to manage
user accounts per-app is simply not doable.
