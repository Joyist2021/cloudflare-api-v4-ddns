Place at:定时任务crontab

 curl https://raw.githubusercontent.com/yulewang/cloudflare-api-v4-ddns/master/cf-v4-ddns.sh > /usr/local/bin/cf-ddns.sh && chmod +x /usr/local/bin/cf-ddns.sh
 
run `crontab -e` and add next line:运行 `crontab -e` 并添加下一行：

 */1 * * * * /usr/local/bin/cf-ddns.sh >/dev/null 2>&1
 
or you need log:或者你需要日志：

 */1 * * * * /usr/local/bin/cf-ddns.sh >> /var/log/cf-ddns.log 2>&1


Usage:

 cf-ddns.sh -k cloudflare-api-key \   # 【Global API Key】API key, 获取地址 https://www.cloudflare.com/a/account/my-account
 
            -u user@example.com \     # email@example.com 即CF的登录邮箱
            
            -h host.example.com \     # fqdn of the record you want to update  # 要更新的记录的 FQDN
            
            -z example.com \          # will show you all zones if forgot, but you need this # 如果忘记的话会显示所有区域，但你需要这个
            
            -t A|AAAA                 # specify ipv4/ipv6, default: ipv4 # 指定ipv4/ipv6，默认：ipv4
            

Optional flags:

            -f false|true \           # force dns update, disregard local stored ip #强制更新 DNS，忽略本地存储的 IP


Cloudflare API v4 Dynamic DNS Update in Bash, without unnecessary requests
Now the script also supports v6(AAAA DDNS Recoards)


换动态ip了 记录下用cf来ddns 方便自用



首先必须要有自己的域名且域名已经接入 Cloudflare （即DNS为CF提供的地址）



获取CFKEY

打开网页：https://dash.cloudflare.com/profile

在页面下方找到【Global API Key】，点击右侧的View查看Key，并保存下来

设置用于 DDNS 解析的二级域名

在 Cloudflare 中新建一个A记录，如：ddns.yourdomain.com，指向 1.1.1.1

（可随意指定，如123.123.123.123等等，主要用于后续查看 DDNS 是否生效）



下载 DDNS 脚本cf-v4-ddns.sh
wget -N --no-check-certificate https://raw.githubusercontent.com/yulewang/cloudflare-api-v4-ddns/master/cf-v4-ddns.sh


修改 DDNS 脚本并填写相关信息
您可在线使用 nano/vi/vim 等工具进行修改，也可以下载到本地进行修改再上传覆盖！

可以参考下面命令使用vi进行编辑

vi cf-v4-ddns.sh

然后按小写字母 i 进入编辑模式


---------修改参数---------

CFKEY=【Global API Key】

这里填写上一步获取的【Global API Key】API key, see https://www.cloudflare.com/a/account/my-account



CFUSER=email@example.com

登陆CF的Username, eg: [email protected](即CF的登录邮箱)



CFZONE=example.com

输入你需要解析用来DDNS解析的根域名 eg: example.com，比如我的域名是123.com，那么此处填写123.com



CFHOST=ddns.yourdomain.com

填写用来DDNS解析的二级域名，与上面设置的要一致, eg: ddns.yourdomain.com（例 ddns.123.com）

---------修改参数---------



全部填写完毕后按左上角的Esc退出编辑模式，然后输入 :wq 它会自动保存并退出

脚本授权并执行

+x cf-v4-ddns.sh
./cf-v4-ddns.sh


如果脚本相关信息填写正确，输出内容会显示当前母鸡IP，登录 Cloudflare DNS选项 查看之前设置的 1.1.1.1 已变为母鸡IP



设置定时任务

输入 crontab -e 然后会弹出 vi 编辑界面，按小写字母 i 进入编辑模式，在文件里面添加一行：

/2    * /root/cf-v4-ddns.sh >/dev/null 2>&1

如果您需要日志文件，上述代码请替换成下面代码

如果需要日志文件，输入下面命令

/2    * /root/cf-v4-ddns.sh >> /var/log/cf-ddns.log 2>&1

-natcloud

补充：crontab计算工具：

https://tool.lu/crontab

https://www.runoob.com/linux/linux-comm-crontab.html



