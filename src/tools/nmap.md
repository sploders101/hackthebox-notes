# Nmap

Nmap is a network scanning utility. This page contains useful flags and args.


## Flags

| Flag     | Description                                                             |
|----------|-------------------------------------------------------------------------|
| -Pn      | Turn off ping checks                                                    |
| -sn      | Turn off port scanning                                                  |
| -sV      | Turn on version checking                                                |
| -A       | Enable OS detection, version detection, script scanning, and traceroute |
| --script | Use an nmap script while scanning                                       |

## Scripts

Nmap contains many scripts to help further identify services. A few of these
are listed below.

| Script           | Description                                                        |
|------------------|--------------------------------------------------------------------|
| banner           | Leverage "banner grabbing" for service identification              |
| smb-os-discovery | Leverage the SMB protocol to identify the OS running on the server |
