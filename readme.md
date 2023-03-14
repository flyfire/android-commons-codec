在接入讯飞识物api的时候，发现demo接入了``commons-codec``，在Android端接入之后发现报错

```
Caused by: java.lang.NoSuchMethodError: No static method encodeHexString([B)Ljava/lang/String; in class Lorg/apache/commons/codec/binary/Hex; or its super classes (declaration of 'org.apache.commons.codec.binary.Hex' appears in /system/framework/ext.jar)
```

搜索了一下发现是Android系统已经内置了``commons-codec``，但是版本比较老，类加载的时候加载的是系统framework的类，导致出错。修改包名可以规避这个问题。

修改包名需要用到[jarjar](https://code.google.com/archive/p/jarjar/downloads)

``rule.txt``有三种指令，分别如下

+ rule用来取代package的名称，``rule pattern result``
+ zap用来移除复合名称的package ``zap pattern``
+ keep之后保留复合的package的名称，其他的则会删除。如果和zap一起使用，将会在zap执行完之后才执行。``keep pattern``。

``pattern``为要对比的字符串，可以使用``*``和``**``来表示任意的package名称，``*``可以表示一层的package，``**``可以用来代表多层的package。result为要取代成的字符串，可以使用``@1``，``@2``这类的符号表示要使用第几个pattern的``*``或``**``所代表的字符串。


+ [通过jarjar.jar来替换jar包名的详细介绍](https://www.cnblogs.com/yejiurui/p/4283505.html)