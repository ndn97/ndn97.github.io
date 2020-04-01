---
layout: post
title: Performance Optimization of http  endpoints
tags: Peformance nginx http
---
http endpoints running on a small instances which needs to be optimized for peformance. can be done using 2 ways

1. **"Throwing resource to the problem"** - easier approach,increase size of the backend instance, this increases cost.
2. **"Optimize Protocols"** - to improve/reduce the traffic reaching small instance, similar or marginal cost increase.

Some opportunities to optimize protocols in nginx:

* #### http/2 instead of http/1.1 
    http/2 provides header compression,multiplexing and server push features, since http request is foundational, any small improvement
    will have huge impact on performance and costs. 
    
    enabling http/2 protocol in nginx can be done as
    
       listen 443 ssl http2;

* #### TLS1.3 and  TLS1.2
    Transport Layer Security(TLS) is successor of Secure Sockets Layer(SSL), TLS 1.2 requires 2 round trips for handshake, TLS 1.3
    completes handshake with 1 round trip, this reduces encryption latency by 50% and thus improves performance.   
    TLS 1.3 also offers 0-RTT (zero Round trip time) Resumption, which helps clients which are previously connected to server with
    zero-handshake and thus improves performance, this feature is vulnerable to  Replay attacks. Not all clients supports TLS1.3, hence
    better to enable both TLS 1.3 and TLS 1.2
    
    enabling TLS V1.3 and 0-RTT in nginx can be done as
    
       ssl_protocols TLSv1.3 TLSv1.2;
       ssl_early_data on;
    
* #### http/3  and http/2
    http/3 is evovled version of http/2 and upcoming standard for http, http/3 is based on QUIC (Quick UDP Internet Connections) which offers much improved handshake 
    than TCP+TLS 1.3 combined and other performance improvements.
    
    support to enable this protocol in nginx is limited , and expected  to be added soon
    

    




