在路由器上安装及使用ShellClash的教程
 发布于 2020-09-20 |  5分钟 |  1162字数
在小米等基于openwrt内核定制的路由器原生系统上使用clash做透明代理

使用步骤：
获取路由器SSH权限
不同路由器开启SSH权限的方法不同，请使用“路由器型号+开启SSH”的关键词使用百度搜索即可获取路由器SSH权限的开启方法教程，本文只详解小米AX3600,AX1800,AX5这3款路由器的SSH开启方法。

可用于开启SSH权限的固件版本为：AX3600 1.0.17版本/AX1800 1.0.34/1.0.328/1.0.336版本/AX5 1.0.16/1.0.26版本，如果您的固件版本高于以上版本，请先执行降级操作！否则请直接跳到“开启SSH”

手动降级
降级的具体固件下载地址为：https://www.right.com.cn/forum/thread-4032490-1-1.html ，下载合适的固件到本地后，登陆路由器的管理界面：Miwifi.com ，然后在：常用设置——系统状态——升级检测中，点击手动升级按钮。之后在弹出的界面中点击选择文件——选择您刚刚下载的固件bin文件，之后点击开始上传即可。降级之前，建议在此界面下方先使用备份功能新建一份配置备份。

成功降级后，请重新设置及登陆路由器。
如需要升级到最新版本，需要使用此贴提供的方式设置永久保留SSH：https://www.right.com.cn/forum/forum.php?mod=viewthread&tid=4046020

开启SSH权限
首先登陆你的路由器管理界面，点击路由状态页签，此时地址栏应该显示如下地址：

http://miwifi.com/cgi-bin/luci/;stok=xxxxxxx****/web/home#router****

将我加粗的/web/home#router（注意不要把那一串xxxx代表的密码替换掉）替换为如下文本，之后输入回车访问，此时页面返回{“code”:0}，即可使用ssh工具测试是否开启成功。

（注意不要使用mac或iOS自带的safari浏览器）

/api/misystem/set_config_iotdev?bssid=Xiaomi&user_id=longdike&ssid=-h%3B%20nvram%20set%20ssh_en%3D1%3B%20nvram%20commit%3B%20sed%20-i%20's%2Fchannel%3D.*%2Fchannel%3D%5C%22debug%5C%22%2Fg'%20%2Fetc%2Finit.d%2Fdropbear%3B%20%2Fetc%2Finit.d%2Fdropbear%20start%3B%20echo%20-e%20'admin%5Cnadmin'%20%7C%20passwd%20root%3B

最终应该是（http://miwifi.com/cgi-bin/luci/;stok=一小串密码/api/misystem/后面一大串）这样的形式，如果不对，可能无法开启，请认真检查，不要弄错！！！

如果以上方式无法开启SSH，请参考：https://www.right.com.cn/forum/thread-4032490-1-1.html或者参考：https://www.right.com.cn/forum/thread-4039512-1-1.html 尝试更多方法开启。

登陆SSH
使用SSH连接工具来登陆SSH，推荐putty（体积最小），JuiceSSH（支持安卓手机），或其他工具，SSH工具请自行下载。

打开SSH工具，在IP（或者host）一栏填写你路由器的网关IP，即刚刚登陆路由器管理界面的IP地址或网址（例如192.168.31.1或者miwifi.com）,端口为默认的22端口不变，密码为admin（刚刚那串破解码中设定的密码），设置好之后点击连接即可登陆路由器SSH！

建议修改默认的登陆密码，在SSH界面输入passwd即可修改密码，注意修改时需要输入2次密码，且输入的密码不会显示，输入好直接回车即可完成修改。

使用一键脚本安装ShellClash
成功登陆SSH后，直接输入以下命令

sh -c "$(curl -kfsSl https://cdn.jsdelivr.net/gh/juewuy/clash-for-Miwifi@master/install.sh)" && source /etc/profile &> /dev/null
或者
sh -c "$(curl -kfsSl --resolve raw.githubusercontent.com:443:199.232.68.133 https://raw.githubusercontent.com/juewuy/ShellClash/master/install.sh)"&& source /etc/profile &> /dev/null

按照提示即可完成安装！
安装完成后，直接在SSH中使用：
clash
命令即可管理脚本！

按照脚本的提示下载clash核心及GeoIP数据库文件
之后输入1即可启动（请按照提示导入配置文件）
导入配置时，使用1的方式可以导入订阅链接或者节点分享链接
使用2的方式可以导入完整的clash配置文件，请针对自己的使用情况酌情使用

clash服务成功启动后可以通过在浏览器访问 http://clash.razord.top 或者 https://yacd.haishan.me 或者http://app.tossp.com 来管理clash内置规则以及开启直连访问
访问管理界面时需要提供的Host地址即为你的网关IP地址；端口为9999，密钥为空

问题反馈&使用交流：
https://t.me/clashfm

本文作者： 适用 Gemini,Pisces 布局,版权说明需配置
本文链接： http://juewuy.github.io/post/clash-for-miwifi-an-zhuang-ji-shi-yong-jiao-cheng/
版权声明： 本博客所有文章除特别声明外，均采用 BY-NC-SA 许可协议。转载请注明出处！
