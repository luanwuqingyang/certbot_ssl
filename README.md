# certbot_ssl
ssl certificate automatic renewal upon expiration
# certbot ssl automatic renewal upon expiration

#### Introduction
java Certbot free wildcard SSL certificate automatic renewal（ali cdn）


#### 安装教程
​
****1.首先安装 epel-release****

 官网地址 https://certbot.eff.org/instructions?ws=apache&os=centosrhel7
```
yum install epel-release
```
****2.安装snapd****

 将 EPEL 存储库添加到您的 CentOS 安装中后，只需安装快照软件包： 
```
yum install snapd 
```
##安装后，需要启用管理主快照通信套接字的 systemd 单元：
``` 
systemctl enable --now snapd.socket 
```
##要启用经典捕捉支持，请输入以下内容以在 和 之间创建符号链 接：/var/lib/snapd/snap /snap 
```
ln -s /var/lib/snapd/snap /snap
```

****3.安装 certbot****

#安装certbot 
```
yum install certbot 

ln -s /snap/bin/certbot /usr/bin/certbot
```
****3.1阿里云dns验证插件添加（此步骤操作是为了自动域名 txt添加校验）****

****1）安装 aliyun cli 工具****
```
    wget https://aliyuncli.alicdn.com/aliyun-cli-linux-latest-amd64.tgz
    tar xzvf aliyun-cli-linux-latest-amd64.tgz
    sudo cp aliyun /usr/local/bin
    rm aliyun
```
****2）安装完成后需要配置凭证信息****
    https://help.aliyun.com/document_detail/110341.html

****3）安装 certbot-dns-aliyun 插件****
```
    wget https://cdn.jsdelivr.net/gh/justjavac/certbot-dns-aliyun@main/alidns.sh
    sudo cp alidns.sh /usr/local/bin
    sudo chmod +x /usr/local/bin/alidns.sh
    sudo ln -s /usr/local/bin/alidns.sh /usr/local/bin/alidns
    rm alidns.sh
```

****4.生成证书执行****

通过自动刷新阿里dns
测试是否能正确申请
```
certbot certonly --email xxx@xxx.com -d *.xxx.com --manual --preferred-challenges dns --manual-auth-hook "alidns" --manual-cleanup-hook "alidns clean" --dry-run
```
 正式申请时去掉 --dry-run 参数：
```
certbot certonly --email xxx@xxx.com -d *.xxx.com --manual --preferred-challenges dns --manual-auth-hook "alidns" --manual-cleanup-hook "alidns clean"
```

****5.生成后重新签发证书信息****

通过自动刷新阿里dns
```
certbot renew --manual --preferred-challenges dns --manual-auth-hook "alidns" --manual-cleanup-hook "alidns clean"
```
```
备注： 可以通过java代码进行执行替换
      1）通过定时任务去执行（5.生成后重新签发证书信息），可以达到自动刷新ssl证书

      2）涉及nginx证书更新，可以复制生成证书到相应nginx目录，并且reload 进行证书替换
```

​
