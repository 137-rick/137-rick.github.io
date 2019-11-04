---
id: 210
title: lucene Field引起的误会
date: 2018-10-10T14:03:37+08:00
author: 徐艺洲
layout: revision
guid: http://www.fireidea.com/archives/210
permalink: /archives/210
---
<div id="sina_keyword_ad_area2" class="articalContent   newfont_family">
  今天，犯了个低级错误…做查询的时候发现lucene用queryparser只有完全等值才能匹配。</p> 
  
  <div>
  </div>
  
  <div>
    经过排查原来是在建立索引的时候使用了StringField作为字段（这个只支持全值相等）
  </div>
  
  <div>
  </div>
  
  <div>
    改为TextField方可模糊查询……
  </div>
  
  <div>
  </div>
  
  <div>
    baidu好多资料是没有发现的……
  </div>
  
  <div>
  </div>
  
  <div>
    后来看到官方某个角落的文档发现了这个，才想起来怎么回事……感冒中……
  </div>
  
  <div>
  </div>
  
  <div>
    <div>
      <ul STYLE="margin: 10px 0px; padding: 0px;">
        <li STYLE="list-style: none; margin-bottom: 15px; line-height: 1.4;">
          <pre STYLE="font-family:" DEJAVU="" SANS="" MARGIN-TOP:="">public class <span STYLE="font-weight: bold;">Field</span>extends <a HREF="https://docs.oracle.com/javase/8/docs/api/java/lang/Object.html?is-external=true" TITLE="class or interface in java.lang">Object</a>implements <a HREF="http://lucene.apache.org/core/6_5_0/core/org/apache/lucene/index/IndexableField.html" TITLE="interface in org.apache.lucene.index">IndexableField</a></pre>
          
          <div STYLE="margin: 3px 10px 2px 0px; color: rgb(71, 71, 71); font-family:" DEJAVU="">
            Expert: directly create a field for a document.Most users should use one of the sugar subclasses:</p> 
            
            <ul STYLE="list-style-type: disc;">
              <li>
                <a HREF="http://lucene.apache.org/core/6_5_0/core/org/apache/lucene/document/TextField.html" TITLE="class in org.apache.lucene.document"><code STYLE="font-family:" DEJAVU="" SANS="" PADDING-TOP:="" MARGIN-TOP:="" LINE-HEIGHT:="">TextField</code></a>: <a HREF="https://docs.oracle.com/javase/8/docs/api/java/io/Reader.html?is-external=true" TITLE="class or interface in java.io"><code STYLE="font-family:" DEJAVU="" SANS="" PADDING-TOP:="" MARGIN-TOP:="" LINE-HEIGHT:="">Reader</code></a> or <a HREF="https://docs.oracle.com/javase/8/docs/api/java/lang/String.html?is-external=true" TITLE="class or interface in java.lang"><code STYLE="font-family:" DEJAVU="" SANS="" PADDING-TOP:="" MARGIN-TOP:="" LINE-HEIGHT:="">String</code></a> indexed for full-textsearch
              </li>
              <li>
                <a HREF="http://lucene.apache.org/core/6_5_0/core/org/apache/lucene/document/StringField.html" TITLE="class in org.apache.lucene.document"><code STYLE="font-family:" DEJAVU="" SANS="" PADDING-TOP:="" MARGIN-TOP:="" LINE-HEIGHT:="">StringField</code></a>: <a HREF="https://docs.oracle.com/javase/8/docs/api/java/lang/String.html?is-external=true" TITLE="class or interface in java.lang"><code STYLE="font-family:" DEJAVU="" SANS="" PADDING-TOP:="" MARGIN-TOP:="" LINE-HEIGHT:="">String</code></a> indexed verbatim as a singletoken
              </li>
              <li>
                <a HREF="http://lucene.apache.org/core/6_5_0/core/org/apache/lucene/document/IntPoint.html" TITLE="class in org.apache.lucene.document"><code STYLE="font-family:" DEJAVU="" SANS="" PADDING-TOP:="" MARGIN-TOP:="" LINE-HEIGHT:="">IntPoint</code></a>: <code STYLE="font-family:" DEJAVU="" SANS="" PADDING-TOP:="" MARGIN-TOP:="" LINE-HEIGHT:="">int</code> indexed for exact/rangequeries.
              </li>
              <li>
                <a HREF="http://lucene.apache.org/core/6_5_0/core/org/apache/lucene/document/LongPoint.html" TITLE="class in org.apache.lucene.document"><code STYLE="font-family:" DEJAVU="" SANS="" PADDING-TOP:="" MARGIN-TOP:="" LINE-HEIGHT:="">LongPoint</code></a>: <code STYLE="font-family:" DEJAVU="" SANS="" PADDING-TOP:="" MARGIN-TOP:="" LINE-HEIGHT:="">long</code> indexed forexact/range queries.
              </li>
              <li>
                <a HREF="http://lucene.apache.org/core/6_5_0/core/org/apache/lucene/document/FloatPoint.html" TITLE="class in org.apache.lucene.document"><code STYLE="font-family:" DEJAVU="" SANS="" PADDING-TOP:="" MARGIN-TOP:="" LINE-HEIGHT:="">FloatPoint</code></a>: <code STYLE="font-family:" DEJAVU="" SANS="" PADDING-TOP:="" MARGIN-TOP:="" LINE-HEIGHT:="">float</code> indexed forexact/range queries.
              </li>
              <li>
                <a HREF="http://lucene.apache.org/core/6_5_0/core/org/apache/lucene/document/DoublePoint.html" TITLE="class in org.apache.lucene.document"><code STYLE="font-family:" DEJAVU="" SANS="" PADDING-TOP:="" MARGIN-TOP:="" LINE-HEIGHT:="">DoublePoint</code></a>: <code STYLE="font-family:" DEJAVU="" SANS="" PADDING-TOP:="" MARGIN-TOP:="" LINE-HEIGHT:="">double</code> indexed forexact/range queries.
              </li>
              <li>
                <a HREF="http://lucene.apache.org/core/6_5_0/core/org/apache/lucene/document/SortedDocValuesField.html" TITLE="class in org.apache.lucene.document"><code STYLE="font-family:" DEJAVU="" SANS="" PADDING-TOP:="" MARGIN-TOP:="" LINE-HEIGHT:="">SortedDocValuesField</code></a>: <code STYLE="font-family:" DEJAVU="" SANS="" PADDING-TOP:="" MARGIN-TOP:="" LINE-HEIGHT:="">byte[]</code> indexed column-wisefor sorting/faceting
              </li>
              <li>
                <a HREF="http://lucene.apache.org/core/6_5_0/core/org/apache/lucene/document/SortedSetDocValuesField.html" TITLE="class in org.apache.lucene.document"><code STYLE="font-family:" DEJAVU="" SANS="" PADDING-TOP:="" MARGIN-TOP:="" LINE-HEIGHT:="">SortedSetDocValuesField</code></a>: <code STYLE="font-family:" DEJAVU="" SANS="" PADDING-TOP:="" MARGIN-TOP:="" LINE-HEIGHT:="">SortedSet</code> indexed column-wise forsorting/faceting
              </li>
              <li>
                <a HREF="http://lucene.apache.org/core/6_5_0/core/org/apache/lucene/document/NumericDocValuesField.html" TITLE="class in org.apache.lucene.document"><code STYLE="font-family:" DEJAVU="" SANS="" PADDING-TOP:="" MARGIN-TOP:="" LINE-HEIGHT:="">NumericDocValuesField</code></a>: <code STYLE="font-family:" DEJAVU="" SANS="" PADDING-TOP:="" MARGIN-TOP:="" LINE-HEIGHT:="">long</code> indexed column-wisefor sorting/faceting
              </li>
              <li>
                <a HREF="http://lucene.apache.org/core/6_5_0/core/org/apache/lucene/document/SortedNumericDocValuesField.html" TITLE="class in org.apache.lucene.document"><code STYLE="font-family:" DEJAVU="" SANS="" PADDING-TOP:="" MARGIN-TOP:="" LINE-HEIGHT:="">SortedNumericDocValuesField</code></a>: <code STYLE="font-family:" DEJAVU="" SANS="" PADDING-TOP:="" MARGIN-TOP:="" LINE-HEIGHT:="">SortedSet</code> indexed column-wise forsorting/faceting
              </li>
              <li>
                <a HREF="http://lucene.apache.org/core/6_5_0/core/org/apache/lucene/document/StoredField.html" TITLE="class in org.apache.lucene.document"><code STYLE="font-family:" DEJAVU="" SANS="" PADDING-TOP:="" MARGIN-TOP:="" LINE-HEIGHT:="">StoredField</code></a>: Stored-only value for retrieving insummary results
              </li>
            </ul>
            
            <p>
              A field is a section of a Document. Each field has three parts:name, type and value. Values may be text (String, Reader orpre-analyzed TokenStream), binary (byte[]), or numeric (a Number).Fields are optionally stored in the index, so that they may bereturned with hits on the document.
            </p>
            
            <p>
              NOTE: the field type is an <a HREF="http://lucene.apache.org/core/6_5_0/core/org/apache/lucene/index/IndexableFieldType.html" TITLE="interface in org.apache.lucene.index"><code STYLE="font-family:" DEJAVU="" SANS="" PADDING-TOP:="" MARGIN-TOP:="" LINE-HEIGHT:="">IndexableFieldType</code></a>. Making changes to the state ofthe IndexableFieldType will impact any Field it is used in. It isstrongly recommended that no changes be made after Fieldinstantiation.
            </p>
          </div>
        </li>
      </ul>
    </div>
    
    <div>
      <ul STYLE="margin: 10px 0px; padding: 0px;">
      </ul>
      
      <div>
      </div>
    </div>
  </div>