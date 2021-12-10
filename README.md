分享 某位大佬的github脚本
https://github.com/luoxue-bot/warp_auto_change_ip

目前的warp ipv4可以解锁奈飞，但用一段时间会失效（大概1天-1周不等）
而每次执行重启命令restart wg-quick@wgcf   就会刷新它的ip
这个脚本就是每隔一段时间（可设置），检测一下解锁nf是否有效，如果失效了，不断重启warp直到刷出了可以解锁奈飞的ip

（此脚本使用warp的ipv4，但不影响ssh的正常登录）

注意：脚本需要挂在后台，可以使用screen
进入screen后运行脚本

设置：每隔多长时间检测一下ip的解锁是否有效
脚本36行            echo -e "Region: ${region} Done, monitoring..."
脚本37行            sleep 6
第37行的sleep 6可以改成任意时间，单位是秒，一般来说，1分钟即sleep 60就行，ip基本都可以存活一整天以上，不需要那么频繁的检测

（先进入screen）
wget https://github.com/luoxue-bot/warp_auto_change_ip/raw/main/warp_change_ip.sh && chmod +x warp_change_ip.sh && ./warp_change_ip.sh

如果之前没用安装过warp，首次运行脚本，根据提示"Is warp installed? [y/n] " 选择n，它会安装warp
然后再次运行脚本，选y

接下来，"Input the region you want(e.g. HK,SG):"
这个地方输入vps所在的国家名字，例如HK US SG UK
注意：一定要大写字母，小写不行
美国的小鸡不可能刷出新加坡的ip，所以填小鸡的真实位置即可

当显示“Region: XX Done, monitoring..."的时候，就说明ok了
从screen退出，运行流媒体检测脚本
bash <(curl -L -s check.unlock.media)
会看到“您的网络为: Cloudflare Warp”，ipv4可以解锁奈飞

和 "题字"(v2，trojan)    的使用相关：
先搭好 题字，确认是通的能上网。再运行这个，不需要额外的设置，直接就可以上网看奈飞了

开启warp后，ipv4的流量会被接管，这时无法申请域名的证书。停止，申请证书，再打开 即可

停止warp
systemctl stop wg-quick@wgcf
启动
systemctl start wg-quick@wgcf
重启
systemctl restart wg-quick@wgcf


screen用法：
screen -S warp   #新建名为warp的窗口
screen -r warp（只有一个窗口时，可以直接screen -r）  恢复创建的warp
退出当前窗口：在当前会话窗口中按Ctrl a +d快捷键可以实现分离
