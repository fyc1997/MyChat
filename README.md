# MyChat
--MyChat压缩包中是android app的具体代码 
--Socket压缩包中是自己搭建的服务器端代码，需运行其中的Server代码，才能接收客户端请求 
--记得更换IP地址，因为考虑到可以分布式，所以每一个socket都有各自IP地址 
  Socket内readme中有Mysql数据库表的字段

实现的功能： 
1.账户注册、登录、查询修改个人信息等基本功能，可以自行增加完善，一般都是首次查询比较慢，后面都会直接通过访问手机数据获取； 
2.用户搜索、添加好友、好友列表显示功能，不能重复发起好友请求，或可修改成最近替代； 
3.聊天功能，因为服务器和数据库的关系，速度还是有些慢，所以代码中设置了比较长的时间刷新，若服务器和数据库功能强大的可以再缩短时间间隔；

一点说明：
1.账号注册：通过输入手机号、密码、手机验证码快速注册，生成的用户名字为：随便逛逛的游客；
2.账号登录：通过手机号和密码验证登陆，首次登录需要输入，否则直接从本地数据库中读取账号密码从而免密登录，注册完成也会直接登录，增强用户体验；
3.查询修改自己的个人信息：首次查询会通过服务器，速度会比较慢（学生服务器比较垃圾、MySql数据库索引也很慢），之后均直接通过本地数据库直接读取，牺牲手机内存从而高效；
4.查询用户和添加好友：根据对方手机号进行查询，可以发起好友请求，等待对方的同意拒绝，收到好友请求的人收到好友请求后可选择接收或拒绝，具体详述如下：
	查询发起好友请求：@SQL-friend:select * from friend where myUserId='"+myUserId+"' and flag="+flag+";我发起请求时会在friend表中插入一个flag为0的数据记录，我查询到对方选择后读取，flag会更改为3，对方拒绝置为4；
	接收好友请求：@SQL-friend:select * from friend where userId='"+myUserId+"' and flag=0;查询flag为0，接收人也匹配的数据；选择同意或拒绝，置flag=1同意，flag=2拒绝；
	查询对方选择：若对方同意flag=1，发起方读取对方选择后flag置3；若对方拒绝flag=2，发起方读取对方选择后flag置4；
	本地数据库操作不再描述，为什么flag这么多的原因：因为设计到发出的请求是否读取到的问题，反馈导致每一次的传输都需要更改flag标志，或增加标志位，但没必要。
