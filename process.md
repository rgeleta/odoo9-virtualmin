<ol ><li><p>Set up server (Ignore if Virtualmin already installed)</p>

 <ol ><li><p>Install OS</p>

 </li>
 <li><p>Run OS Updates</p>

 </li>
 <li><p>Install Virtualmin</p>

 </li>
 <li><p>Note the version of PostgreSQL installed</p>

 </li>
 </ol></li>
 <li><p>Create Odoo account "odoo9a.example.com"</p>

 <ol ><li><p>Virtualmin &gt; Create Virtual Server</p>

 <ol ><li><p>Enabled Features</p>

 <ol ><li><p>Setup DNS Zone</p>

 </li>
 <li><p>Create PostgreSQL database</p>

 </li>
 </ol></li>
 </ol></li>
 <li><p>Notes on the account name</p>

 <ol ><li><p>odoo9a is for "odoo", version "9", installation "a"<br />
 odoo9 will enable differentiation between multiple versions of odoo installed</p>

 </li>
 <li><p>"a" suffix for your first attempt, <br />
 then "b" for your second if something does not go right</p>

 </li>
 <li><p>“example.com" means use "example.com"<br />
 not yourdomain.com<br />
 the reason will become clearer below</p>

 </li>
 </ol></li>
 </ol></li>
 <li><p>Configure account directories for Odoo<br />
```
cd /home/odoo9a
mkdir bin.odoo
mkdir opt<br />
mkdir etc<br />
mkdir etc/init.d<br />
```

 </li>
 <li><p>Get an Odoo installation scripts<br />
git clone https://github.com/aschenkels-ictstudio/odoo-install-scripts <br />
 is a good place to start</p>

 </li>
 <li><p>Modify installation scripts<br />
 Andre's script is good for installing Odoo on a bare OS server<br />
 it needs modification for installing with Virtualmin</p>

 <ol ><li><p>Change the value of the following variables</p>

 <ol ><li><p>OE_USER=odoo9a</p>

 </li>
 <li><p>OE_HOME="/home/$OE_USER/opt/$OE_USER"</p>

 </li>
 <li><p>OE_HOME_EXT-"/home/$OE_USER/opt/$OE_USER/$OE_USER-server"</p>

 </li>
 </ol></li>
 <li><p>Add the following variables</p>

 <ol ><li><p>OE_DIR_ETC="/home/$OE_USER/etc"</p>

 </li>
 </ol></li>
 <li><p>Comment out the following sections of the script</p>

 <ol ><li><p>PostgreSQL x.y Settings</p>

 </li>
 <li><p>Creating the ODOO PostgreSQL User</p>

 </li>
 <li><p>Create ODOO system user</p>

 </li>
 </ol></li>
 <li><p>In the section "Configure ODOO" change all references <br />
 from " /etc" <br />
 to " $OE_DIR_ETC"</p>

 </li>
 </ol></li>
 <li><p>(optional) Create a wrapper script<br />
```
 #/bin/bash <br />
 #<br />
 # set bash option to show commands being run <br />
 set +x <br />
 # <br />
 # run install script <br />
sudo bash ./odoo9-install.sh 2&gt;&amp;1 | tee `pwd`/odoo9-install.log <br />
 #<br />
 # done <br />
```</p>

 </li>
 <li><p>Run Odoo installation script (either optional wrapper script or main script)</p>

 </li>
 <li><p>Create links in /etc and /etc/init.d to point to scripts in /home/odoo9/etc<br />
 as root<br />
 &lt;code&gt;<br />
 cd /etc<br />
ln -s /home/odoo9a/etc/init.d/odoo9a-server /etc/init.d/odoo9a-server<br />
ln -s /home/odoo9a/etc/odoo9a-server.conf /etc/odoo9a-server.conf<br />
 &lt;/code&gt;</p>

 </li>
 <li><p>Start Odoo<br />
 as root<br />
 service odoo9a-server start</p>

 </li>
 <li><p>Using Virtualmin, open port 8069</p>

 <ol ><li><p>Webmin &gt; Networking &gt; Linux Firewall</p>

 </li>
 <li><p>Add Rule</p>

 </li>
 <li><p>Rule Comment: Odoo Port</p>

 </li>
 <li><p>Action to Take: Accept</p>

 </li>
 <li><p>Destination TCP or UDP Port: 8069</p>

 </li>
 <li><p>Create</p>

 </li>
 </ol></li>
 <li><p>Create a website in your domain to run Odoo</p>

 <ol ><li><p>Virtualmin &gt; Create Virtual Server</p>

 <ol ><li><p>Domain Name: odoo9a.yourdomain.com</p>

 </li>
 <li><p>Uncheck everything </p>

 </li>
 <li><p>Check "Setup website for domain"</p>

 </li>
 <li><p>Check "Setup SSL website too"</p>

 </li>
 <li><p>Create Server</p>

 </li>
 </ol></li>
 <li><p>Virtualmin &gt; Server Configuration &gt; Edit Proxy Website</p>

 <ol ><li><p>Proxying enabled? Yes</p>

 </li>
 <li><p>Proxy to URL: http://odoo9a.example.com:8069<br />
 NOTE: here's where the "example.com" is actually used.</p>

 </li>
 </ol></li>
 </ol></li>
 <li><p>Access website from an external machine's browser<br />
 http://odoo9a.yourdomain.com:8069</p>

 </li>
 </ol>

<p>## end ##</p>

