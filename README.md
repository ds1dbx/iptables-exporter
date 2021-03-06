# Iptables exporter

A Prometheus exporter that collects traffic data from iptables rules.


## Installation

    pip install iptables-exporter


## Usage

Test run:

    iptables-exporter --dump-data

Run iptables-exporter:

    iptables-exporter --port 9119

Point your browser to http://localhost:9119/metrics


## Docker

    docker run --net=host --cap-add=NET_ADMIN madron/iptables-exporter


## Configure iptables

Optionally you can monitor specific rules by adding a comment starting with `iptables-exporter`:

    iptables -A INPUT --dport ssh -j ACCEPT -m comment --comment "iptables-exporter ssh traffic"

collects packets and bytes counter:

    iptables_packets{ip_version="4",table="filter",chain="input",rule="ssh traffic"} 347.0
    iptables_bytes{ip_version="4",table="filter",chain="input",rule="ssh traffic"} 44512.0

More rules with same name:

    iptables -A INPUT -s 10.0.0.0/8     --dport ssh -j ACCEPT -m comment --comment "iptables-exporter ssh traffic"
    iptables -A INPUT -s 172.16.0.0/12  --dport ssh -j ACCEPT -m comment --comment "iptables-exporter ssh traffic"
    iptables -A INPUT -s 192.168.0.0/16 --dport ssh -j ACCEPT -m comment --comment "iptables-exporter ssh traffic"

exports total packets and bytes for the 3 rules as they have same ip_version, table, chain and name.
