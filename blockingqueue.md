# BlockingQueue常用操作

方法类型|抛出异常|特殊值|阻赛|超时|其他
--|:--:|--:
插入|add(e)|offer(e)|put(e)|offer(e,time,timeUnit)|无
移除|remove(e) Y|poll(e) Y|take(e) Y|poll(e,time,timrUnit) Y|drainTo(e) Y
检查|element() N|peek() N|无|无|contain(e) N

- 是否会移除队列中的元素：Y:会，N:不会
