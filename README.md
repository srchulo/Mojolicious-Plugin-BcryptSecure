# NAME

Mojolicious::Plugin::BcryptSecure - Securely bcrypt and validate your passwords.

# STATUS

<div>
    <a href="https://travis-ci.org/srchulo/Mojolicious-Plugin-BcryptSecure"><img src="https://travis-ci.org/srchulo/Mojolicious-Plugin-BcryptSecure.svg?branch=master"></a>
</div>

# SYNOPSIS

    # Mojolicious::Lite

    # use the default cost of 12
    plugin 'BcryptSecure'

    # set your own cost
    plugin BcryptSecure => { cost => 8 };

    # Mojolicious

    sub startup {
      my $self = shift;

      # use the default cost of 12
      $self->plugin('BcryptSecure');

      # set your own cost
      $self->plugin('BcryptSecure', { cost => 8 })
    }

# DESCRIPTION

[Mojolicious::Plugin::BcryptSecure](https://metacpan.org/pod/Mojolicious::Plugin::BcryptSecure) is a fork of [Mojolicious::Plugin::Bcrypt](https://metacpan.org/pod/Mojolicious::Plugin::Bcrypt) with two main differences:

- [Crypt::URandom](https://metacpan.org/pod/Crypt::URandom) is used to generate the salt used in ["bcrypt"](#bcrypt) with strongest available source of non-blocking randomness on the current platform.
- ["secure\_compare" in Mojo::Util](https://metacpan.org/pod/Mojo::Util#secure_compare) is used in ["bcrypt\_validate"](#bcrypt_validate) when comparing the crypted passwords to
help prevent timing attacks.

You also may want to look at [Mojolicious::Command::bcrypt](https://metacpan.org/pod/Mojolicious::Command::bcrypt) to help easily generate crypted passwords
with your app's `bcrypt` settings via a [Mojolicious::Command](https://metacpan.org/pod/Mojolicious::Command).

# OPTIONS

## cost

A non-negative integer with at most two digits that controls the cost of the hash function.
The number of operations is proportional to 2^cost. The default value is 12.
This option is described more in [Crypt::Eksblowfish::Bcrypt](https://metacpan.org/pod/Crypt::Eksblowfish::Bcrypt).

    # Mojolicious::Lite
    plugin BcryptSecure => { cost => 8 };

    # Mojolicious
    sub startup {
      my $self = shift;

      $self->plugin('BcryptSecure', { cost => 8 })
    }

# HELPERS

## bcrypt

Crypts a password via the bcrypt algorithm and returns the resulting crypted value.

    my $crypted_password = $c->bcrypt($plaintext_password);

    # optionally pass your own settings
    my $crypted_password = $c->bcrypt($plaintext_password, $settings);

`$settings` is an optional string which encodes the algorithm parameters, as described in [Crypt::Eksblowfish::Bcrypt](https://metacpan.org/pod/Crypt::Eksblowfish::Bcrypt).

## bcrypt\_validate

Validates a password against a crypted password (from your database, for example):

    if ($c->bcrypt_validate($plaintext_password, $crypted_password)) {
        # Authenticated
    } else {
        # Uh oh...
    }

# AUTHOR

Adam Hopkins <srchulo@cpan.org>

# COPYRIGHT

Copyright 2019- Adam Hopkins

# LICENSE

This library is free software; you can redistribute it and/or modify
it under the same terms as Perl itself.

# SEE ALSO

- [Mojolicious::Command::bcrypt](https://metacpan.org/pod/Mojolicious::Command::bcrypt)
- [Crypt::Eksblowfish::Bcrypt](https://metacpan.org/pod/Crypt::Eksblowfish::Bcrypt)
- [Crypt::URandom](https://metacpan.org/pod/Crypt::URandom)
- [Mojolicious::Plugin::Bcrypt](https://metacpan.org/pod/Mojolicious::Plugin::Bcrypt)
