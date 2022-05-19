Testing facebook buttons
========================

When creating facebook like buttons for your page, facebook has to be able to reach you site. Deploying to a test site is slow, and you might have to do this multiple times, just for one button.

Using openport, you can have your development site online, so facebook can find your page, and read all the "og" tags it needs.

Simply use

.. code-block::

    openport 8080 --http-forward

And change the og:url tag to match the address you are given by openport.

.. code-block::

    <meta property="og:url" content="http://abcd.u.openport.io/" />

Then visit your site at the address that is given to you (abcd.u.openport.io in the example), and your facebook buttons will work.
