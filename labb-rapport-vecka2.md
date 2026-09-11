# Labbrapport - Vecka 2

**Namn:** Erik Testsson
**Kurs:** IT-infrastruktur (ISCX26)
**Labbdatum:** 2026-09-08
**Inlämningsdatum:** 2026-09-15

> **Moment:** Inspektera och dokumentera operativsystem- och nätverksinställningar i labbmiljön.
>
> **Tid/form:** Egenstudier / Eget arbete / Labbövningar (egen tid)
>
> **Koppling till kursmål:** Mål 2, 3, 8 och 9 (Förberedelse inför Mål 12)

---

## Moment 1: Nätverkskonfiguration & Granskning (CLI)

Undersök din virtuella maskins nätverkskort och IP-konfiguration med hjälp av kommandoraden.

### Tabell: Nätverksparametrar

| Parameter | Kommando (Windows / Linux) | Identifierat Värde / Resultat |
|---|---|---|
| IP-adress (IPv4) | `ipconfig` / `ip a` | `192.168.56.101` |
| Nätmask (Subnet Mask) | `ipconfig` / `ip a` | `255.255.255.0` / `24` |
| Standard Gateway | `ipconfig` / `ip route` | `192.168.56.1` |
| DNS-servrar | `ipconfig /all` / `cat /etc/resolv.conf` | `8.8.8.8`, `8.8.4.4` |
| MAC-adress (Physical Address) | `getmac` / `ip link` | `08:00:27:a1:b2:c3` |

### Kommandoutskrift 1: Nätverkskonfiguration

**Linux (`ip a`):**

```text
1: lo: <LOOPBACK,UP,LOWER_UP> mtu 65536 qdisc noqueue state UNKNOWN group default qlen 1000
    link/loopback 00:00:00:00:00:00 brd 00:00:00:00:00:00
    inet 127.0.0.1/8 scope host lo
       valid_lft forever preferred_lft forever
    inet6 ::1/128 scope host
       valid_lft forever preferred_lft forever
2: enp0s3: <BROADCAST,MULTICAST,UP,LOWER_UP> mtu 1500 qdisc fq_codel state UP group default qlen 1000
    link/ether 08:00:27:a1:b2:c3 brd ff:ff:ff:ff:ff:ff
    inet 192.168.56.101/24 brd 192.168.56.255 scope global dynamic noprefixroute enp0s3
       valid_lft 86143sec preferred_lft 86143sec
    inet6 fe80::a00:27ff:fea1:b2c3/64 scope link noprefixroute
       valid_lft forever preferred_lft forever
```

**Windows (`ipconfig /all`):**

```text
   Ethernet adapter Ethernet:

      Connection-specific DNS Suffix  . :
      Description . . . . . . . . . . . : Intel(R) PRO/1000 MT Desktop Adapter
      Physical Address. . . . . . . . . : 08-00-27-A1-B2-C3
      DHCP Enabled. . . . . . . . . . . : Yes
      Autoconfiguration Enabled . . . . : Yes
      IPv4 Address. . . . . . . . . . . : 192.168.56.101(Preferred)
      Subnet Mask . . . . . . . . . . . : 255.255.255.0
      Lease Obtained. . . . . . . . . . : den 8 september 2026 09:12:34
      Lease Expires . . . . . . . . . . : den 15 september 2026 09:12:34
      Default Gateway . . . . . . . . . : 192.168.56.1
      DHCP Server . . . . . . . . . . . : 192.168.56.1
      DNS Servers . . . . . . . . . . . : 8.8.8.8
                                          8.8.4.4
      NetBIOS over Tcpip. . . . . . . . : Enabled
```

---

## Moment 2: Anslutningstester & Nätverksfelsökning

Verifiera nätverkskommunikationen i flera steg för att identifiera eventuella hinder på vägen.

### Tabell: Anslutningstester

| Teststeg | Kommando | Förväntat utfall | Faktiskt resultat |
|---|---|---|---|
| 1. Loopback-test | `ping 127.0.0.1` | Verifiera lokal TCP/IP-stack | Lyckad |
| 2. Lokal Gateway | `ping 192.168.56.1` | Verifiera kontakt med router/gateway | Lyckad |
| 3. Extern IP-adress | `ping 8.8.8.8` | Verifiera internetanslutning utan DNS | Lyckad |
| 4. DNS-uppslagning | `nslookup systementor.se` | Verifiera att namnuppslag fungerar | `93.158.194.26` |

### Kommandoutskrift 2: Routing och Tracing

**Linux (`traceroute 8.8.8.8`):**

```text
traceroute to 8.8.8.8 (8.8.8.8), 30 hops max, 60 byte packets
 1  192.168.56.1 (192.168.56.1)  0.642 ms  0.589 ms  0.563 ms
 2  10.0.2.1 (10.0.2.1)  1.234 ms  1.198 ms  1.176 ms
 3  172.16.0.1 (172.16.0.1)  3.456 ms  3.401 ms  3.378 ms
 4  85.24.128.1 (85.24.128.1)  8.234 ms  8.198 ms  8.167 ms
 5  72.14.236.204 (72.14.236.204)  12.567 ms  12.523 ms  12.489 ms
 6  8.8.8.8 (8.8.8.8)  14.892 ms  14.856 ms  14.823 ms
```

**Windows (`tracert 8.8.8.8`):**

```text
Kör spårning till 8.8.8.8 med max 30 hopp.

  1    <1 ms    <1 ms    <1 ms  192.168.56.1
  2     1 ms     1 ms     1 ms  10.0.2.1
  3     3 ms     3 ms     3 ms  172.16.0.1
  4     8 ms     8 ms     8 ms  85.24.128.1
  5    12 ms    12 ms    12 ms  72.14.236.204
  6    14 ms    14 ms    14 ms  8.8.8.8

Spårning klar.
```
