IP link protection
==================

 Opening a port from your pc to the internet sounds dangerous, and it is. That's why openport has build-in protection, that requires a user to know a unique token when he wants to access your port.

Let's say you want to share your ssh connection, you will get something like

    openport 22
    INFO - Now forwarding remote port openport.io:12345 to localhost:22 .
    You can keep track of your shares at https://openport.io/user .
    You are now connected. You still can transfer 10000 megabyte this month.
    Tell the one you are sending this link to, to first go here: https://openport.io/l/12345/abscsqdfklmagsdgq . This will allow his IP to contact you.

As long as the user on the other end of the line does not know this link, he cannot reach you, and so you are safe from intruders. Try it yourself!

You can also find this link in 'My Openport'>'Sessions'.

You can disable this feature using the 'Edit' button in the 'clients' tab in 'My Openport', but this is not recommended.