---
id: 212
title: c++ 11 map基础value排序
date: 2018-10-10T14:04:02+08:00
author: 徐艺洲
layout: revision
guid: http://www.fireidea.com/archives/212
permalink: /archives/212
---
<div id="sina_keyword_ad_area2" class="articalContent   newfont_family">
  本身是不支持的，但是可以用其他方法制作出来，参考了下别人的资料</p> 
  
  <div>
    发现缺少说明，这次转过来只是加个说明</p> 
    
    <div>
    </div>
    
    <div>
      <div>
        #include <b><font STYLE="font-size: 14px;"> 《<span STYLE="margin: 0px; padding: 0px; border: none; font-family: Consolas, 'Courier new', Courier, mono, serif; line-height: 18px; background-color: rgb(255, 255, 255);">algorithm》</span></font></b>
      </div>
      
      <div>
        //pair类型定义
      </div>
      
      <div>
        typedef pair PAIR;
      </div>
      
      <div>
      </div>
      
      <div>
        //排序对比函数
      </div>
      
      <div>
        int cmp(const PAIR &x, const PAIR &y)
      </div>
      
      <div>
        {
      </div>
      
      <div>
        return x.second > y.second;
      </div>
      
      <div>
        }
      </div>
      
      <div>
      </div>
      
      <div>
        <span STYLE="line-height: 21px;">std::vector《PAIR》 pair_vec;//排序用的vector对象（原文少了这个）</span>
      </div>
      
      <div>
        <span STYLE="line-height: 21px;"><br /></span>
      </div>
      
      <div>
        //将要排序的map数据遍历并将其值插入到vector对象内
      </div>
      
      <div>
        for (map::iterator map_iter = map_verb.begin(); map_iter !=map_verb.end(); ++map_iter)
      </div>
      
      <div>
        {
      </div>
      
      <div>
        pair_vec.push_back(make_pair(map_iter->first,map_iter->second));
      </div>
      
      <div>
        }
      </div>
      
      <div>
      </div>
      
      <div>
        //对vector进行排序
      </div>
      
      <div>
        sort(pair_vec.begin(), pair_vec.end(),cmp);
      </div>
      
      <div>
      </div>
      
      <div>
        //排序结果输出
      </div>
      
      <div>
        for (vector::iterator curr = pair_vec.begin(); curr !=pair_vec.end(); ++curr)
      </div>
      
      <div>
        {
      </div>
      
      <div>
        outfile << curr->first << &#8220;\t&#8221; <<curr->second << endl;
      </div>
      
      <div>
        }
      </div>
      
      <div>
      </div>
      
      <div>
        转自：http://blog.csdn.net/glp_hit/article/details/8109594
      </div>
      
      <div>
      </div>
    </div>
  </div>