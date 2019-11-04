---
id: 101
title: epoll和select
date: 2010-04-29T13:04:49+08:00
author: 徐艺洲
layout: post
guid: http://www.fireidea.com/archives/101
permalink: /archives/101
categories:
  - 经典技巧
tags:
  - it
---
<div id="sina_keyword_ad_area2" class="articalContent   ">
  <p>
    转载的，感觉不错，所以留下备份
  </p>
  
  <p>
    来自于：http://m.cnblogs.com/62707/1681172.html
  </p>
  
  <p>
  </p>
  
  <p>
    epoll是Kernel 2.6后新加入的事件机制，在高并发条件下，远优于select.
  </p>
  
  <p>
    用个硬件中的例子吧，可能不太恰当：epoll相当于I／O中断（有的时候才相应），而select相当于轮询（总要反复查询）。
  </p>
  
  <p>
    其实epoll比slect好用很多， 主要一下几个用法。
  </p>
  
  <p>
    struct epoll_event ;epoll事件体，事件发生时候你可以得到一个它。其中epoll_event.data.fd可以存储关联的句柄，epoll_event.event是监听标志，常用的有EPOLLIN （有数据，可以读）、EPOLLOUT（有数据，可以写）EPOLLET（有事件，通用）;
  </p>
  
  <p>
    （1） 创建epoll句柄
  </p>
  
  <p>
    int epFd = epoll_create(EPOLL_SIZE);
  </p>
  
  <p>
    （2）加入一个句柄到 epoll的监听队列
  </p>
  
  <p>
    ev.data.fd = serverFd;<br />ev.events = EPOLLIN | EPOLLET;<br />epoll_ctl(epFd, EPOLL_CTL_ADD, serverFd, &ev);
  </p>
  
  <p>
    上 面的fd是你要绑定给事件发生时候使用的fd,到时候只能操作这个，下面是事件类型。
  </p>
  
  <p>
    使用epoll_ctl添加到之中，EPOLL_CTL_ADD是epoll控制类型，这里监听的fd和给event的fd一般相同。
  </p>
  
  <p>
    (3)等待event返回
  </p>
  
  <p>
    int nfds = epoll_wait(epFd, evs, EVENT_ARR, -1);
  </p>
  
  <p>
    传入的evs是epoll_event的数组，EVENT_ARR应当是不超过这个数组的长度。返回nfds的是不超过EVENT_ARR的数值，表示本次等待到了几个事件。
  </p>
  
  <p>
    (4)遍历事件
  </p>
  
  <p>
    注意，这里遍历的事件是肯定已经发生了的，而select中遍历的是每个fd，而fd不一定在FDSET中（即不一定有读事件发生）！这是效率最大的差别所在！
  </p>
  
  <p>
    for (int i = 0; i < nfds; i++)
  </p>
  
  <p>
    {
  </p>
  
  <p>
    //do something
  </p>
  
  <p>
    }
  </p>
  
  <p>
    (5)其他技巧
  </p>
  
  <p>
    对事件是否是 serverFd判断，如果是，进行accept并加入epoll监听队列，要设置异步读取。
  </p>
  
  <p>
    如果evts[i]&EPOLLIN，表示可读，使用read进行试探，如果>0表示连接没有关闭，否则连接已经关闭（出发事件又读取不到东西，表示socket关闭！）。如果<0出错。如果>0，需要继续读取直到为0，但是注意这里的为0是在第一次read不为0的前提下，毕竟我们设置了异步读取，暂时没有数据可以读就返回0了！而如果第一次返回0，那么就是关闭吧！
  </p>
  
  <p>
    注意关闭要移出出epoll并且 close(clientFd)
  </p>
  
  <p>
    罗嗦了好多，看代码！
  </p>
  
  <div>
    <div>
    </div>
  </div><div ALT1=""> 
  
  <table>
    <tr>
      <td>
        <code>1</code>
      </td>
      
      <td>
      </td>
    </tr>
  </table>
</div><div ALT1=""> 

<table>
  <tr>
    <td>
      <code>11</code>
    </td>
    
    <td>
      <code PREPROCESSOR="">#include&lt;stdio.h&gt;</code>
    </td>
  </tr>
</table></div> <div ALT2=""> 

<table>
  <tr>
    <td>
      <code>12</code>
    </td>
    
    <td>
      <code PREPROCESSOR="">#include&lt;stdlib.h&gt;</code>
    </td>
  </tr>
</table></div> <div ALT1=""> 

<table>
  <tr>
    <td>
      <code>13</code>
    </td>
    
    <td>
      <code PREPROCESSOR="">#include&lt;unistd.h&gt;</code>
    </td>
  </tr>
</table></div> <div ALT2=""> 

<table>
  <tr>
    <td>
      <code>14</code>
    </td>
    
    <td>
      <code PREPROCESSOR="">#include&lt;fcntl.h&gt;</code>
    </td>
  </tr>
</table></div> <div ALT1=""> 

<table>
  <tr>
    <td>
      <code>15</code>
    </td>
    
    <td>
      <code PREPROCESSOR="">#include&lt;arpa/inet.h&gt;</code>
    </td>
  </tr>
</table></div> <div ALT2=""> 

<table>
  <tr>
    <td>
      <code>16</code>
    </td>
    
    <td>
      <code PREPROCESSOR="">#include&lt;netinet/in.h&gt;</code>
    </td>
  </tr>
</table></div> <div ALT1=""> 

<table>
  <tr>
    <td>
      <code>17</code>
    </td>
    
    <td>
      <code PREPROCESSOR="">#include&lt;sys/epoll.h&gt;</code>
    </td>
  </tr>
</table></div> <div ALT2=""> 

<table>
  <tr>
    <td>
      <code>18</code>
    </td>
    
    <td>
      <code PREPROCESSOR="">#include&lt;errno.h&gt;</code>
    </td>
  </tr>
</table></div> <div ALT1=""> 

<table>
  <tr>
    <td>
      <code>19</code>
    </td>
    
    <td>
    </td>
  </tr>
</table></div> <div ALT2=""> 

<table>
  <tr>
    <td>
      <code>20</code>
    </td>
    
    <td>
      <code PREPROCESSOR="">#define EPOLL_SIZE 10</code>
    </td>
  </tr>
</table></div> <div ALT1=""> 

<table>
  <tr>
    <td>
      <code>21</code>
    </td>
    
    <td>
      <code PREPROCESSOR="">#define EVENT_ARR 20</code>
    </td>
  </tr>
</table></div> <div ALT2=""> 

<table>
  <tr>
    <td>
      <code>22</code>
    </td>
    
    <td>
      <code PREPROCESSOR="">#define BACK_QUEUE 10</code>
    </td>
  </tr>
</table></div> <div ALT1=""> 

<table>
  <tr>
    <td>
      <code>23</code>
    </td>
    
    <td>
      <code PREPROCESSOR="">#define PORT 18001</code>
    </td>
  </tr>
</table></div> <div ALT2=""> 

<table>
  <tr>
    <td>
      <code>24</code>
    </td>
    
    <td>
      <code PREPROCESSOR="">#define BUF_SIZE 16</code>
    </td>
  </tr>
</table></div> <div ALT1=""> 

<table>
  <tr>
    <td>
      <code>25</code>
    </td>
    
    <td>
    </td>
  </tr>
</table></div> <div ALT2=""> 

<table>
  <tr>
    <td>
      <code>26</code>
    </td>
    
    <td>
      <code KEYWORD="" BOLD="">void</code> <code PLAIN="">setnonblocking(</code><code COLOR1="" BOLD="">int</code><code PLAIN="">sockFd) {</code>
    </td>
  </tr>
</table></div> <div ALT1=""> 

<table>
  <tr>
    <td>
      <code>27</code>
    </td>
    
    <td>
      <code>    </code><code COLOR1="" BOLD="">int</code> <code PLAIN="">opt;</code>
    </td>
  </tr>
</table></div> <div ALT2=""> 

<table>
  <tr>
    <td>
      <code>28</code>
    </td>
    
    <td>
    </td>
  </tr>
</table></div> <div ALT1=""> 

<table>
  <tr>
    <td>
      <code>29</code>
    </td>
    
    <td>
      <code>    </code><code COMMENTS="">//获取sock原来的flag</code>
    </td>
  </tr>
</table></div> <div ALT2=""> 

<table>
  <tr>
    <td>
      <code>30</code>
    </td>
    
    <td>
      <code>    </code><code PLAIN="">opt= fcntl(sockFd, F_GETFL);</code>
    </td>
  </tr>
</table></div> <div ALT1=""> 

<table>
  <tr>
    <td>
      <code>31</code>
    </td>
    
    <td>
      <code>    </code><code KEYWORD="" BOLD="">if</code> <code PLAIN="">(opt &lt; 0){</code>
    </td>
  </tr>
</table></div> <div ALT2=""> 

<table>
  <tr>
    <td>
      <code>32</code>
    </td>
    
    <td>
      <code>        </code><code FUNCTIONS="" BOLD="">printf</code><code PLAIN="">(</code><code STRING="">"fcntl(F_GETFL) fail."</code><code PLAIN="">);</code>
    </td>
  </tr>
</table></div> <div ALT1=""> 

<table>
  <tr>
    <td>
      <code>33</code>
    </td>
    
    <td>
      <code>        </code><code FUNCTIONS="" BOLD="">exit</code><code PLAIN="">(-1);</code>
    </td>
  </tr>
</table></div> <div ALT2=""> 

<table>
  <tr>
    <td>
      <code>34</code>
    </td>
    
    <td>
      <code>    </code><code PLAIN="">}</code>
    </td>
  </tr>
</table></div> <div ALT1=""> 

<table>
  <tr>
    <td>
      <code>35</code>
    </td>
    
    <td>
    </td>
  </tr>
</table></div> <div ALT2=""> 

<table>
  <tr>
    <td>
      <code>36</code>
    </td>
    
    <td>
      <code>    </code><code COMMENTS="">//设置新的flag,非阻塞</code>
    </td>
  </tr>
</table></div> <div ALT1=""> 

<table>
  <tr>
    <td>
      <code>37</code>
    </td>
    
    <td>
      <code>    </code><code PLAIN="">opt|= O_NONBLOCK;</code>
    </td>
  </tr>
</table></div> <div ALT2=""> 

<table>
  <tr>
    <td>
      <code>38</code>
    </td>
    
    <td>
      <code>    </code><code KEYWORD="" BOLD="">if</code> <code PLAIN="">(fcntl(sockFd, F_SETFL, opt)&lt; 0) {</code>
    </td>
  </tr>
</table></div> <div ALT1=""> 

<table>
  <tr>
    <td>
      <code>39</code>
    </td>
    
    <td>
      <code>        </code><code FUNCTIONS="" BOLD="">printf</code><code PLAIN="">(</code><code STRING="">"fcntl(F_SETFL) fail."</code><code PLAIN="">);</code>
    </td>
  </tr>
</table></div> <div ALT2=""> 

<table>
  <tr>
    <td>
      <code>40</code>
    </td>
    
    <td>
      <code>        </code><code FUNCTIONS="" BOLD="">exit</code><code PLAIN="">(-1);</code>
    </td>
  </tr>
</table></div> <div ALT1=""> 

<table>
  <tr>
    <td>
      <code>41</code>
    </td>
    
    <td>
      <code>    </code><code PLAIN="">}</code>
    </td>
  </tr>
</table></div> <div ALT2=""> 

<table>
  <tr>
    <td>
      <code>42</code>
    </td>
    
    <td>
      <code PLAIN="">}</code>
    </td>
  </tr>
</table></div> <div ALT1=""> 

<table>
  <tr>
    <td>
      <code>43</code>
    </td>
    
    <td>
    </td>
  </tr>
</table></div> <div ALT2=""> 

<table>
  <tr>
    <td>
      <code>44</code>
    </td>
    
    <td>
      <code COLOR1="" BOLD="">int</code> <code PLAIN="">main(){</code>
    </td>
  </tr>
</table></div> <div ALT1=""> 

<table>
  <tr>
    <td>
      <code>45</code>
    </td>
    
    <td>
    </td>
  </tr>
</table></div> <div ALT2=""> 

<table>
  <tr>
    <td>
      <code>46</code>
    </td>
    
    <td>
      <code>    </code><code COLOR1="" BOLD="">int</code> <code PLAIN="">serverFd;</code>
    </td>
  </tr>
</table></div> <div ALT1=""> 

<table>
  <tr>
    <td>
      <code>47</code>
    </td>
    
    <td>
    </td>
  </tr>
</table></div> <div ALT2=""> 

<table>
  <tbo dy></p> 
  
  <tr>
    <td>
      <code>48</code>
    </td>
    
    <td>
      <code>    </code><code COMMENTS="">//创建服务器fd</code>
    </td>
  </tr></tbody>
</table></div> <div ALT1=""> 

<table>
  <tr>
    <td>
      <code>49</code>
    </td>
    
    <td>
      <code>    </code><code PLAIN="">serverFd= socket(AF_INET, SOCK_STREAM, 0);</code>
    </td>
  </tr>
</table></div> <div ALT2=""> 

<table>
  <tr>
    <td>
      <code>50</code>
    </td>
    
    <td>
      <code>    </code><code PLAIN="">setnonblocking(serverFd);</code>
    </td>
  </tr>
</table></div> <div ALT1=""> 

<table>
  <tr>
    <td>
      <code>51</code>
    </td>
    
    <td>
    </td>
  </tr>
</table></div> <div ALT2=""> 

<table>
  <tr>
    <td>
      <code>52</code>
    </td>
    
    <td>
      <code>    </code><code COMMENTS="">//创建epoll，并把serverFd放入监听队列</code>
    </td>
  </tr>
</table></div> <div ALT1=""> 

<table>
  <tr>
    <td>
      <code>53</code>
    </td>
    
    <td>
      <code>    </code><code COLOR1="" BOLD="">int</code> <code PLAIN="">epFd =epoll_create(EPOLL_SIZE);</code>
    </td>
  </tr>
</table></div> <div ALT2=""> 

<table>
  <tr>
    <td>
      <code>54</code>
    </td>
    
    <td>
      <code>    </code><code KEYWORD="" BOLD="">struct</code> <code PLAIN="">epoll_event ev,evs[EVENT_ARR];</code>
    </td>
  </tr>
</table></div> <div ALT1=""> 

<table>
  <tr>
    <td>
      <code>55</code>
    </td>
    
    <td>
      <code>    </code><code PLAIN="">ev.data.fd= serverFd;</code>
    </td>
  </tr>
</table></div> <div ALT2=""> 

<table>
  <tr>
    <td>
      <code>56</code>
    </td>
    
    <td>
      <code>    </code><code PLAIN="">ev.events= EPOLLIN | EPOLLET;</code>
    </td>
  </tr>
</table></div> <div ALT1=""> 

<table>
  <tr>
    <td>
      <code>57</code>
    </td>
    
    <td>
      <code>    </code><code PLAIN="">epoll_ctl(epFd,EPOLL_CTL_ADD, serverFd, &ev);</code>
    </td>
  </tr>
</table></div> <div ALT2=""> 

<table>
  <tr>
    <td>
      <code>58</code>
    </td>
    
    <td>
    </td>
  </tr>
</table></div> <div ALT1=""> 

<table>
  <tr>
    <td>
      <code>59</code>
    </td>
    
    <td>
      <code>    </code><code COMMENTS="">//绑定服务器端口</code>
    </td>
  </tr>
</table></div> <div ALT2=""> 

<table>
  <tr>
    <td>
      <code>60</code>
    </td>
    
    <td>
      <code>    </code><code KEYWORD="" BOLD="">struct</code> <code PLAIN="">sockaddr_inserverAddr;</code>
    </td>
  </tr>
</table></div> <div ALT1=""> 

<table>
  <tr>
    <td>
      <code>61</code>
    </td>
    
    <td>
      <code>    </code><code PLAIN="">socklen_tserverLen =</code> <code KEYWORD="" BOLD="">sizeof</code><code PLAIN="">(</code><code KEYWORD="" BOLD="">struct</code> <code PLAIN="">sockaddr_in);</code>
    </td>
  </tr>
</table></div> <div ALT2=""> 

<table>
  <tr>
    <td>
      <code>62</code>
    </td>
    
    <td>
      <code>    </code><code PLAIN="">serverAddr.sin_addr.s_addr= htonl(INADDR_ANY);</code>
    </td>
  </tr>
</table></div> <div ALT1=""> 

<table>
  <tr>
    <td>
      <code>63</code>
    </td>
    
    <td>
      <code>    </code><code PLAIN="">serverAddr.sin_port= htons(PORT);</code>
    </td>
  </tr>
</table></div> <div ALT2=""> 

<table>
  <tr>
    <td>
      <code>64</code>
    </td>
    
    <td>
      <code>    </code><code KEYWORD="" BOLD="">if</code> <code PLAIN="">(bind(serverFd,(</code><code KEYWORD="" BOLD="">struct</code> <code PLAIN="">sockaddr *) &serverAddr, serverLen)){</code>
    </td>
  </tr>
</table></div> <div ALT1=""> 

<table>
  <tr>
    <td>
      <code>65</code>
    </td>
    
    <td>
      <code>        </code><code FUNCTIONS="" BOLD="">printf</code><code PLAIN="">(</code><code STRING="">"bind()fail.\n"</code><code PLAIN="">);</code>
    </td>
  </tr>
</table></div> <div ALT2=""> 

<table>
  <tr>
    <td>
      <code>66</code>
    </td>
    
    <td>
      <code>        </code><code FUNCTIONS="" BOLD="">exit</code><code PLAIN="">(-1);</code>
    </td>
  </tr>
</table></div> <div ALT1=""> 

<table>
  <tr>
    <td>
      <code>67</code>
    </td>
    
    <td>
      <code>    </code><code PLAIN="">}</code>
    </td>
  </tr>
</table></div> <div ALT2=""> 

<table>
  <tr>
    <td>
      <code>68</code>
    </td>
    
    <td>
    </td>
  </tr>
</table></div> <div ALT1=""> 

<table>
  <tr>
    <td>
      <code>69</code>
    </td>
    
    <td>
      <code>    </code><code COMMENTS="">//打开监听</code>
    </td>
  </tr>
</table></div> <div ALT2=""> 

<table>
  <tr>
    <td>
      <code>70</code>
    </td>
    
    <td>
      <code>    </code><code KEYWORD="" BOLD="">if</code> <code PLAIN="">(listen(serverFd, BACK_QUEUE)){</code>
    </td>
  </tr>
</table></div> <div ALT1=""> 

<table>
  <tr>
    <td>
      <code>71</code>
    </td>
    
    <td>
      <code>        </code><code FUNCTIONS="" BOLD="">printf</code><code PLAIN="">(</code><code STRING="">"Listenfail.\n"</code><code PLAIN="">);</code>
    </td>
  </tr>
</table></div> <div ALT2=""> 

<table>
  <tr>
    <td>
      <code>72</code>
    </td>
    
    <td>
      <code>        </code><code FUNCTIONS="" BOLD="">exit</code><code PLAIN="">(-1);</code>
    </td>
  </tr>
</table></div> <div ALT1=""> 

<table>
  <tr>
    <td>
      <code>73</code>
    </td>
    
    <td>
      <code>    </code><code PLAIN="">}</code>
    </td>
  </tr>
</table></div> <div ALT2=""> 

<table>
  <tr>
    <td>
      <code>74</code>
    </td>
    
    <td>
    </td>
  </tr>
</table></div> <div ALT1=""> 

<table>
  <tr>
    <td>
      <code>75</code>
    </td>
    
    <td>
      <code>    </code><code COMMENTS="">//死循环处理</code>
    </td>
  </tr>
</table></div> <div ALT2=""> 

<table>
  <tr>
    <td>
      <code>76</code>
    </td>
    
    <td>
      <code>    </code><code COLOR1="" BOLD="">int</code> <code PLAIN="">clientFd;</code>
    </td>
  </tr>
</table></div> <div ALT1=""> 

<table>
  <tr>
    <td>
      <code>77</code>
    </td>
    
    <td>
      <code>    </code><code PLAIN="">sockaddr_inclientAddr;</code>
    </td>
  </tr>
</table></div> <div ALT2=""> 

<table>
  <tr>
    <td>
      <code>78</code>
    </td>
    
    <td>
      <code>    </code><code PLAIN="">socklen_tclientLen;</code>
    </td>
  </tr>
</table></div> <div ALT1=""> 

<table>
  <tr>
    <td>
      <code>79</code>
    </td>
    
    <td>
      <code>    </code><code COLOR1="" BOLD="">char</code> <code PLAIN="">buf[BUF_SIZE];</code>
    </td>
  </tr>
</table></div> <div ALT2=""> 

<table>
  <tr>
    <td>
      <code>80</code>
    </td>
    
    <td>
      <code>    </code><code KEYWORD="" BOLD="">while</code> <code PLAIN="">(1) {</code>
    </td>
  </tr>
</table></div> <div ALT1=""> 

<table>
  <tr>
    <td>
      <code>81</code>
    </td>
    
    <td>
      <code>        </code><code COMMENTS="">//等待epoll事件的到来，最多取EVENT_ARR个事件</code>
    </td>
  </tr>
</table></div> <div ALT2=""> 

<table>
  <tr>
    <td>
      <code>82</code>
    </td>
    
    <td>
      <code>        </code><code COLOR1="" BOLD="">int</code> <code PLAIN="">nfds = epoll_wait(epFd, evs,EVENT_ARR, -1);</code>
    </td>
  </tr>
</table></div> <div ALT1=""> 

<table>
  <tr>
    <td>
      <code>83</code>
    </td>
    
    <td>
      <code>        </code><code COMMENTS="">//处理事件</code>
    </td>
  </tr>
</table></div> <div ALT2=""> 

<table>
  <tr>
    <td>
      <code>84</code>
    </td>
    
    <td>
      <code>        </code><code KEYWORD="" BOLD="">for</code> <code PLAIN="">(</code><code COLOR1="" BOLD="">int</code> <code PLAIN="">i = 0; i &lt; nfds; i++){</code>
    </td>
  </tr>
</table></div> <div ALT1=""> 

<table>
  <tr>
    <td>
      <code>85</code>
    </td>
    
    <td>
      <code>            </code><code KEYWORD="" BOLD="">if</code> <code PLAIN="">(evs[i].data.fd == serverFd&& evs[i].data.fd &EPOLLIN) {</code>
    </td>
  </tr>
</table></div> <div ALT2=""> 

<table>
  <tr>
    <td>
      <code>86</code>
    </td>
    
    <td>
      <code>                </code><code COMMENTS="">//如果是serverFd,表明有新连接连入</code>
    </td>
  </tr>
</table></div> <div ALT1=""> 

<table>
  <tr>
    <td>
      <code>87</code>
    </td>
    
    <td>
      <code>                </code><code KEYWORD="" BOLD="">if</code> <code PLAIN="">((clientFd =accept(serverFd,</code>
    </td>
  </tr>
</table></div> <div ALT2=""> 

<table>
  <tr>
    <td>
      <code>88</code>
    </td>
    
    <td>
      <code>                        </code><code PLAIN="">(</code><code KEYWORD="" BOLD="">struct</code> <code PLAIN="">sockaddr *)&clientAddr, &clientLen))&lt; 0) {</code>
    </td>
  </tr>
</table></div> <div ALT1=""> 

<table>
  <tr>
    <td>
      <code>89</code>
    </td>
    
    <td>
      <code>                    </code><code FUNCTIONS="" BOLD="">printf</code><code PLAIN="">(</code><code STRING="">"acceptfail.\n"</code><code PLAIN="">);</code>
    </td>
  </tr>
</table></div> <div ALT2=""> 

<table>
  <tr>
    <td>
      <code>90</code>
    </td>
    
    <td>
      <code>                </code><code PLAIN="">}</code>
    </td>
  </tr>
</table></div> <div ALT1=""> 

<table>
  <tr>
    <td>
      <code>91</code>
    </td>
    
    <td>
      <code>                </code><code FUNCTIONS="" BOLD="">printf</code><code PLAIN="">(</code><code STRING="">"Connect from %s:%d\n"</code><code PLAIN="">,inet_ntoa(clientAddr.sin_addr),</code>
    </td>
  </tr>
</table></div> <div ALT2=""> 

<table>
  <tr>
    <td>
      <code>92</code>
    </td>
    
    <td>
      <code>                        </code><code PLAIN="">htons(clientAddr.sin_port));</code>
    </td>
  </tr>
</table></div> <div ALT1=""> 

<table>
  <tr>
    <td>
      <code>93</code>
    </td>
    
    <td>
      <code>&lt;br />
       </code><code PLAIN="">setnonblocking(clientFd);</code>
    </td>
  </tr>
</table></div> <div ALT2=""> 

<table>
  <tr>
    <td>
      <code>94</code>
    </td>
    
    <td>
      <code>                </code><code COMMENTS="">//注册accept()到的连接</code>
    </td>
  </tr>
</table></div> <div ALT1=""> 

<table>
  <tr>
    <td>
      <code>95</code>
    </td>
    
    <td>
      <code>                </code><code PLAIN="">ev.data.fd= clientFd;</code>
    </td>
  </tr>
</table></div> <div ALT2=""> 

<table>
  <tr>
    <td>
      <code>96</code>
    </td>
    
    <td>
      <code>                </code><code PLAIN="">ev.events= EPOLLIN | EPOLLET;</code>
    </td>
  </tr>
</table></div> <div ALT1=""> 

<table>
  <tr>
    <td>
      <code>97</code>
    </td>
    
    <td>
      <code>                </code><code PLAIN="">epoll_ctl(epFd,EPOLL_CTL_ADD, clientFd, &ev);</code>
    </td>
  </tr>
</table></div> <div ALT2=""> 

<table>
  <tr>
    <td>
      <code>98</code>
    </td>
    
    <td>
      <code>            </code><code PLAIN="">}</code><code KEYWORD="" BOLD="">else</code> <code KEYWORD="" BOLD="">if</code> <code PLAIN="">(evs[i].events &EPOLLIN) {</code>
    </td>
  </tr>
</table></div> <div ALT1=""> 

<table>
  <tr>
    <td>
      <code>99</code>
    </td>
    
    <td>
      <code>                </code><code COMMENTS="">//如果不是serverFd,则是client的可读</code>
    </td>
  </tr>
</table></div> <div ALT2=""> 

<table>
  <tr>
    <td>
      <code>100</code>
    </td>
    
    <td>
      <code>                </code><code KEYWORD="" BOLD="">if</code> <code PLAIN="">((clientFd = evs[i].data.fd)&gt; 0) {</code>
    </td>
  </tr>
</table></div> <div ALT1=""> 

<table>
  <tr>
    <td>
      <code>101</code>
    </td>
    
    <td>
      <code>                    </code><code COMMENTS="">//先进行试探性读取</code>
    </td>
  </tr>
</table></div> <div ALT2=""> 

<table>
  <tr>
    <td>
      <code>102</code>
    </td>
    
    <td>
      <code>                    </code><code COLOR1="" BOLD="">int</code> <code PLAIN="">len = read(clientFd, buf,BUF_SIZE);</code>
    </td>
  </tr>
</table></div> <div ALT1=""> 

<table>
  <tr>
    <td>
      <code>103</code>
    </td>
    
    <td>
      <code>                    </code><code KEYWORD="" BOLD="">if</code> <code PLAIN="">(len &gt; 0){</code>
    </td>
  </tr>
</table></div> <div ALT2=""> 

<table>
  <tr>
    <td>
      <code>104</code>
    </td>
    
    <td>
      <code>                        </code><code COMMENTS="">//有数据可以读，Echo写入</code>
    </td>
  </tr>
</table></div> <div ALT1=""> 

<table>
  <tr>
    <td>
      <code>105</code>
    </td>
    
    <td>
      <code>                        </code><code KEYWORD="" BOLD="">do</code> <code PLAIN="">{</code>
    </td>
  </tr>
</table></div> <div ALT2=""> 

<table>
  <tr>
    <td>
      <code>106</code>
    </td>
    
    <td>
      <code>                            </code><code KEYWORD="" BOLD="">if</code> <code PLAIN="">(write(clientFd, buf, len)&lt; 0) {</code>
    </td>
  </tr>
</table></div> <div ALT1=""> 

<table>
  <tr>
    <td>
      <code>107</code>
    </td>
    
    <td>
      <code>                                </code><code FUNCTIONS="" BOLD="">printf</code><code PLAIN="">(</code><code STRING="">"write() fail.\n"</code><code PLAIN="">);</code>
    </td>
  </tr>
</table></div> <div ALT2=""> 

<table>
  <tr>
    <td>
      <code>108</code>
    </td>
    
    <td>
      <code>                            </code><code PLAIN="">}</code>
    </td>
  </tr>
</table></div> <div ALT1=""> 

<table>
  <tr>
    <td>
      <code>109</code>
    </td>
    
    <td>
      <code>                            </code><code PLAIN="">len= read(clientFd, buf, BUF_SIZE);</code>
    </td>
  </tr>
</table></div> <div ALT2=""> 

<table>
  <tr>
    <td>
      <code>110</code>
    </td>
    
    <td>
      <code>                        </code><code PLAIN="">}</code><code KEYWORD="" BOLD="">while</code> <code PLAIN="">(len&gt; 0);</code>
    </td>
  </tr>
</table></div> <div ALT1=""> 

<table>
  <tr>
    <td>
      <code>111</code>
    </td>
    
    <td>
      <code>                    </code><code PLAIN="">}</code><code KEYWORD="" BOLD="">else</code> <code KEYWORD="" BOLD="">if</code> <code PLAIN="">(len == 0) {</code>
    </td>
  </tr>
</table></div> <div ALT2=""> 

<table>
  <tr>
    <td>
      <code>112</code>
    </td>
    
    <td>
      <code>                        </code><code COMMENTS="">//出发了EPOLLIN事件，却没有可以读取的，表示断线</code>
    </td>
  </tr>
</table></div> <div ALT1=""> 

<table>
  <tr>
    <td>
      <code>113</code>
    </td>
    
    <td>
      <code>                        </code><code FUNCTIONS="" BOLD="">printf</code><code PLAIN="">(</code><code STRING="">"Clientclosed at %d\n"</code><code PLAIN="">, clientFd);</code>
    </td>
  </tr>
</table></div> <div ALT2=""> 

<table>
  <tr>
    <td>
      <code>114</code>
    </td>
    
    <td>
      <code>                        </code><code PLAIN="">epoll_ctl(epFd,EPOLL_CTL_DEL, clientFd, &ev);</code>
    </td>
  </tr>
</table></div> <div ALT1=""> 

<table>
  <tr>
    <td>
      <code>115</code>
    </td>
    
    <td>
      <code>                        </code><code PLAIN="">close(clientFd);</code>
    </td>
  </tr>
</table></div> <div ALT2=""> 

<table>
  <tr>
    <td>
      <code>116</code>
    </td>
    
    <td>
      <code>                        </code><code PLAIN="">evs[i].data.fd= -1;</code>
    </td>
  </tr>
</table></div> <div ALT1=""> 

<table>
  <tr>
    <td>
      <code>117</code>
    </td>
    
    <td>
      <code>                        </code><code KEYWORD="" BOLD="">break</code><code PLAIN="">;</code>
    </td>
  </tr>
</table></div> <div ALT2=""> 

<table>
  <tr>
    <td>
      <code>118</code>
    </td>
    
    <td>
      <code>                    </code><code PLAIN="">}</code><code KEYWORD="" BOLD="">else</code> <code KEYWORD="" BOLD="">if</code> <code PLAIN="">(len == EAGAIN) {</code>
    </td>
  </tr>
</table></div> <div ALT1=""> 

<table>
  <tr>
    <td>
      <code>119</code>
    </td>
    
    <td>
      <code>                        </code><code KEYWORD="" BOLD="">continue</code><code PLAIN="">;</code>
    </td>
  </tr>
</table></div> <div ALT2=""> 

<table>
  <tr>
    <td>
      <code>120</code>
    </td>
    
    <td>
      <code>                    </code><code PLAIN="">}</code><code KEYWORD="" BOLD="">else</code> <code PLAIN="">{</code>
    </td>
  </tr>
</table></div> <div ALT1=""> 

<table>
  <tr>
    <td>
      <code>121</code>
    </td>
    
    <td>
      <code>                        </code><code COMMENTS="">//client读取出错</code>
    </td>
  </tr>
</table></div> <div ALT2=""> 

<table>
  <tr>
    <td>
      <code>122</code>
    </td>
    
    <td>
      <code>                        </code><code FUNCTIONS="" BOLD="">printf</code><code PLAIN="">(</code><code STRING="">"read()fail."</code><code PLAIN="">);</code>
    </td>
  </tr>
</table></div> <div ALT1=""> 

<table>
  <tr>
    <td>
      <code>123</code>
    </td>
    
    <td>
      <code>                    </code><code PLAIN="">}</code>
    </td>
  </tr>
</table></div> <div ALT2=""> 

<table>
  <tr>
    <td>
      <code>124</code>
    </td>
    
    <td>
      <code>                </code><code PLAIN="">}</code>
    </td>
  </tr>
</table></div> <div ALT1=""> 

<table>
  <tr>
    <td>
      <code>125</code>
    </td>
    
    <td>
      <code>            </code><code PLAIN="">}</code><code KEYWORD="" BOLD="">else</code> <code PLAIN="">{</code>
    </td>
  </tr>
</table></div> <div ALT2=""> 

<table>
  <tr>
    <td>
      <code>126</code>
    </td>
    
    <td>
      <code>                </code><code FUNCTIONS="" BOLD="">printf</code><code PLAIN="">(</code><code STRING="">"otherevent.\n"</code><code PLAIN="">);</code>
    </td>
  </tr>
</table></div> <div ALT1=""> 

<table>
  <tr>
    <td>
      <code>127</code>
    </td>
    
    <td>
      <code>            </code><code PLAIN="">}</code>
    </td>
  </tr>
</table></div> <div ALT2=""> 

<table>
  <tr>
    <td>
      <code>128</code>
    </td>
    
    <td>
      <code>        </code><code PLAIN="">}</code>
    </td>
  </tr>
</table></div> <div ALT1=""> 

<table>
  <tr>
    <td>
      <code>129</code>
    </td>
    
    <td>
      <code>    </code><code PLAIN="">}</code>
    </td>
  </tr>
</table></div> <div ALT2=""> 

<table>
  <tr>
    <td>
      <code>130</code>
    </td>
    
    <td>
    </td>
  </tr>
</table></div> <div ALT1=""> 

<table>
  <tr>
    <td>
      <code>131</code>
    </td>
    
    <td>
      <code>    </code><code KEYWORD="" BOLD="">return</code> <code PLAIN="">0;</code>
    </td>
  </tr>
</table></div> 

<table>
  <tr>
    <td>
      <code>132</code>
    </td>
    
    <td>
      <code PLAIN="">}</code>
    </td>
  </tr>
</table>