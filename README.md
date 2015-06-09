## ngx_lua_waf

ngx_lua_waf tour is interesting, when I started going to the office to develop a web application firewall based ngx_lua.

Code is very simple, the main intention is to develop the use of simple, high-performance and lightweight.

Now open up, to comply with MIT license agreement. Which includes our filtering rules. If you have any suggestions and would like fa, and I welcome the perfect together.

### Usage:

Prevent sql injection, local contain, some overflow, fuzzing test, xss, SSRF and other web attacks
Prevent svn / backup class file leak
ApacheBench prevent attacks like stress testing tool
Shielding common hacking tools to scan, the scanner
Abnormal network requests shield
Shielding Pictures directory php execute permissions Accessories
Upload prevent webshell

### Recommended installation:

Recommended lujit2.1 do lua support

ngx_lua If it is 0.9.2 or later, we recommend regular filter function to ngx.re.find, matching efficiency will be increased by about three times.


### Usage:

nginx installation path is assumed to be :/ usr / local / nginx / conf /

The ngx_lua_waf downloaded to conf directory, unzip named waf

Http section added in nginx.conf

lua_package_path "/ usr / local / nginx / conf / waf / lua?.";
         lua_shared_dict limit 10m;
         init_by_lua_file / usr / local / nginx / conf / waf / init.lua;
     access_by_lua_file / usr / local / nginx / conf / waf / waf.lua;

Waf rules in the configuration config.lua directory (usually in waf / conf / directory)

         RulePath = "/ usr / local / nginx / conf / waf / wafconf /"

Absolute paths are subject to change, the need to modify the corresponding

Then you can restart nginx


### Configuration file Details:

     RulePath = "/ usr / local / nginx / conf / waf / wafconf /"
         - Rule store directory
         attacklog = "off"
         - Whether to open the attack information recorded, you need to configure logdir
         logdir = "/ usr / local / nginx / logs / hack /"
         - log storage directory, which requires the user to own a new, cutting the need nginx users can write permissions
         UrlDeny = "on"
         - Whether the interceptor url visit
         Redirect = "on"
         - Whether the interceptor after redirect
         CookieMatch = "on"
         - Whether the cookie blocking attacks
         postMatch = "on"
         - Whether to intercept post attack
         whiteModule = "on"
         - Whether to open the URL whitelist
         ipWhitelist = {"127.0.0.1"}
         - ip whitelist, multiple ip separated by commas
         ipBlocklist = {"1.0.0.1"}
         - ip blacklist, multiple ip separated by commas
         CCDeny = "on"
         - Whether to open blocked cc attack (the http segment increased need nginx.conf lua_shared_dict limit 10m ;)
         CCrate = "100/60"
         - Set cc attack frequency, in seconds.
         - Default 1 minute with an IP address can only request the same 100 times
         html = [[Please go away ~ ~]]
         - Warnings can be customized within the brackets
         NOTE: Do not tamper with double quotes, case sensitive

### Check whether the rules in force

Deployed can try the following command:

         curl http://xxxx/test.php?id=../etc/passwd
         Return "Please go away ~ ~" character, explained the rules take effect.

Note: By default, the machine is not in the whitelist filtering, self-configuration can be adjusted config.lua


### Renderings as follows:

! [sec] (http://i.imgur.com/wTgOcm2.png)

! [sec] (http://i.imgur.com/DqU30au.png)

### Rule update:

Taking into account the regular cache problem, dynamic rules affect performance, so the shared memory temporarily useless things like dictionaries and redis for dynamic management.

Rules update rules files can be placed into other servers via crontab tasks regularly download to update the rules, nginx reload to take effect. To protect ngx lua waf performance.

Filter log records only, not open filter, in the code in front of the check plus - Notes can be, if you need to filter, and vice versa

### Some explanations:

Filtering rules in wafconf, can be adjusted according to the needs of their own, need to wrap each rule, or use | Split

global is a global filter files, which rules on the post and get all filters
get only get request filtering rules
post is the only post request filtering rules
whitelist is a white list, which makes the filter matched to the url
user-agent is a user-agent filtering rules


Enabled by default get and post filtering, cookie filtering need to open, edit waf.lua cancel part - Notes to

Log file name format is as follows: virtual host name _sec.log


## Copyright

<table>
   <tr>
     <td> Weibo </ td> <td> magical magician </ td>
   </ tr>
   <tr>
     <td> Forum </ td> <td> http://bbs.linuxtone.org/ </ td>
   </ tr>
   <tr>
     <td> Copyright </ td> <td> Copyright (c) 2013 - loveshell </ td>
   </ tr>
   <tr>
     <td> License </ td> <td> MIT License </ td>
   </ tr>
</ table>

Thank ngx_lua module developers [@ agentzh](https://github.com/agentzh/), Chun is what I have come into contact with the spirit of open source best people





##ngx_lua_waf

ngx_lua_waf是我刚入职趣游时候开发的一个基于ngx_lua的web应用防火墙。

代码很简单，开发初衷主要是使用简单，高性能和轻量级。

现在开源出来，遵从MIT许可协议。其中包含我们的过滤规则。如果大家有什么建议和想fa，欢迎和我一起完善。

###用途：

	防止sql注入，本地包含，部分溢出，fuzzing测试，xss,SSRF等web攻击
	防止svn/备份之类文件泄漏
	防止ApacheBench之类压力测试工具的攻击
	屏蔽常见的扫描黑客工具，扫描器
	屏蔽异常的网络请求
	屏蔽图片附件类目录php执行权限
	防止webshell上传

###推荐安装:

推荐使用lujit2.1做lua支持

ngx_lua如果是0.9.2以上版本，建议正则过滤函数改为ngx.re.find，匹配效率会提高三倍左右。


###使用说明：

nginx安装路径假设为:/usr/local/nginx/conf/

把ngx_lua_waf下载到conf目录下,解压命名为waf

在nginx.conf的http段添加

		lua_package_path "/usr/local/nginx/conf/waf/?.lua";
        lua_shared_dict limit 10m;
        init_by_lua_file  /usr/local/nginx/conf/waf/init.lua;
    	access_by_lua_file /usr/local/nginx/conf/waf/waf.lua;

配置config.lua里的waf规则目录(一般在waf/conf/目录下)

        RulePath = "/usr/local/nginx/conf/waf/wafconf/"

绝对路径如有变动，需对应修改

然后重启nginx即可


###配置文件详细说明：

    	RulePath = "/usr/local/nginx/conf/waf/wafconf/"
        --规则存放目录
        attacklog = "off"
        --是否开启攻击信息记录，需要配置logdir
        logdir = "/usr/local/nginx/logs/hack/"
        --log存储目录，该目录需要用户自己新建，切需要nginx用户的可写权限
        UrlDeny="on"
        --是否拦截url访问
        Redirect="on"
        --是否拦截后重定向
        CookieMatch = "on"
        --是否拦截cookie攻击
        postMatch = "on"
        --是否拦截post攻击
        whiteModule = "on"
        --是否开启URL白名单
        ipWhitelist={"127.0.0.1"}
        --ip白名单，多个ip用逗号分隔
        ipBlocklist={"1.0.0.1"}
        --ip黑名单，多个ip用逗号分隔
        CCDeny="on"
        --是否开启拦截cc攻击(需要nginx.conf的http段增加lua_shared_dict limit 10m;)
        CCrate = "100/60"
        --设置cc攻击频率，单位为秒.
        --默认1分钟同一个IP只能请求同一个地址100次
        html=[[Please go away~~]]
        --警告内容,可在中括号内自定义
        备注:不要乱动双引号，区分大小写

###检查规则是否生效

部署完毕可以尝试如下命令：

        curl http://xxxx/test.php?id=../etc/passwd
        返回"Please go away~~"字样，说明规则生效。

注意:默认，本机在白名单不过滤，可自行调整config.lua配置


###效果图如下：

![sec](http://i.imgur.com/wTgOcm2.png)

![sec](http://i.imgur.com/DqU30au.png)

###规则更新：

考虑到正则的缓存问题，动态规则会影响性能，所以暂没用共享内存字典和redis之类东西做动态管理。

规则更新可以把规则文件放置到其他服务器，通过crontab任务定时下载来更新规则，nginx reload即可生效。以保障ngx lua waf的高性能。

只记录过滤日志，不开启过滤，在代码里在check前面加上--注释即可，如果需要过滤，反之

###一些说明：

	过滤规则在wafconf下，可根据需求自行调整，每条规则需换行,或者用|分割

		global是全局过滤文件，里面的规则对post和get都过滤
		get是只在get请求过滤的规则
		post是只在post请求过滤的规则
		whitelist是白名单，里面的url匹配到不做过滤
		user-agent是对user-agent的过滤规则


	默认开启了get和post过滤，需要开启cookie过滤的，编辑waf.lua取消部分--注释即可

	日志文件名称格式如下:虚拟主机名_sec.log


## Copyright

<table>
  <tr>
    <td>Weibo</td><td>神奇的魔法师</td>
  </tr>
  <tr>
    <td>Forum</td><td>http://bbs.linuxtone.org/</td>
  </tr>
  <tr>
    <td>Copyright</td><td>Copyright (c) 2013- loveshell</td>
  </tr>
  <tr>
    <td>License</td><td>MIT License</td>
  </tr>
</table>

感谢ngx_lua模块的开发者[@agentzh](https://github.com/agentzh/),春哥是我所接触过开源精神最好的人
