# SNMP

There are several snmp clients, many of which are actually single-call binaries
(SCBs) to one another. These help with querying data from SNMP servers.

## Protocol details

### Authentication

SNMP v1 & v2c authenticates using a "community string", which is a plaintext
string used for authentication which is not encrypted or hashed.

### Structure

SNMP follows a structure based on "Object IDs" or "OIDs", which are a
tree-based ID structure (`header.subheader.subsubheader.property`). MIBs are
used to document these ID structures and map them to human-readable names and
descriptiptions. There is some standardization, but many MIBs are
vendor-specific and may conflict with those of other vendors.

## Common Flags

These flags are common for most or all SNMP commands

| Flag     | Description                 |
|----------|-----------------------------|
| -v       | The protocol version to use |
| -c <str> | Community string            |

## Snmpwalk

The `snmpwalk` command "walks the tree" of OIDs to discover properties on a
device. This can be useful, for example, when looking to discover what
interfaces are available on a device, and get their names. The interface ID
will typically be one of the last segments of the OID, and cannot be discovered
through other means.

Example usage:
```
sploders101@htb[/htb]$ snmpwalk -v 2c -c public 10.129.42.253 1.3.6.1.2.1.1.5.0

iso.3.6.1.2.1.1.5.0 = STRING: "gs-svcscan"
```

## Other tools

### Onesixtyone: Fast SNMP scanning & brute-forcing tool

Example usage for community string brute-forcing:
```
sploders101@htb[/htb]$ onesixtyone -c dict.txt 10.129.42.254

Scanning 1 hosts, 51 communities
10.129.42.254 [public] Linux gs-svcscan 5.4.0-66-generic #74-Ubuntu SMP Wed Jan 27 22:54:38 UTC 2021 x86_64
```
