# Enumeration cheat sheet

## Nmap

```bash
nmap -p- --min-rate 5000 -T5 -Pn -vvv -n <target> -oG allPorts
```

```bash
ports=$(cat allPorts | grep '^[0-9]' | cut -d '/' -f 1 | tr '\n' ',' | sed s/,$//)
```

```bash
nmap -sC -sV -T5 -p$ports <target> -oN targeted
```

```bash
nmap -sU --top-ports 100 -T4 <target>
```

## Fingerprinting

```bash
whatweb <target>
```

## Fuzzing — directories

```bash
ffuf -u http://<target>/FUZZ -w /usr/share/seclists/Discovery/Web-Content/raft-medium-directories.txt -recursion -recursion-depth 2 -ac
```

## Fuzzing — extensions

```bash
ffuf -u http://<target>/FUZZ -w common.txt -e .php,.txt,.html,.bak -ac
```

## Fuzzing — vhosts / subdomains

```bash
ffuf -u http://<target> -H "Host: FUZZ.domain.tld" -w /usr/share/wordlists/seclists/Discovery/DNS/subdomains-top1million-5000.txt -fs <default_size>
```

## Fuzzing — parameters

```bash
ffuf -u "http://<target>/api/endpoint?FUZZ=test" -w params.txt -ac
```

## DNS

```bash
dig any domain.tld @<target>
dig axfr domain.tld @<target>
```

## SMB

```bash
smbmap -H <target>
```

```bash
enum4linux -a <target>
```

```bash
smbclient //<target>/<share> -N
```

## LDAP

```bash
ldapsearch -x -H ldap://<target> -b "dc=domain,dc=tld"
```

## FTP

```bash
ftp <target>
```

## SSH

```bash
nc -vn <target> 22
```

## SMTP

```bash
smtp-user-enum -M VRFY -U users.txt -t <target>
```

## SNMP

```bash
snmpwalk -c public -v1 <target>
onesixtyone -c community.txt <target>
```

## NFS

```bash
showmount -e <target>
```

## Databases

```bash
mysql -h <target> -u root -p
```

## Notes

- Add the target domain to `/etc/hosts` as soon as it's known
- Check `robots.txt`, page source, and HTML comments manually before fuzzing
- Keep found credentials (user, pass, hash) in one place, they tend to get reused
- Try default/anonymous credentials on every open service before brute-forcing