mixin: 
  dns:
    enable: true
    ipv6: true
    listen: :53
    default-nameserver:
      - "223.5.5.5"
      - "[2400:3200::1]:53"
      - "120.53.53.185"
      - "[2402:4e00:0::36e3:43bf]:53"
    enhanced-mode: redir-host
    fake-ip-range: 198.18.0.1/16
    use-hosts: true
    nameserver:
      - https://123317.alidns.com/dns-query
      - tls://123317.alidns.com
      -	https://doh-36e343bf.doh.pub/dns-query
      -	tls://dot-36e343bf.dot.pub
    fallback:
      - https://1.1.1.1/dns-query
      - https://dns.google/dns-query
      - tls://1.1.1.1:853


dns:
    enable: true
    ipv6: true
    default-nameserver: [223.5.5.5, "[2400:3200::1]:53", 120.53.53.185, "[2402:4e00:0::36e3:43bf]:53"]
    enhanced-mode: redir-host
    fake-ip-range: 198.18.0.1/16
    use-hosts: true
    nameserver: ['https://123317.alidns.com/dns-query','tls://123317.alidns.com','https://doh-36e343bf.doh.pub/dns-query','tls://dot-36e343bf.dot.pub']
    fallback: ['tls://1.0.0.1:853', 'https://cloudflare-dns.com/dns-query', 'https://dns.google/dns-query']
    fallback-filter: { geoip: true, ipcidr: [240.0.0.0/4, 0.0.0.0/32] }
	
username：admin
password：JqMHjA3s

# tencent
DOH:https://doh-36e343bf.doh.pub/dns-query
DOT:dot-36e343bf.dot.pub
IPv4:120.53.53.185 120.53.53.165
IPv6:2402:4e00:0::36e3:43bf 2402:4e00:1::36e3:43bf

# aliyun
DOH:https://123317.alidns.com/dns-query
DOT:123317.alidns.com
IPv4:223.5.5.5 223.6.6.6
IPv6:2400:3200::1 2400:3200:baba::1

# AdGuard
DOH：https://family.adguard-dns.com/dns-query
DOT：family.adguard-dns.com
IPv4：94.140.14.15 94.140.15.16
IPv6：2a10:50c0::bad1:ff 2a10:50c0::bad2:ff


Gamil：VEQBXJ@gmail.com
Password：3qiEcmcJ

Gamil：vegakindred@gmail.com
Password：ty2LSHyr

Gamil：ophitcadsre@gmail.com
password：WzMXUgnU
Buff：vegakindred@gmail.com

Number：+1(802)7650715