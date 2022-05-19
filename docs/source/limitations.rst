Known limitations
=================
When forwarding a port with the "--http-forward" option, not all incoming http requests are supported. Only GET and POST.
Websockets and other methods will not work with this option, but can be used without the --http-forward option.