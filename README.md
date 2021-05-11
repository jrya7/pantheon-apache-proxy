# pantheon-apache-proxy
Proxy setup to serve Pantheon Drupal pages from an Apache web server using the same domain name without having to use a subdomain


## overview
- proxy an apache request to a drupal site on pantheon
- shares the same domain with 2 different servers without a subdomain 
- takes an internal path and proxies to a pantheon site
- if a match is not found then will resolve to apache location
- the local path does not have to be valid to proxy requests

## requirements
make sure the following apache modules are enabled (list may be incomplete, send a PR if any should be added)
- LoadModule proxy_module modules/mod_proxy.so
- LoadModule proxy_http_module modules/mod_proxy_http.so


## usage
- modify the pantheon-proxy.conf file for the paths you want to proxy by editing the path in the "Location" directive start tag
  - this will be the URL path on the local server
  - you do not need to enter the web root path
- modify the proxy location inside of the "Location" directive block
  - this will be the remote URL location
  - you will need to enter the full FQDN and http protocol
- save your changes and restart apache
  - `systemctl restart httpd`
- visit the URL path in your browser to verify
  - the URL should stay as the orginal path but show the proxy page location


## examples
the following examples are for using www.cgd.ucar.edu as the local apache server

`<Location "/research/cdg">  
	ProxyPass https://climatedataguide.ucar.edu/  
	ProxyPassReverse https://climatedataguide.ucar.edu/  
</Location>`

this directive will proxy all requests to:
https://www.cgd.ucar.edu/research/cdg

over to:
https://climatedataguide.ucar.edu

meaning that the user will see https://www.cgd.ucar.edu/research/cdg in their browser but the page shown is actually https://climatedataguide.ucar.edu