---
id: 171
title: '[原创]android framelayout inflate浮动层方便的封装类'
date: 2013-12-18T13:41:40+08:00
author: 徐艺洲
layout: post
guid: http://www.fireidea.com/archives/171
permalink: /archives/171
categories:
  - Android
  - 经典技巧
tags:
  - it
---
<div id="sina_keyword_ad_area2" class="articalContent   ">
  <div>
    这个类是我实际做android时候做activity内的浮动层效果时封装的类
  </div>
  
  <div>
    如果一个界面有多个浮动层可以简单复制这个类快速做出多个浮动层来
  </div>
  
  <div>
  </div>
  
  <div>
    public class PlayerHeader extends FrameLayout {
  </div>
  
  <div>
    private static final String LOG_TAG =MainActivity.class.getName();
  </div>
  
  <div>
  </div>
  
  <div>
    public PlayerHeader(Context context) {
  </div>
  
  <div>
    super(context);
  </div>
  
  <div>
    mContext = context;
  </div>
  
  <div>
    }
  </div>
  
  <div>
  </div>
  
  <div>
    public PlayerHeader(Context context, AttributeSet attrs){
  </div>
  
  <div>
    super(context, attrs);
  </div>
  
  <div>
    headerRoot = null;
  </div>
  
  <div>
    mContext = context;
  </div>
  
  <div>
    }
  </div>
  
  <div>
  </div>
  
  <div>
    public PlayerHeader(Context context, AttributeSet attrs, intdefStyle) {
  </div>
  
  <div>
    super(context, attrs, defStyle);
  </div>
  
  <div>
    }
  </div>
  
  <div>
    // 这里只调用一次，使用这个函数创建一个布局到framelayout
  </div>
  
  <div>
    public void init(ViewGroup anchorView, VideoView videoview){
  </div>
  
  <div>
    //添加浮动层的framelayout
  </div>
  
  <div>
    mAnchor = anchorView;
  </div>
  
  <div>
    // init header &#8212;&#8212;&#8212;&#8212;&#8212;&#8211;
  </div>
  
  <div>
    LayoutInflater inflate = (LayoutInflater) mContext
  </div>
  
  <div>
    .getSystemService(Context.LAYOUT_INFLATER_SERVICE);
  </div>
  
  <div>
    //新建的层放在framelayout顶部
  </div>
  
  <div>
    FrameLayout.LayoutParams tlp1 = newFrameLayout.LayoutParams(
  </div>
  
  <div>
    ViewGroup.LayoutParams.MATCH_PARENT,
  </div>
  
  <div>
    ViewGroup.LayoutParams.WRAP_CONTENT, Gravity.TOP);
  </div>
  
  <div>
  </div>
  
  <div>
    // init header
  </div>
  
  <div>
    headerRoot = inflate.inflate(R.layout.media_controler_header,null);
  </div>
  
  <div>
    //这个只是示范代码，在这里记录所有控件的句柄
  </div>
  
  <div>
    bTitle = (TextView)headerRoot.findViewById(R.id.video_title);
  </div>
  
  <div>
    bIcon = (ImageView)headerRoot.findViewById(R.id.channel_icon);
  </div>
  
  <div>
    mAnchor.addView(headerRoot, tlp1);
  </div>
  
  <div>
  </div>
  
  <div>
    hide();
  </div>
  
  <div>
  </div>
  
  <div>
    }
  </div>
  
  <div>
    // 控制板显示/
  </div>
  
  <div>
    public void show() {
  </div>
  
  <div>
  </div>
  
  <div>
    headerRoot.setVisibility(VISIBLE);
  </div>
  
  <div>
    mShowing = true;
  </div>
  
  <div>
    }
  </div>
  
  <div>
  </div>
  
  <div>
    public void hide() {
  </div>
  
  <div>
    headerRoot.setVisibility(INVISIBLE);
  </div>
  
  <div>
    mShowing = false;
  </div>
  
  <div>
    }
  </div>
  
  <div>
    }
  </div>
  
  <div>
  </div>
  
  <div>
    使用方法
  </div>
  
  <div>
    <div>
      playerHeader= new <span STYLE="line-height: 21px;">PlayerHeader </span>(this);
    </div>
    
    <div>
      //找到要添加浮动层的framelayout
    </div>
    
    <div>
      playerHeader.init((FrameLayout) findViewById(R.id.ff),player);
    </div>
  </div>
  
  <div>
    playerHeader.show()//显示
  </div>
  
  <div>
    <span STYLE="line-height: 21px;">playerHeader.hide()//显示</span>
  </div>
  
  <div>
  </div>