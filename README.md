Installs and configures postfix to relay e-mails to an external service like mailgun, mailchimp, sendgrid...

IMO It's usually better to use local postfix as a spool than to use the API or directly SMTP from app but YMMV.

Currently only one relay is supported but postfix can route e-mails based on `envelope from` host (`relayhostsby`), shall be easy to adapt.

Vars:
 * `postfix_relay_host`
 * `postfix_relay_username`
 * `postfix_relay_password`
 * `postfix_relay_port` 
 
# Multihost mail routing notes

In `main.cf`: `sender_dependent_relayhost_maps = hash:/etc/postfix/relayhostsby`

In `/etc/postfix/relayhostsby`:
```
@fromhost1 [someotherextrnalservice]
@fromhost2 [smtp.mandrillapp.com]:587
@fromhost3 [smtp.mandrillapp.com]:587
```

In `/etc/postfix/sasl_passwd`:
```
@fromhost1 hostuser:hostpass
@fromhost2 mandrilluser:mandrillpass
@fromhost2 mandrilluser:mandrillpass
```
 
 
Some other opts that probably solve some problems:
```
sender_dependent_relayhost_maps = hash:/etc/postfix/relayhostsby
smtp_sender_dependent_authentication = yes
smtp_sasl_password_maps = hash:/etc/postfix/sasl_passwd
smtp_sasl_auth_enable = yes
smtp_use_tls=yes
smtp_tls_CAfile = /etc/pki/tls/certs/ca-bundle.crt
smtp_tls_security_level = may
smtpd_tls_received_header = yes
tls_random_source = dev:/dev/urandom
smtpd_tls_security_level = may
smtp_use_tls = yes
smtp_sasl_security_options = noanonymous
```
 
