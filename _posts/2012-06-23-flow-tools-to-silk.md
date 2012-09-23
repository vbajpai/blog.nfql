---
layout: post
title: "flow tools to SiLK"
description: ""
category: 
tags: []
---
{% include JB/setup %}
Flow-tools to SiLK
------------------
- - - 

Install `nfdump` and `ft2nfdump`

	>> sudo apt-get install nfdump
	>> sudo apt-get install nfdump-flow-tools


Convert flow-tools traces to nfdump

	>> flow-cat $INPUT | ft2nfdump | nfdump -w $OUTPUT
	>> nfdump -r $OUTPUT

Replay the nfdump traces  
(The trace is replayed to `127.0.0.1` at port `9995`)

	>> nfreplay -r $TRACE
	

Configure a sensor to collect replayed data

	>> cat sensors.conf
	
	probe S1 netflow-v5
	  listen-on-port 9995
	  protocol udp
	  accept-from-host 127.0.0.1
	end probe
	
	sensor S1
	  netflow-v5-probes S1
	  internal-ipblocks 10.0.0.0/8
	  external-ipblocks remainder
	end sensor

Collect flow data and save in binary SiLK files

	>> rwflowpack \
       --site-config-file=/usr/local/share/silk/generic-silk.conf \
       --sensor-configuration=sensors.conf \ 
       --root-directory=/var/log/silk/ \
       --log-destination=both
	    
    rwflowpack[14830]: Forked child 14831.  Parent exiting
	rwflowpack[14831]: Using packing logic from …
 	rwflowpack[14831]: Creating stream cache                                        
	rwflowpack[14831]: Starting flow processor #1 for PDU Reader
	rwflowpack[14831]: Creating PDU Reader Source Pool
	rwflowpack[14831]: Creating PDU Reader for probe 'S1' on 0.0.0.0:9995
	rwflowpack[14831]: Starting flush timer                                    
	rwflowpack[14831]: Started manager thread for PDU Reader
	
Flatten the SiLK root directory

	>> find /var/log/silk -type f -exec cp {} /var/log/silkflat/ \;

Combine all SiLK files into a single archive

	>> ls | rwcat --xargs --output-path=/var/log/silk.gz
