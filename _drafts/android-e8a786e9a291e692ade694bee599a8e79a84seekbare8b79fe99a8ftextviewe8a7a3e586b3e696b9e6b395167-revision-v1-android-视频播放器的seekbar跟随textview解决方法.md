---
id: 234
title: android 视频播放器的seekbar跟随textview解决方法
date: 2018-10-10T14:10:01+08:00
author: 徐艺洲
layout: revision
guid: http://www.fireidea.com/archives/234
permalink: /archives/234
---
<div id="sina_keyword_ad_area2" class="articalContent   ">
  //在progresschange里面获取seekbar对象的thumb<br />RelativeLayout.LayoutParams paramsStrength = newRelativeLayout.LayoutParams(<br /> RelativeLayout.LayoutParams.WRAP_CONTENT,<br /> RelativeLayout.LayoutParams.WRAP_CONTENT);</p> 
  
  <p>
    leftmargin = ((MySeekBar)mProgress).getSeekBarThumb().getBounds()<br /> .centerX()<br /> &#8211; tvWeight.getWidth() / 2+10;<br /> if (leftmargin < 0)<br /> leftmargin = 0;<br /> paramsStrength.leftMargin = leftmargin;<br /> tvWeight.setLayoutParams(paramsStrength);<br /> tvWeight.setText(stringForTime(mPlayer.getCurrentPosition()));
  </p>
  
  <p>
    //继承重写seekbar代码以获取thumb的对象<br />import android.content.Context;<br />import android.graphics.drawable.Drawable;<br />import android.util.AttributeSet;<br />import android.widget.SeekBar;
  </p>
  
  <p>
    public class MySeekBar extends SeekBar {<br /> DrawablemThumb;
  </p>
  
  <p>
    publicMySeekBar(Context context) {<br /> super(context);<br /> }<br /> publicMySeekBar(Context context, AttributeSet attrs) {<br /> super(context, attrs);<br /> }<br /> publicMySeekBar(Context context, AttributeSet attrs, int defStyle){<br /> super(context, attrs, defStyle);<br /> }
  </p>
  
  <p>
    @Override<br /> public voidsetThumb(Drawable thumb) {<br /> super.setThumb(thumb);<br /> mThumb =thumb;<br /> }<br /> publicDrawable getSeekBarThumb() {<br /> returnmThumb;<br /> }
  </p>
  
  <p>
    }
  </p>