- the system DNS resolver (mDNSResponder) supports domain-specific configuration via files in /etc/resolver/
	- When you create a file like /etc/resolver/ts.net containing:
	- ```
	  nameserver 100.100.100.100
	  ```
	- macOS automatically routes all DNS queries for domains ending in .ts.net (your Tailscale MagicDNS suffix) directly to Tailscale's built-in DNS proxy running locally at 100.100.100.100.