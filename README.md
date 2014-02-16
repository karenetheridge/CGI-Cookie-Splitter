# NAME

CGI::Cookie::Splitter - Split big cookies into smaller ones.

# VERSION

version 0.04

# SYNOPSIS

    use CGI::Cookie::Splitter;

    my $splitter = CGI::Cookie::Splitter->new(
        size => 123, # defaults to 4096
    );

    @small_cookies = $splitter->split( @big_cookies );

    @big_cookies = $splitter->join( @small_cookies );

# DESCRIPTION

RFC 2109 reccomends that the minimal cookie size supported by the client is
4096 bytes. This has become a pretty standard value, and if your server sends
larger cookies than that it's considered a no-no.

This module provides a pretty simple interface to generate small cookies that
are under a certain limit, without wasting too much effort.

# METHODS

- new %params

    The only supported parameters right now are `size`. It defaults to 4096.

- split @cookies

    This method accepts a list of CGI::Cookie objects (or look alikes) and returns
    a list of CGI::Cookies.

    Whenever an object with a total size that is bigger than the limit specified at
    construction time is encountered it is replaced in the result list with several
    objects of the same class, which are assigned serial names and have a smaller
    size and the same domain/path/expires/secure parameters.

- join @cookies

    This is the inverse of `split`.

- should\_split $cookie

    Whether or not the cookie should be split

- mangle\_name\_next $name

    Demangles name, increments the index and remangles.

- mangle\_name $name, $index
- demangle\_name $mangled\_name

    These methods encapsulate a name mangling scheme for changing the cookie names
    to allo wa 1:n relationship.

    The default mangling behavior is not 100% safe because cookies with a safe size
    are not mangled.

    As long as your cookie names don't start with the substring `_bigcookie_` you
    should be OK ;-)

# SUBCLASSING

This module is designed to be easily subclassed... If you need to split cookies
using a different criteria then you should look into that.

# SEE ALSO

[CGI::Cookie](https://metacpan.org/pod/CGI::Cookie), [CGI::Simple::Cookie](https://metacpan.org/pod/CGI::Simple::Cookie), [http://www.cookiecutter.com/](http://www.cookiecutter.com/),
[http://perlcabal.org/~gaal/metapatch/images/copper-moose-cutter.jpg](http://perlcabal.org/~gaal/metapatch/images/copper-moose-cutter.jpg),
RFC 2109

# AUTHOR

יובל קוג'מן (Yuval Kogman) <nothingmuch@woobling.org>

# COPYRIGHT AND LICENSE

This software is copyright (c) 2006 by יובל קוג'מן (Yuval Kogman).

This is free software; you can redistribute it and/or modify it under
the same terms as the Perl 5 programming language system itself.
