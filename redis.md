//使用Jedis 封装 redis 的 setnx()方法

```
public boolean setNx(String key,String value,int expireTime){

  Jedis jedis = new Jedis("127.0.0.1",6379);
  String result = jedis.set(key,value,"NX","EX",expireTime);
  jedis.close();
  return "OK".equalsIgoreCase(result);
}

//测试
@Test
public void test(String key,String value,int expireTime){

  setNx("maohau","qiamo" 5);
  
}
```
