<h1>Configuring SSL for HTTP (HTTPS)</h1>

<h2>Description</h2>
This portion of the project is to configure SSL for the HTTP server in the environment. 
<br />

<h2>Change UFW from Apache to Apache Full</h2>

With enabling SSL, a new firewall rule needs to be added to allow for traffic on port 443, HTTPS. I denied the Apache profile and enabled the Apache Full profile (you’ll see why this was a mistake later). The Apache profile only allows port 80 while the Apache Full profile enables both port 80 and 443.

<h2>Made New HTML File for Testing</h2>

I quickly whipped up a new HTML file to get away from the default Apache2 webpage.

<h2>Forward Port 443 through CentOS</h2>

As I was going to be sending traffic over a new port, I needed to add a rule in CentOS to forward traffic on port 443 to the webserver. I did this using the same method I used to forward port 80 in the previous lab session.

<h2>Enable mod_ssl</h2>

Now it’s time to enable the SSL module for Apache. This is achieved by using the “a2enmod SSL” command, then restarting the Apache service. This allows Apache to use SSL v3 and TLS v1.x.

<h2>Generated SSL Key and Certificate (OpenSSL)</h2>

Now that SSL is enabled for Apache to use, a key and certificate file need to be generated. This server will be using a self-signed certificate for ease of setting up and because this is an entirely internal network that will not be exposed to the internet. This will cause computers connecting to it to throw up errors pertaining to the fact that the certificate is self-signed, but these errors are nothing to worry about.

<h2>Configured Apache2 to Use SSL</h2>

Now that a key and certificate have been generated, Apache needs to be configured to use them. Two “VirtualHost” modules were added to direct traffic on ports 80 and 443 to the proper home directories (/var/www/10.0.0.10/). This allows users who connect to the website to choose whether they use HTTP or HTTPS.

<h2>Problems</h2>

Now is the fun part, troubleshooting. There were three main issues that I ran into while working on this section of this project: (1) the default webpage configuration file not getting disabled, (2) the UFW blocking legitimate traffic, and (3) an HTTP-to-HTTPS redirect causing issues. 

For the first issue, despite disabling the default Apache webpage (or so I thought), the default still overrode the new index and home directory that I enabled and assigned to both HTTP and HTTPS traffic. As a quick fix, I copied the default file’s configuration starting with “000” to my desktop, then deleted it from its directory entirely. Quick and dirty, I know, but effective!

For the second issue, when I had switched from using the Apache profile to the Apache Full profile in UFW, I denied Apache instead of removing it. Despite having enabled Apache Full, I was denying access on port 80 but allowing access on port 443. This was fixed by just re-enabling the Apache profile since port 80 was to be allowed anyways. 

For the third issue, I had originally implemented a VirtualHost configuration to redirect port 80 traffic to port 443, thus forcing the use of HTTPS. The method in which I did this was to make a redirect statement that looked like “Redirect / https://10.0.0.10/”, however this only worked on the Ubuntu machine and no other host on the network. Ultimately, I removed this statement in favor of a working webpage. This is an issue I need to investigate further into, as I believe the method I opted for may be outdated for the environment I am working in. 

Ultimately, this was my first time ever enabling SSL for a webpage. I learned an incredible amount of new information that furthered my understanding of secure web browsing, webpage configuration, and certificates.

<h2>Screenshots:</h2>

<p align="center">
What the Firewall Should NOT Look Like #1: <br/>
<img src="https://i.imgur.com/vAxmKMz.png" height="60%" width="60%" alt="What the Firewall Should NOT Look Like #1"/>
<br />
<br />
What the Firewall Should NOT Look Like #2: <br/>
<img src="https://i.imgur.com/8wAWm90.png" height="60%" width="60%" alt="What the Firewall Should NOT Look Like #2"/>
<br />
<br />
Making a New Page and Directory: <br/>
<img src="https://i.imgur.com/4gfuxal.png" height="60%" width="60%" alt="Making a New Page and Directory"/>
<br />
<br />
Forwarding Port 443 in CentOS: <br/>
<img src="https://i.imgur.com/nzdWJVd.png" height="60%" width="60%" alt="Forwarding Port 443 in CentOS"/>
<br />
<br />
Enabling mod_ssl: <br/>
<img src="https://i.imgur.com/ifHzLdT.png" height="60%" width="60%" alt="Enabling mod_ssl"/>
<br />
<br />
Generating Certificate and Key #1: <br/>
<img src="https://i.imgur.com/qcB8xYD.png" height="60%" width="60%" alt="Generating Certificate and Key #1"/>
<br />
<br />
Generating Certificate and Key #2: <br/>
<img src="https://i.imgur.com/YL5j5jc.png" height="60%" width="60%" alt="Generating Certificate and Key #2"/>
<br />
<br />
Enabling Apache to Use SSL: <br/>
<img src="https://i.imgur.com/xQ1LERr.png" height="60%" width="60%" alt="Enabling Apache to Use SSL"/>
<br />
<br />
Warning for Self-Signed Certificate: <br/>
<img src="https://i.imgur.com/wUxMTO7.png" height="60%" width="60%" alt="Warning for Self-Signed Certificate"/>
<br />
<br />
HTTPS Webpage on Ubuntu: <br/>
<img src="https://i.imgur.com/O6b7Cw4.png" height="60%" width="60%" alt="HTTPS Webpage on Ubuntu"/>
<br />
<br />
HTTP and HTTPS Webpages on External Machines: <br/>
<img src="https://i.imgur.com/J7s7w92.jpeg" height="60%" width="60%" alt="HTTP and HTTPS Webpages on External Machines"/>
<br />

</p>
