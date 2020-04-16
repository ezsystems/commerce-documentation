# Email server

The GDPR requests to handle emails in a secure way.

We recommend to configure a secure STMP transport:

parameters.yml:

``` yaml
mailer_host: <your mail server>
mailer_port: 465
mailer_encryption: ssl
#the following stream_options configuration is required only if the mail server uses a self signed certificate
mailer_stream_options:
    ssl:
        verify_peer: false
        verify_peer_name: false
```

Required configuration for swiftmailer:

``` yaml
swiftmailer:
...
    port: '%mailer_port%'
    encryption: '%mailer_encryption%'
    stream_options: '%mailer_stream_option%'
```
