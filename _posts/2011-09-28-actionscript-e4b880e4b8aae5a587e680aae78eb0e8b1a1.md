---
id: 135
title: actionscript 一个奇怪现象
date: 2011-09-28T15:27:52+08:00
author: 徐艺洲
layout: post
guid: http://www.fireidea.com/archives/135
permalink: /archives/135
categories:
  - 经典技巧
tags:
  - it
---
<div id="sina_keyword_ad_area2" class="articalContent   ">
  最近用actionscript3.0碰到一个土鳖问题</p> 
  
  <table CELLPADDING="0" CELLSPACING="0">
    <tr>
      <td>
        包
      </td>
      
      <td>
        <a HREF="http://www.itleadphp.com/Tutorials/ActionScript3.0/flash/display/package-detail.html">flash.display</a>
      </td>
    </tr>
    
    <tr>
      <td>
        类
      </td>
      
      <td>
        public dynamic class MovieClip
      </td>
    </tr>
    
    <tr>
      <td>
        继承
      </td>
      
      <td>
        MovieClip <img ALT="Inheritance" TITLE="Inheritance" src="http://simg.sinajs.cn/blog7style/images/common/sg_trans.gif" real_src ="http://www.itleadphp.com/Tutorials/ActionScript3.0/images/inherit-arrow.gif" /> <a HREF="http://www.itleadphp.com/Tutorials/ActionScript3.0/flash/display/Sprite.html">Sprite</a> <img ALT="Inheritance" TITLE="Inheritance" src="http://simg.sinajs.cn/blog7style/images/common/sg_trans.gif" real_src ="http://www.itleadphp.com/Tutorials/ActionScript3.0/images/inherit-arrow.gif" /> <a HREF="http://www.itleadphp.com/Tutorials/ActionScript3.0/flash/display/DisplayObjectContainer.html">DisplayObjectContainer</a> <img ALT="Inheritance" TITLE="Inheritance" src="http://simg.sinajs.cn/blog7style/images/common/sg_trans.gif" real_src ="http://www.itleadphp.com/Tutorials/ActionScript3.0/images/inherit-arrow.gif" /> <a HREF="http://www.itleadphp.com/Tutorials/ActionScript3.0/flash/display/InteractiveObject.html">InteractiveObject</a> <img ALT="Inheritance" TITLE="Inheritance" src="http://simg.sinajs.cn/blog7style/images/common/sg_trans.gif" real_src ="http://www.itleadphp.com/Tutorials/ActionScript3.0/images/inherit-arrow.gif" /> <a HREF="http://www.itleadphp.com/Tutorials/ActionScript3.0/flash/display/DisplayObject.html">DisplayObject</a> <img ALT="Inheritance" TITLE="Inheritance" src="http://simg.sinajs.cn/blog7style/images/common/sg_trans.gif" real_src ="http://www.itleadphp.com/Tutorials/ActionScript3.0/images/inherit-arrow.gif" /> <a HREF="http://www.itleadphp.com/Tutorials/ActionScript3.0/flash/events/EventDispatcher.html">EventDispatcher</a> <img ALT="Inheritance" TITLE="Inheritance" src="http://simg.sinajs.cn/blog7style/images/common/sg_trans.gif" real_src ="http://www.itleadphp.com/Tutorials/ActionScript3.0/images/inherit-arrow.gif" /> <a HREF="http://www.itleadphp.com/Tutorials/ActionScript3.0/Object.html">Object</a>
      </td>
    </tr>
    
    <tr>
      <td>
        子类
      </td>
      
      <td>
        <a HREF="http://www.itleadphp.com/Tutorials/ActionScript3.0/fl/livepreview/LivePreviewParent.html">LivePreviewParent</a>
      </td>
    </tr>
  </table>
  
  <table BORDER="0" CELLPADDING="0" CELLSPACING="0">
    <tr>
      <td STYLE="white-space:nowrap" VALIGN="top">
        <b>语言版本: </b>
      </td>
      
      <td>
        ActionScript 3.0
      </td>
    </tr>
  </table>
  
  <table BORDER="0" CELLPADDING="0" CELLSPACING="0">
    <tr>
      <td STYLE="white-space:nowrap" VALIGN="top">
        <b>Player 版本: </b>
      </td>
      
      <td>
        Flash Player 9
      </td>
    </tr>
  </table>
  
  <p>
    MovieClip类从以下类继承而来：Sprite、DisplayObjectContainer、InteractiveObject、DisplayObject和 EventDispatcher。
  </p>
  
  <p>
    不同于 Sprite 对象，MovieClip 对象拥有一个时间轴。
  </p>
  
  <p>
    MovieClip 类的方法提供的功能与定位影片剪辑的动作所提供的功能相同。 还有一些其它方法在 Flash创作工具的“动作”面板中的“动作”工具箱中没有等效动作。
  </p>
  
  <p STYLE="color: rgb(227, 0, 0);">
    如果修改包含补间动画的 MovieClip对象的下列任一属性，Flash Player 便会停止该 MovieClip对象中的播放头：<code>alpha</code>、<code>blendMode</code>、<code>filters</code>、<code>height</code>、<code>opaqueBackground</code>、<code>rotation</code>、<code>scaleX</code>、<code>scaleY</code>、<code>scale9Grid</code>、<code>scrollRect</code>、<code>transform</code>、<code>visible</code>、<code>width</code>、<code>x</code>或 <code>y</code>。 但是，它不会停止在该 MovieClip 对象的任何子 MovieClip对象中的播放头。
  </p>
  
  <p>
    就是说如果你用enterframe去检测动画的动作的话会很失败。。
  </p>