Implementing the system as described on the next few pages will only protect
your data from being compromised should the machine be stolen. Should the
physical security of the machine be compromised, then none of the preparations
detailed in this document will continue to protect your data. The steps
outlined in this document are also not designed to protect the computer from
attacks that come from the network the computer is connected to.

In computer security, there's really only one way you can secure a computer:

Assemble a computer with no connections to the outside world whatsoever,
beyond the normal devices that you would use to physically interact with the
computer (monitor, keyboard, mouse, etc.).

Even without connections to other computers, a computer that's set up like
this is not totally 100% safe from being compromised (see Van Eck Phreaking or
TEMPEST sheilding on Wikipedia for examples of why no computer is 100% safe
from being compromised).

Obviously, a computer without connectivity to other computers is generally not
as useful to multiple users as one that is connected to other computers. Most
likely, the data you wish to protect is on a computer that has connections to
other computers of some type, most likely an Internet connection of some kind.
Web servers, file servers, e-mail (SMTP) servers have all become services that
any networked computer can use, but these services also present ways for an
attacker to gain access to the system in question (attack vector). Should a
server application running on a machine using disk encryption be compromised
in some way, the attacker could then possibly read the data on the disk that
is protected by encryption, including the key used to encrypt the disk, if it
is available to the attacker.

Data can also be compromised by users with the correct authorization, who then
use their access to steal or taint the data that they have access to.

See also the Wikipedia article on Computer Security
