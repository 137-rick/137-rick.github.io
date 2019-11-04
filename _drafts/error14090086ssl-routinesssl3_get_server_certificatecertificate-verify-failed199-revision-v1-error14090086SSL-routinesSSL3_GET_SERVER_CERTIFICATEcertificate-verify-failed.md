---
id: 211
title: error:14090086:SSL routines:SSL3_GET_SERVER_CERTIFICATE:certificate verify failed
date: 2018-10-10T14:03:52+08:00
author: 徐艺洲
layout: revision
guid: http://www.fireidea.com/archives/211
permalink: /archives/211
---
<div id="sina_keyword_ad_area2" class="articalContent   newfont_family">
  <div>
    curl一个http报错如下：
  </div>
  
  <div>
  </div>
  
  <div>
    <div>
      curl: (60) SSL certificate problem, verify that the CA cert isOK. Details:
    </div>
    
    <div>
      error:14090086:SSLroutines:SSL3_GET_SERVER_CERTIFICATE:certificate verifyfailed
    </div>
    
    <div>
      More details here:http://curl.haxx.se/docs/sslcerts.html
    </div>
    
    <div>
    </div>
    
    <div>
      curl performs SSL certificate verification by default, using a&#8221;bundle&#8221;
    </div>
    
    <div>
      of Certificate Authority (CA) public keys(CA certs). The default
    </div>
    
    <div>
      bundle is named curl-ca-bundle.crt; you canspecify an alternate file
    </div>
    
    <div>
      using the &#8211;cacert option.
    </div>
    
    <div>
      If this HTTPS server uses a certificate signed by a CArepresented in
    </div>
    
    <div>
      the bundle, the certificate verificationprobably failed due to a
    </div>
    
    <div>
      problem with the certificate (it might beexpired, or the name might
    </div>
    
    <div>
      not match the domain name in the URL).
    </div>
    
    <div>
      If you&#8217;d like to turn off curl&#8217;s verification of thecertificate, use
    </div>
    
    <div>
      the -k (or &#8211;insecure) option.
    </div>
  </div>
  
  <div>
  </div>
  
  <div>
    上网络找了半天原来是本地CA证书太老了
  </div>
  
  <div>
    https://raymii.org/s/snippets/CentOS_5_CA_Certificate_Bundle_Update.html
  </div>
  
  <div>
  </div>
  
  <div>
    然后下载本地做了下测试：curl https://xxxx &#8211;cacert/etc/pki/tls/certs/ca-bundle.crt.test
  </div>
  
  <div>
    报错：
  </div>
  
  <div>
    curl: (35) error:0D0C50A1:asn1 encodingroutines:ASN1_item_verify:unknown message digest algorithm
  </div>
  
  <div>
  </div>
  
  <div>
    一查原来是本地openssl版本太低不认识新版的CA好吧。。
  </div>
  
  <div>
  </div>
  
  <div>
    最后还是代码里面加上了CURLOPT_SSL_VERIFYPEER =false
  </div>
  
  <div>
    安静了
  </div>
  
  <div>
    服务器太老了（centos 5.4），还在线服务不能随便动。。
  </div>
  
  <div>
  </div>