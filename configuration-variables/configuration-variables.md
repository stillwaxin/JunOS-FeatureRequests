# Configuration Variables

## FEATURE / ENHANCEMENT REQUEST: Configuration Variables

Please implement a feature such that configurations can use variables. Specific use case is the following:
Current config may look like this:
	set interfaces lo0.0 family inet address 192.0.2.1/32
	set routing-options router-id 192.0.2.1
	set protocols bgp group foo type internal
	set protocols bgp group foo local-address 192.0.2.1
	set routing-instances bar route-distinguisher 192.0.2.1:1234
	set routing-instances baz route-distinguisher 192.0.2.1:3641
	set routing-instances bam route-distinguisher 192.0.2.1:5349

What I would to be able to do is represent the IP address in a single location in config and for JunOS to use it as substitute where possible. Example like so:
set variables string ipv4-lo0 value 192.0.2.1

	set interfaces lo0.0 family inet address $ipv4-lo0/32
	set routing-options router-id $ipv4-lo0
	set protocols bgp group foo type internal
	set protocols bgp group foo local-address $ipv4-lo0
	set routing-instances bar route-distinguisher $ipv4-lo0:1234
	set routing-instances baz route-distinguisher $ipv4-lo0:3641
	set routing-instances bam route-distinguisher $ipv4-lo0:5349

I would expect other types to be supported including family inet / ipv4, inet6 / ipv6 addresses and integers. I used string above when perhaps ipv4 prefix would suffice but JunOS would then need handling of the prefix-len as part of this since for the application it is not used in other parts of the config. Perhaps the IP could be represented as 2 parts in that case with the address as one part and the prefix-len as another, example like so:
	set variables prefix-ipv4 ipv4-lo0 prefix 192.0.2.1
	set variables prefix-ipv4 ipv4-lo0 prefix-len 32 
or combined on one like so:
	set variables prefix-ipv4 ipv4-lo0 prefix 192.0.2.1 prefix-len 32

set interfaces lo0.0 family inet address $ipv4-lo0:prefix-len (or have JunOS be context aware and apply the prefix-len automatically without having to specify)
	set routing-options router-id $ipv4-lo0
	set protocols bgp group foo type internal
	set protocols bgp group foo local-address $ipv4-lo0
	set routing-instances bar route-distinguisher $ipv4-lo0:1234
	set routing-instances baz route-distinguisher $ipv4-lo0:3641
	set routing-instances bam route-distinguisher $ipv4-lo0:5349
