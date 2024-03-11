<h1 align="center">Java Object Searcher | java内存对象搜索辅助工具</h1>

<p align="center">
  <img title="portainer" src='https://img.shields.io/badge/version-0.1.0-brightgreen.svg' />
  <img title="portainer" src='https://img.shields.io/badge/java-1.7.*-yellow.svg' />
  <img title="portainer" src='https://img.shields.io/badge/license-MIT-red.svg' />
</p>


## 0x01 工具简介

```
#############################################################
   Java Object Searcher v0.01
   author: c0ny1<root@gv7.me>
   github: http://github.com/c0ny1/java-object-searcher
#############################################################
```
c0ny1师傅写的原版工具在idea中计算表达式时会卡在evaluating...

经测试，使用ArrayList进行函数传参是罪魁祸首，至于bug原理就不太清楚了，也就是下面这句：

```
SearchRequstByBFS searcher = new SearchRequstByBFS(Thread.currentThread(),keys);
```

![image-20240124231625635](https://raw.githubusercontent.com/7rovu/java-object-searcher/master/README.assets/image-20240124231625635.png)



做如下修改：

```
# 修改SearchRequstByBFS和SearchRequstByDFS构造函数
public SearchRequstByBFS(Object target){
  this.target = target;
  //把当前的元素加入到队列尾
  q.offer(new NodeT.Builder().setChain("").setField_name("TargetObject").setField_object(target).build());
}

# 添加addKey()
public void addKey(Keyword keyword){
  this.keys.add(keyword);
}
```



## 0x02 使用步骤

```
//定义黑名单
List<Blacklist> blacklists = new ArrayList<>();
blacklists.add(new Blacklist.Builder().setField_type("java.io.File").build());
//新建一个广度优先搜索Thread.currentThread()的搜索器
SearchRequstByBFS searcher = new SearchRequstByBFS(Thread.currentThread());
//设置搜索类型包含Request关键字的对象
searcher.addKey(new Keyword.Builder().setField_type("Request").build());
// 设置黑名单
searcher.setBlacklists(blacklists);
//打开调试模式,会生成log日志
searcher.setIs_debug(true);
//挖掘深度为20
searcher.setMax_search_depth(20);
//设置报告保存位置
searcher.setReport_save_path("D:\\apache-tomcat-7.0.94\\bin");
searcher.searchObject();
```

![image-20240124233339278](https://raw.githubusercontent.com/7rovu/java-object-searcher/master/README.assets/image-20240124233339278.png)
