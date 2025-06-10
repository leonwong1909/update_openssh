# update_openssh
# 手动编译更新openssh-10
> 银河麒麟V10 x86
1. 更新系统
```bash
yum install gcc make zlib-devel openssl-devel pam-devel
```
2. 下载源码包
```bash
wget https://mirrors.aliyun.com/pub/OpenBSD/OpenSSH/portable/openssh-10.0p2.tar.gz
```
3. 解压并进入
```bash
tar -zxvf openssh-10.0p2.tar.gz
cd openssh-10.0p2
```
4. 备份
```bash
mkdir -p /data/bak/20250610
cp /usr/sbin/sshd /data/bak/20250610/sshd.bak
cp -r /etc/ssh /data/bak/20250610/ssh.bak
```
5. 编译安装
```bash
./configure --prefix=/usr --sysconfdir=/etc/ssh --with-pam --with-zlib --with-ssl-dir=/usr --with-gssapi
make
make install
```
6. 验证
```bash
/usr/sbin/sshd -V
```
7. 对比配置文件
```bash
diff /etc/ssh/sshd_config /data/bak/20250610/ssh.bak/sshd_config
```
8. 修改配置文件
```bash
vim /etc/ssh/sshd_config
/etc/ssh/sshd_config line 80: Unsupported option GSSAPIAuthentication
/etc/ssh/sshd_config line 81: Unsupported option GSSAPICleanupCredentials
/etc/ssh/sshd_config line 147: Deprecated option RSAAuthentication
/etc/ssh/sshd_config line 149: Deprecated option RhostsRSAAuthentication
```
8. 重启服务
```bash
systemctl restart sshd
```
9. 查看版本
```bash
ssh -V
```
# 重启失败
```bash
systemctl restart sshd
```
1. 替换opensshserver.config配置
```bash
cd /etc/crypto-policies/back-ends
```
