---
layout: page
title: NFQL
tagline: Supporting tagline
---
{% include JB/setup %}

Cisco’s NetFlow protocol and IETF’s IPFIX open standard have contributed
heavily in pushing IP flow export as the de-facto technique for
collecting aggregate network traffic statistics. These flow records are
used for billing and mediation, bandwidth provisioning, detecting
malicious attacks and network performance evaluation. However,
understanding certain traffic patterns requires sophisticated flow
analysis tools that can mine flow records for such a usage. We recently
proposed a flow query language that can cap such flow-records. In this
work, we introduce NFQL, an efficient implementation of the query
language. NFQL can process flow records, aggregate them into groups,
apply absolute or relative filters, invoke Allen interval algebra rules,
and merge group records. NFQL has been evaluated by a suite of
benchmarks against contemporary flow-processing tools.

[nfql.vaibhavbajpai.com &rarr;](http://nfql.vaibhavbajpai.com)  
  
<br/>
  
## Blog Posts
<ul class="posts">
  {% for post in site.posts %}
    <li><span>{{ post.date | date_to_string }}</span> &raquo; <a href="{{ BASE_PATH }}{{ post.url }}">{{ post.title }}</a></li>
  {% endfor %}
</ul>
