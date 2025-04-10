---

# **🌐CGI Scripts (Common Gateway Interface)**

## **📘Overview**
CGI (Common Gateway Interface) is a standard protocol that allows web servers to execute external programs or scripts to generate dynamic web content.

## **⚙️How CGI Works**
1. A user accesses a CGI-enabled URL.
2. The web server runs a script located (typically) in the `cgi-bin` directory.
3. The script processes input and returns a dynamically generated HTML response.

## **💻 Supported Scripting Languages**
CGI scripts can be written in:
- **Perl** – Historically common
- **Python** – Clean and easy to maintain
- **PHP** – Widely used in web development
- **Ruby** – Flexible scripting
- **Shell Scripts** – Great for server-side tasks and quick utilities

---

## **🔧Installation and Setup**

### **📦Install Required Packages**
```bash
yum install httpd
yum install perl-CGI perl
```

### **✅ Verify CGI Module in Apache**
```bash
httpd -M | grep cgi
```

### **Check SELinux Status**
```bash
sestatus
```

---

## **🧪Apache Configuration for CGI**

### **Edit the Configuration File**
```bash
vim /etc/httpd/conf/httpd.conf
```

### **📝Example Directory Configuration**
Make sure the following block is present and correctly configured:

```apache
<Directory "/var/www/cgi-bin">
    AllowOverride None
    Options ExecCGI
    AddHandler cgi-script .cgi .pl .py .sh
    Require all granted
</Directory>
```

## **Creating and Deploying CGI Scripts**

### **Example: Perl CGI Script for Memory Usage**
```perl
#!/usr/bin/perl
print "Content-type: text/html\n\n";
print "<h1>Server Memory Usage</h1>";
print "<pre>";
exec("free -h");
print "</pre>";
```

Save as:
```bash
vim /var/www/cgi-bin/test.cgi
```

Set permissions:
```bash
chown apache:apache /var/www/cgi-bin/test.cgi
chmod 755 /var/www/cgi-bin/test.cgi
```

Restart Apache:
```bash
systemctl restart httpd.service
```

Access the script via:
```
http://<server-ip>/cgi-bin/test.cgi
```

---

### **Additional CGI Script Examples**

#### `mem.cgi` – Bash Memory Usage Script
```bash
#!/bin/bash
echo "Content-type: text/html"
echo
echo "<h1>Server Memory Usage</h1>"
echo "<pre>"
free -h
echo "</pre>"
```
Save and set permissions:
```bash
vim /var/www/cgi-bin/mem.cgi
chown apache:apache /var/www/cgi-bin/mem.cgi
chmod 755 /var/www/cgi-bin/mem.cgi
```

#### `ping.cgi` – Shell Script for Ping Test
```bash
#!/bin/bash
echo "Content-type: text/html"
echo
echo "<h1>Ping Test</h1>"
echo "<pre>"
ping -c 4 google.com
echo "</pre>"
```

Save and set permissions:
```bash
vim /var/www/cgi-bin/ping.cgi
chown apache:apache /var/www/cgi-bin/ping.cgi
chmod 755 /var/www/cgi-bin/ping.cgi
```

Access via:
```
http://<server-ip>/cgi-bin/ping.cgi
```

---

## **Managing Other Scripts**

#### Example: `script.cgi`
```bash
vim /var/www/cgi-bin/script.cgi
chmod +x /var/www/cgi-bin/script.cgi
ls -lh /var/www/cgi-bin/script.cgi
```

#### Example: `script2.cgi`
```bash
vim /var/www/cgi-bin/script2.cgi
chmod +x /var/www/cgi-bin/script2.cgi
ls -lh /var/www/cgi-bin/script2.cgi
```

## ✅ **Apache Configuration for CGI Support**

### 🔧 **Steps:**

1. **Open Apache Configuration File**  
   Edit the main Apache config:
   ```bash
   vim /etc/httpd/conf/httpd.conf
   ```

2. **Enable CGI Execution**
   Make sure this section is either present or added to the file:

   ```apache
   ScriptAlias /cgi-bin/ "/var/www/cgi-bin/"

   <Directory "/var/www/cgi-bin">
       AllowOverride None
       Options +ExecCGI
       AddHandler cgi-script .cgi .pl .py .sh
       Require all granted
   </Directory>
   ```

   - `ScriptAlias` maps `/cgi-bin/` in the URL to the actual directory.
   - `ExecCGI` allows scripts to be executed.
   - `AddHandler` tells Apache which file extensions should be treated as CGI.
   - `Require all granted` makes it accessible to all users.

3. **Ensure CGI Module is Enabled**
   Run:
   ```bash
   httpd -M | grep cgi
   ```
   You should see something like:
   ```
   cgi_module (shared)
   ```

   If it’s **not enabled**, add or uncomment this line in `httpd.conf`:
   ```apache
   LoadModule cgi_module modules/mod_cgi.so
   ```

4. **Set Permissions**
   Make sure your scripts:
   - Are **executable**: `chmod 755 script.cgi`
   - Have the correct **ownership**: `chown apache:apache script.cgi`

5. **Restart Apache**
   ```bash
   systemctl restart httpd.service
   ```

6. **Test Access**
   In your browser:
   ```
   http://<your-server-ip>/cgi-bin/script.cgi
   ```

---
