`一个小白撸谷歌云300$搭建Trojan的全程记录，如果你也是小白，按文中步骤操作，你也可以的。希望能帮到大家。有写的不恰当的地方欢迎斧正。全文用到的工具和命令下载：`https://www.lanzous.com/b00zcgdgd<br>`正文中点击“回到顶部”四个字即可回到目录，点击目录也会跳到相应模块。`
* [一、申请谷歌云并创建防火墙规则](#一申请谷歌云并创建防火墙规则)<br>
  * [1、申请谷歌云](#1申请谷歌云)<br>
  * [2、创建防火墙规则](#2创建防火墙规则 )<br>
* [二、为SSH工具准备密匙](#二为SSH工具准备密匙)<br>
  * [生成密匙](#生成密匙)<br>
  * [保存密匙](#保存密匙)<br>
* [三、谷歌云创建虚拟机实例（服务器）](#三谷歌云创建虚拟机实例服务器)<br>
  * [找到创建实例](#找到创建实例)<br>
  * [创建实例过程填写](#创建实例过程填写)<br>
* [四、准备域名](#四准备域名)<br>
    * [域名购买](#域名购买)<br>
* [五、把域名解析到谷歌云服务器地址](#五把域名解析到谷歌云服务器地址)<br>
  * [更改域名解析服务器](#更改域名解析服务器)<br>
  * [把域名解析到谷歌云实例](#把域名解析到谷歌云实例)<br>
  * [测试是否解析成功](#测试是否解析成功)<br>
* [六、利用SSH工具连接谷歌云实例](#六利用SSH工具连接谷歌云实例)<br>
  * [创建对话，设置连接和用户身份验证](#创建对话设置连接和用户身份验证)<br>
* [七、部署BBR加速和Trojan](#七部署bbr加速和trojan)<br>    
  * [部署BBR加速](#部署bbr加速)<br>
  * [部署Trojan](#部署trojan)<br>
* [八、福利](#八福利)<br>
  * [Windows端只支持Trojan的图形界面工具](#给大家一个windows端只支持trojan的图形界面工具)<br>
  * [vultr新户充10$送100$活动](#vultr新户充10送100活动)
# 一、申请谷歌云并创建防火墙规则
## 1、申请谷歌云
* 打开[谷歌云官网](https://cloud.google.com/)，点免费开始试用，选择一个国家，我这里以日文为例，这里选择哪个国家都可以。如图
![谷歌云申请300$](https://www.louimg.com/u/20200325/17012694.png "谷歌云申请300$")
![谷歌云申请300$](https://www.louimg.com/u/20200325/17012963.png "谷歌云申请300$")
<br>[回到顶部](#readme)
* 接下来填写信息，信息完成后，点击开始免费试用，具体见下图：<br>
  * 账号类型选择个人；<br>
  * 不管选择哪个国家，这里的地址信息一定写真实的地址，我是用谷歌搜了一个大阪的具体地方，网上有很多地址生成器也可以用。姓名随便写；<br>
  * 信用卡信息全部写自己的真实信息，持卡人姓名写自己真实姓名；<br>
![谷歌云申请300$](https://www.louimg.com/u/20200325/17112558.png "谷歌云申请300$")
![谷歌云申请300$](https://www.louimg.com/u/20200325/17112824.png "谷歌云申请300$")
<br>[回到顶部](#readme)
* 申请成功后，直接进入到控制台页面。Google会从信用卡扣除1美元用于验证, 验证完成后会退还到信用卡中，我因为选的国家是日本，所以按等额的日元的扣的。具体如下图。那个验证费返还的特别快，我在收到扣费信息后，立马给银行打了电话，工作人员说后台显示已经冲账了，相当于没有刷卡扣钱。<br>
![谷歌云申请300$](https://www.louimg.com/u/20200325/17334963.png "谷歌云申请300$")
![谷歌云申请300$](https://www.louimg.com/u/20200325/1733525.jpg "谷歌云申请300$")
<br>[回到顶部](#readme)
## 2、创建防火墙规则
进入谷歌云后台，按照下图路劲找到防火墙规则，点击创建防火墙规则，需要填写的地方已用红圈圈出，如下图。<br>
![创建防火墙规则](https://www.louimg.com/u/20200325/18255456.png "创建防火墙规则")
* 注意来源IP地址范围是0.0.0.0/0
* 目标选为网络中所有实例
* 协议和端口选择全部允许
![创建防火墙规则](https://www.louimg.com/u/20200325/18255798.png "创建防火墙规则")
<br>[回到顶部](#readme)
# 二、为SSH工具准备密匙
## 生成密匙
打开puttygen.exe([官网](https://www.puttygen.com/)), 直接点generate, 在下图红圈标注的空白区域不断移动鼠标直到生成密匙<br>
![生成SSH密匙](https://www.louimg.com/u/20200325/19150462.png "生成SSH密匙")
![生成SSH密匙](https://www.louimg.com/u/20200325/19233220.png "生成SSH密匙")
![生成SSH密匙](https://www.louimg.com/u/20200325/21091173.png "生成SSH密匙")
<br>[回到顶部](#readme)
## 保存密匙
点击Conversions->Export Open SSH Key把密匙文件保存到电脑，后边使用SSH工具连接谷歌云服务器时会用到。
![密匙上传](https://www.louimg.com/u/20200325/21193488.png "密匙上传")
<br>[回到顶部](#readme)
# 三、谷歌云创建虚拟机实例（服务器）
## 找到创建实例
* 导航栏->computer engine->虚拟机实例，点击创建实例，如下图<br>
![实例路劲](https://www.louimg.com/u/20200325/21452018.png "实例路劲")
<br>[回到顶部](#readme)
## 创建实例过程填写
`我把创建实例过程中需要填写修改的地方全部标注出来`<br>
* 名称: 随便填<br>
* 地区: 建议选asia-east1-c，asia-east1-a, asia-east1-b, asia-east1-c 机房都在中国台湾彰化县, 实测c区更好。<br>
* 区域：一般选择物理距离离你最近的
* 机器配置: 选共享核心(一个共享vCPU)，614M内存, 一般加速上网, 看视频, 玩游戏都够用了, 看你自己需求<br>
![实例填写](https://www.louimg.com/u/20200325/22423131.png "实例填写")
<br>[回到顶部](#readme)
* 启动磁盘: 推荐选CentOS 8，本文命令都是基于CentOS 8, 如选Debian, Ubuntu等其他系统, 命令会稍有不同, 自行解决。<br>
* 勾选HTTP和HTTPS<br>
![实例填写](https://www.louimg.com/u/20200325/23151022.png "实例填写")
<br>[回到顶部](#readme)
* 添加第二步中生成的SSH密匙
![实例填写](https://www.louimg.com/u/20200325/2321322.png "实例填写")
<br>[回到顶部](#readme)
* 创建静态IP
![实例填写](https://www.louimg.com/u/20200325/23275862.png "实例填写")
<br>[回到顶部](#readme)
* 以上全部完成后，点击创建<br>
![实例填写](https://www.louimg.com/u/20200325/23330175.png "实例填写")
<br>[回到顶部](#readme)
* 创建好的实例如图所示
![实例填写](https://www.louimg.com/u/20200325/23414747.png "实例填写")
<br>[回到顶部](#readme)
# 四、准备域名
## 域名购买
* 这里以购买域名为例，免费的域名网上很多，比如可到Freenom申请。不过还是建议购买一个域名，因为综合来说更稳定，况且现在的域名价格也不是很贵。[免费跟付费的域名有什么区别，请点击查看](https://blog.csdn.net/liangtaox8/article/details/96839227)。我是在大名鼎鼎的域名提供商godaddy([官网](https://www.godaddy.com/))上购买的。当初是准备建设一个自己的博客而买的。当然越常见的域名后缀价格越高，比如.com、.net、.cn、.org等这些就属于常见的域名。我的购买账单如图：<br>
![godaddy账单](https://www.louimg.com/u/20200322/19103367.png "godaddy账单")
<br>[回到顶部](#readme)
* 大致讲一下购买步骤：打开官网，输入你想要的域名，比如我输入了sxcool1024，点击搜索，就会列出所有域名，根据自己需求选择，加入购物车，购买。具体如下图<br>
![godaddy购买](https://www.louimg.com/u/20200325/15270758.png "godaddy购买")
![godaddy购买](https://www.louimg.com/u/20200325/15271181.png "godaddy购买")
<br>[回到顶部](#readme)
![godaddy购买](https://www.louimg.com/u/20200325/15394149.png "godaddy购买")
![godaddy购买](https://www.louimg.com/u/20200325/15394328.png "godaddy购买")
<br>[回到顶部](#readme)
# 五、把域名解析到谷歌云服务器地址
## 更改域名解析服务器
* 1、这里推荐用DNSPOD([官网](https://www.dnspod.cn))，这是一个提供免费域名解析服务的网站，请自行注册，注册完成后，进入控制台，找到DNS管理->我的域名，添加域名。添加完成后，点击域名，默认里边有两条记录。如下图
![域名解析](https://www.louimg.com/u/20200326/00292795.png "域名解析")
![域名解析](https://www.louimg.com/u/20200326/00201698.png "域名解析")
<br>[回到顶部](#readme)
* 2、进入购买域名的网站godaddy，找到我的产品，点击DNS，找到域名服务器，点击更改，把上一步在DNSPOD里添加的域名对应的两条默认记录的记录值复制到godaddy的自定义域名服务器处，这样我的域名(sxcool1024.com)解析服务器就变为DNSPOD。如下图
![域名解析](https://www.louimg.com/u/20200326/00130082.png "域名解析")
![域名解析](https://www.louimg.com/u/20200326/00130180.png "域名解析")
<br>[回到顶部](#readme)
## 把域名解析到谷歌云实例
* DNSPOD添加域名后，域名里默认有两条记录，我们还需手动添加另外两条记录，手动添加的两条记录的记录值就是谷歌云创建的实例的外部IP,其它参数参照下图填写，如图所示
![域名解析](https://www.louimg.com/u/20200326/00452451.png "域名解析")
<br>[回到顶部](#readme)
## 测试是否解析成功
* ping一下你的域名，看域名是否成功解析到你创建的谷歌云实例，注意，如果你的域名是免费的，等大概半小时以后再去ping，因为免费域名解析需要的时间长，如果是付费的域名，可直接操作这一步，不用等待。下图所示的就是成功解析。
![域名解析](https://www.louimg.com/u/20200326/13380756.png "域名解析")
<br>[回到顶部](#readme)
# 六、利用SSH工具连接谷歌云实例
## 创建对话，设置连接和用户身份验证
* 这里用到的工具是XShell6，打开XShell6点击文件->新建对话，依次设置连接和用户身份验证，具体如下图
![SSH连接](https://www.louimg.com/u/20200326/13590532.png "SSH连接")
![SSH连接](https://www.louimg.com/u/20200326/13590687.png "SSH连接")
<br>[回到顶部](#readme)
* 连接成功后如图所示，连接成功后才可以进行接下来的操作<br>
sxszcoollm是当前登录的用户名，你的跟我的不一样<br>
instance-1是实例名<br>
~是当前工作路径<br>
最后都是以 $ 或者 # 号结束，普通用户以 $ 号结束，只有root用户以 # 结束。
![SSH连接成功](https://www.louimg.com/u/20200326/14111468.png "SSH连接成功")
<br>[回到顶部](#readme)
# 七、部署BBR加速和Trojan
## 部署BBR加速
* Linux系统中root用户拥有最高权限, 我们需要先切换到root用户, 否则运行某些命令时会提示无权限。Xshell连上服务器后, 先要切换到root用户。输入命令:
sudo -i，切换成功后，如下图
![切换root](https://www.louimg.com/u/20200326/14470342.png "切换root")
* 依次运行以下4条命令
  * 安装wget
    * 命令：`yum install -y wget`，运行如下图![安装wget](https://www.louimg.com/u/20200326/15022413.png "安装wget")
  * 下载shell脚本
    * 命令：`wget --no-check-certificate https://github.com/teddysun/across/raw/master/bbr.sh`，运行如下图
    ![下载shell](https://www.louimg.com/u/20200326/15094069.png "下载shell")
  * 给脚本赋予执行权限
    * 命令：`chmod +x bbr.sh`
  * 执行脚本
    * 命令：`./bbr.sh`，运行如下图<br>![执行脚本](https://www.louimg.com/u/20200326/15253383.png "执行脚本")
<br>[回到顶部](#readme)
## 部署Trojan
* 执行以下命令一键部署，运行如下图<br>
`curl -O https://raw.githubusercontent.com/atrandys/trojan/master/trojan_mult.sh && chmod +x trojan_mult.sh && ./trojan_mult.sh`<br>
![执行脚本](https://www.louimg.com/u/20200326/15302431.png "执行脚本")
* 选择数字1，开始安装Trojan，安装过程中要输入绑定到本VPS的域名，如下图(动态图)
![执行脚本](https://www.louimg.com/u/20200326/15385460.gif "执行脚本")
<br>[回到顶部](#readme)
* 输入我的域名sxcool1024l.com，你自己输入你的域名，最后结果如图，复制红圈中的链接在浏览器打开，下载Trojan文件压缩包，如下图
![执行脚本](https://www.louimg.com/u/20200326/15582494.png "执行脚本")
* 打开下载的Trojan压缩包，此时你直接双击trojan.exe即可运行Trojan实现翻墙，但是它是一个命令行黑界面，看着很不舒服。打开config.json文件可查看Trojan的服务器地址，端口、密码等信息，方便我们添加节点到其它科学上网工具。具体如下图
![执行脚本](https://www.louimg.com/u/20200326/17062580.png "执行脚本")
<br>[回到顶部](#readme)
***
**到这里，我们的谷歌云搭建Trojan全部完成**
***
# 八、福利
## 给大家一个Windows端只支持Trojan的图形界面工具
![福利](https://www.louimg.com/u/20200326/1809222.png "福利")
## vultr新户充10$送100$活动
* 著名云服务器商vultr也在搞新户充10$送100$活动。而且关注它的推特还会送3$，我自己充了10$，加上送的一共113$，够玩一阵子了。[vultr活动注册](https://www.vultr.com/?ref=8477410)<br>
* [vultr16个机房测速工具](https://www.lanzous.com/iawn5je)<br>
**欢迎加入电报群一起交流： https://t.me/sxcool1024g**
<br>[回到顶部](#readme)

