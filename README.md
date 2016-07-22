Installs and configures postfix to relay e-mails to an external service like mailgun, mailchimp, sendgrid...

IMO It's usually better to use local postfix as a spool than to use the API or directly SMTP from app but YMMV.

Currently only one relay is supported but postfix can route e-mails based on `envelope from` host, shall be easy to adapt.

Vars:
 * `postfix_relay_host`
 * `postfix_relay_username`
 * `postfix_relay_password`
 * `postfix_relay_port`