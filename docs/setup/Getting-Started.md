# Getting Started

Once you are all setup and ready to go you should be able to load Organizr at [http://{% if organizr.domain %}{{ organizr.domain }}{% else %}{{ organizr.subdomain + "." + domain }}{% endif %}/](http://{% if organizr.domain %}{{ organizr.domain }}{% else %}{{ organizr.subdomain + "." + domain }}{% endif %}/).

If you see `{ domain }` in the link above, you are either viewing these docs on the hosted Github pages and not actually through VivumLab, or something is configured incorrectly.

## Securing Services

If you want to add an extra layer of authentication to a service, all you have to do is set `servicename.auth` to true. If [Authelia](../software/authelia.md) is enabled, it will handle authentication. Otherwise, Traefik will use HTTP Basic Auth with `default_username` and `default_password` as the credentials.

## File locations

VivumLab sets up `{{ volumes_root }}` on your server, which maps to `/var/vivumlab` by default

## HTTPS via LetsEncrypt

VivumLab will use Traefik's built in LetsEncrypt integration to automatically generate SSL certificates for your various services. If initially some of the certificates don't appear valid, you have likely run into [LetsEncrypt rate limits](https://letsencrypt.org/docs/rate-limits/). Unfortunately the only fix we have for that right now is 'wait a week'.

## SMTP

In order for your applications to be able to utilise email (confirmation emails, etc), you need an SMTP server. A nice free one is [Mailgun](http://mailgun.com/).

Create an account, and follow their steps to register your domain.

Once that is completed, you can plug the settings they provide into your `settings/vault.yml` file.
To do this:

1. run the **`vlab decrypt`** command
2. find and edit the `vault.yml` file
    > NOTE: editing this file may require superuser/ root permissions
3. Set the following settings to what you have been provided

```
# SMTP Settings
smtp:
  host:
  port:
  user:
  pass:
  from_email:
  from_name:
```

4. Now, edit the `settings/config.yml` file, and set:
```
mailu:
  forward_to_smtp: False
```
to ...

```
mailu:
  forward_to_smtp: True
```

This will allow mailu (and every other VivumLab service) to use the mail gun SMTP server

5. Save, and run **`vlab deploy`** to finalise any changes.

## Vivumlab Commands

`vlab -t` will list all available commands.
`vlab -h COMMAND` will show help info for the specified command.
