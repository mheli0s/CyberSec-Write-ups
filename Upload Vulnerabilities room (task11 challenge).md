Steps:

- enumerate endpoints:
	  - `gobuster dir -t 200 -u http://jewel.uploadvulns.thm -w /usr/share/wordlists/dirbuster/directory-list-2.3-medium.txt`
	  - alt: `ffuf -u http://jewel.uploadvulns.thm/FUZZ -c -w /usr/share/wordlists/dirb/common.txt`
		/admin
		/modules
		/content
- discover backend tech-stack:
	  - `curl -I <url>` or see response `X-Powered-By` header in Burp
		- Express/Node.js

- get a NodeJS reverse shell (not a PHP one etc.):
	- paste/save as shell.jpg from https://swisskyrepo.github.io/InternalAllTheThings/cheatsheets/shell-reverse-cheatsheet/#nodejs
	- change port & ip to your own ovpn or attackbox one

- bypass client-side upload restrictions:
	- reload (use Ctrl-F5 to force fresh version) jewel, then in Burp, intercept the filter's (upload.js) response via do intercept->response.
	- *note:* need to remove the js file type entry in request interception rules via Burp proxy settings to see and intercept the JS file
	- delete the raw JS for the filter restrictions then forward it
  
- upload shell.jpg now that filter is gone

- enumerate files to find name shell was saved as on the server (since it's common for servers to randomise the naming of uploaded files):
  `gobuster dir -t 200 -u http://jewel.uploadvulns.thm/content -w <path-to-wordlist> -x jpg`
  
- start netcat listener on your local ovpn box/attackbox:
	  - `nc -lvnp 1234`

- execute the reverse shell indirectly (NodeJS doesn't recognise/run code by just loading it's filepath in browser like PHP can): 
	  - go to http://jewel.uploadvulns.thm/modules
	  - run the XXX.jpg via inputting `../content/xxx.jpg` (XXX is the name the server saved shell.jpg as)
	  - check the netcat listener - reverse shell is connected!
  
- get flag: 
	 - `cat /var/www/flag.txt`: THM{redacted}
	- pwned! :)