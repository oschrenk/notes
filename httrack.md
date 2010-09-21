# Httrack #

## CLI Parameter ##

-w
:	mirror web sites (--mirror)
-O
:	backup directory
-%P
:	extended parsing, attempt to parse all links, even in unknown tags or Javascript (%P0 don&#8217;t use) (--extended-parsing[=N])
-N0
:	Saves files like in site Site-structure (default)
-s0
:	follow robots.txt and meta robots tags (0=never,1=sometimes,* 2=always) (--robots[=N])
-p7
:	Expert options, priority mode: 7 &gt; get html files before, then treat other files
-S
:	Expert option, stay on the same directory
-a
:	Expert option, stay on the same address
-K0
:	keep original links (e.g. http://www.adr/link) (K0 *relative link, K absolute links, K3 absolute URI links) (--keep-links[=N]
-A25000
:	maximum transfer rate in bytes/seconds (1000=1kb/s max) (--max-rate[=N]), here 25kb/s
-F
:	user-agent field (-F “user-agent name”) (--user-agent )
-%s
:	update hacks: various hacks to limit re-transfers when updating (identical size, bogus response..) (--updatehack)
-x
:	Build option, replace external html links by error pages
-%x
:	Build option, do not include any password for external password protected websites (%x0 include) (--no-passwords)

