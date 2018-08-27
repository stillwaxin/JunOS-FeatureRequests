# Interface Tags

## FEATURE / ENHANCEMENT REQUEST: Interface Tags

Please implement a feature such that one can apply one or more tags to an interface and have the interface apply at various other parts of config.

Example of such usage:
	set interfaces xe-1/2/0.0 tag CORE
	set interfaces xe-1/2/0 tag CORE-PHY
	set interfaces ae0.0 tag CORE
	set interfaces ae0 tag CORE-PHY
	
	set protocols ospf area 0.0.0.0 interface inherit-tag CORE
	set protocols ldp interface inherit-tag CORE
	set protocols rsvp interface inherit-tag CORE

For each of the protocols the interfaces are applied as if they were manually entered. Basically whereever JunOS would expect "interface <\*>" then I would expect it to accept "inherit-tag <tag>" in the place of the int name. There should be a command to display the results in the same manner as "display inheritance" or update display inh to show this expansion as well. Also ensure that it is possible to embed the tag commands inside groups for use in apply-groups.

Also operational commands should be extended to support tags such as :
	show interfaces tag CORE-PHY
