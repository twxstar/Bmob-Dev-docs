## Android相关问题

### 支持同步数据上传吗

不支持阻塞主线程同步上传数据的方法！

### SDK请求时占用内存大吗

如果只是数据服务的话，占用内存非常小。如果涉及图片服务，需要视图片大小而定内存占用情况。

### 文件能不能使用批量操作

当然可以。

### 定义类名必须和表中的名一致？

类名和表名一致，表内字段名和类变量名一致。

### Bmob如何实现储存和传输图片？

通过`BmobProFile`类上传图片，上传成功之后，会返回一个`BmobFile`，你从这个`BmobFile`可以得到文件上传之后的url，把这个url保存到你的对应表中。下载的时候，先查询数据表得到url，然后下载这个图片就可以了。 

### Bmob如何将整批图片下载在本地呢？

首先先查询，得到全部数据，从而得到图片的url列表，再用一些下载文件的代码把图片批量下载下来。

### BmobFile类最多可以保存多少张图片？

`BmobFile`类只能保存一张图片，你可以用`BmobFile`或者`BmobProFile`类上传图片，得到图片的url，保存的字段用string或者array。

### 怎么让表的某个字段包含多张图片？

用array来存储文件的url

### 查询单条数据的时候，只能通过objectId来查询么？ 

如果确定是只有一个的，条件查询也可以。

### 如果不知道objectId，是否可以通过表中的元素获得数据？

添加数据的时候，`onSuccess`中可以得到objectId。也可以通过条件查询得到对应的objectId的。

### 注册和登录的流程是怎样开发的

注册成功之后，服务器会返回`sessionToken`（标识用户登录成功的会话信息）给`BmobUser`对象，这时即可立即显示登录后台的界面，同步在后台调用登录接口进行登录操作。

### 登录踢人、改密码踢人相关

一处登录其他地方下线以及改密码的问题请看如下伪代码：
![](image/pseudocode.png)

### 打开了邮箱验证功能，注册成功后未验证也能登录成功？

Bmob SDK中，邮箱的验证和用户的注册登录是异步的关系，也就是说，即使用户没有点击邮箱验证功能，也是一样可以登录成功的。如果需要限制用户的登录或者只能查看到登录后的部分功能，可以使用`BmobUser.getEmailVerified`。

### Bmob如何实现用户登录之后获取数据读写权限，以及如何实现登出操作的？

用户登录之后，我们会把获取到的用户信息保存在本地文件中，你可以通过`BmobUser.getCurrentUser`方法获取对应的值，当调用 `logout`方法之后，这些缓存的数据就会清除。如果不调用`logout`方法，下次重新打开这个应用，还是可以通过`BmobUser.getCurrentUser`方法获得上次登陆的用户信息，从而判断是否登陆过。

### 我有个Relation字段，想用它来记录喜欢这篇文章的用户，我该怎么添加里面的数据呢？

这个问题请看 [数据关联](http://docs.bmob.cn/android/developdoc/index.html?menukey=develop_doc&key=develop_android#index_%E6%95%B0%E6%8D%AE%E5%85%B3%E8%81%94) 相关文档。

### Bmob数据库的pointer和我自己使用外建字段的区别？

pointer的好处是可以在查询的时候一并把关联的记录也查询下来，不需要二次查询。让查询的速度更快


### 新版文件管理上传文件大小限制，免费用户单文件50M/每应用，但是我上传的文件是28M，却提示我超过最大限制，请问这是为什么？

web开发者后台的文件服务中，有一个 文件大小限制 的功能，你可以在上面修改的。

### 查询成功，但是list只能在onSuccess方法中使用,如何在本类中的其他地方使用？

网路请求都是异步独立线程的，你用`handler`把数据传递出来就可以。

### Bmob怎么设计赞和踩功能？

利用原子计数器   
很多应用可能会有计数器功能的需求，比如文章点赞的功能，如果大量用户并发操作，用普通的更新方法操作的话，会存在数据不一致的情况。
详情请查看[原子计数器](http://docs.bmob.cn/android/developdoc/index.html?menukey=develop_doc&key=develop_android#index_%E5%8E%9F%E5%AD%90%E8%AE%A1%E6%95%B0%E5%99%A8)。

### 手机中安装两个包含bmob push sdk的app，那么这时另一个包含bmobpush sdk的app会报错。
解决方法：
删除androidMainfest.xml中
```<permission android:protectionLevel="normal" android:name="cn.bmob.permission.push"></permission>```
这一句。

其实这一句完全可以不加，也可以正常使用，亲测2个app推送正常，且不报错，关于原因请百度android permission相关知识（如果找不到再找客服吧~）

### 为什么我修改表中的某个Number类型的字段，其他Number类型的都变为0呢？

继承自BmobObject的类不要用int类型，用Integer。

