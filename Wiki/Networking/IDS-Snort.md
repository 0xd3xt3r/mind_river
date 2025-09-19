# [[Snort]] - IDS/IPS

[Project URL](https://github.com/snort3/snort3/blob/master/doc/user/concepts.txt)

## Concepts

(pkt) -> \[**decode**\] -> \[**preprocess**\] -> \[**detect**\] -> \[**log**\] -> (verdict)

The steps are:

1.  **Decode** each packet to determine the basic network characteristics such as source and destination addresses and ports. A typical packet might have ethernet containing IP containing TCP containing HTTP (ie eth:ip:tcp:http). The various encapsulating protocols are examined for sanity and anomalies as the packet is decoded. This is essentially a stateless effort.
2.  **Preprocess** each decoded packet using accumulated state to determine the purpose and content of the innermost message. This step may involve reordering and reassembling IP fragments and TCP segments to produce the original application protocol data unit (PDU). Such PDUs are analyzed and normalized as needed to support further processing.
3.  **[[Detection]]** is a two step process. For efficiency, most rules contain a specific content pattern that can be searched for such that if no match is found no further processing is necessary. Upon start up, the rules are compiled into pattern groups such that a single, parallel search can be done for all patterns in the group. If any match is found, the full rule is examined according to the specifics of the signature.
4.  The **logging** step is where Snort saves any pertinent information resulting from the earlier steps. More generally, this is where other actions can be taken as well such as blocking the packet.


## [[Writing Rules]]

Good Tutorial on Writing Snort 2 rules [link](https://paginas.fe.up.pt/~mgi98020/pgr/writing_snort_rules.htm#rule_header)

Rules tell Snort how to detect interesting conditions, such as an attack, and what to do when the condition is detected. Here is an example rule:

```
alert tcp any any -> 192.168.1.1 80 ( msg:"A ha!"; content:"attack"; sid:1; )
```

The structure is:

```
action proto source dir dest ( body )
```

Where:

1. **action** - tells Snort what to do when a rule "fires", ie when the signature matches. In this case Snort will log the event. It can also do thing like block the flow when running inline.
2. **[[protocol]]** - tells Snort what protocol applies. This may be ip, icmp, tcp, udp, http, etc.
3. **source** - specifies the sending IP address and port, either of which can be the keyword any, which is a wildcard.
4. **direction** - must be either unidirectional as above or bidirectional indicated by <>.
5. **destination** - similar to source but indicates the receiving end.
6. **body** - detection and other information contained in parenthesis.

## References

1. https://github.com/snort3/snort3/blob/master/doc/user/concepts.txt
2. https://snort-org-site.s3.amazonaws.com/production/release_files/files/000/016/343/original/snort_user.html
3. https://francoisbotha.io/2017/12/10/getting-started-with-snort-for-intrusion-detection/