---
id: 208
title: 'PHP提示 Fatal error: Class &#039;DOMDocument&#039; not found in'
date: 2018-10-10T14:02:33+08:00
author: 徐艺洲
layout: revision
guid: http://www.fireidea.com/archives/208
permalink: /archives/208
---
<div id="sina_keyword_ad_area2" class="articalContent   ">
  <table>
    <tr>
      <td WIDTH="100%">
        <h1>
          How to fix PHP Fatal error: Class&#8217;DOMDocument&#8217; not found in<br />
        </h1>
      </td>
      
      <td ALIGN="right" WIDTH="100%">
        <a HREF="http://www.techportal.co.za/linux/202-how-to-fix-php-fatal-error-class-domdocument-not-found-in-varwwwhtmlindexphp-on-line-171-or-similar-error?format=pdf" TITLE="PDF" REL="nofollow"><img src="http://simg.sinajs.cn/blog7style/images/common/sg_trans.gif" real_src ="http://www.techportal.co.za/images/M_images/pdf_button.png" ALT="PDF" TITLE="PHP提示 Fatal error: Class 'DOMDocument' not found in" /></a>
      </td>
      
      <td ALIGN="right" WIDTH="100%">
        <a HREF="http://www.techportal.co.za/linux/202-how-to-fix-php-fatal-error-class-domdocument-not-found-in-varwwwhtmlindexphp-on-line-171-or-similar-error?tmpl=component&print=1&page=" TITLE="Print" REL="nofollow"><img src="http://simg.sinajs.cn/blog7style/images/common/sg_trans.gif" real_src ="http://www.techportal.co.za/images/M_images/printButton.png" ALT="Print" TITLE="PHP提示 Fatal error: Class 'DOMDocument' not found in" /></a>
      </td>
      
      <td ALIGN="right" WIDTH="100%">
        <a HREF="http://www.techportal.co.za/component/mailto/?tmpl=component&link=aHR0cDovL3d3dy50ZWNocG9ydGFsLmNvLnphL2xpbnV4LzIwMi1ob3ctdG8tZml4LXBocC1mYXRhbC1lcnJvci1jbGFzcy1kb21kb2N1bWVudC1ub3QtZm91bmQtaW4tdmFyd3d3aHRtbGluZGV4cGhwLW9uLWxpbmUtMTcxLW9yLXNpbWlsYXItZXJyb3I=" TITLE="E-mail"><img src="http://simg.sinajs.cn/blog7style/images/common/sg_trans.gif" real_src ="http://www.techportal.co.za/images/M_images/emailButton.png" ALT="E-mail" TITLE="PHP提示 Fatal error: Class 'DOMDocument' not found in" /></a>
      </td>
    </tr>
  </table>
  
  <p>
    <u><strong>Question</strong></u>: How to fix <strong>PHP Fatalerror: Class &#8216;DOMDocument&#8217; not found in/var/www/html/index.php on line 171</strong> or similar error?
  </p>
  
  <p>
    <strong>NOTE</strong>: Received this error on a <strong>Centos5</strong> environment while trying to run a <strong>PHP</strong>script that makes use of the <strong>DOMDocument</strong> class
  </p></p> 
  
  <p>
    <u><strong>Answer</strong></u>:
  </p>
  
  <p>
    In the <strong>CLI</strong> <font COLOR="#999999"><em>(CommandLine Interface)</em></font> run the following command
  </p></p> 
  
  <div STYLE="overflow: hidden; display: block; height: auto; width: inherit;">
    <div>
      <div>
        <div>
          <a STYLE="width: 16px; height: 16px;" TITLE="view source" HREF="http://www.techportal.co.za/linux/202-how-to-fix-php-fatal-error-class-domdocument-not-found-in-varwwwhtmlindexphp-on-line-171-or-similar-error#viewSource">view source</a><a STYLE="width: 16px; height: 16px;" TITLE="print" HREF="http://www.techportal.co.za/linux/202-how-to-fix-php-fatal-error-class-domdocument-not-found-in-varwwwhtmlindexphp-on-line-171-or-similar-error#printSource">print</a><a STYLE="width: 16px; height: 16px;" TITLE="?" HREF="http://www.techportal.co.za/linux/202-how-to-fix-php-fatal-error-class-domdocument-not-found-in-varwwwhtmlindexphp-on-line-171-or-similar-error#about">?</a>
        </div>
      </div>
      
      <div>
        <div>
          <code>1.</code><span><span STYLE="margin-left: 0px ! important;"><code>yum</code><code>install</code> <code>php-xml</code></span></span>
        </div>
      </div>
    </div>
  </div>
  
  <p>
    This will install XML support and will solve the<strong>DOMDocument</strong> issue
  </p>