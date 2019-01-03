# 安装 fibos-centos7
方法

## 下载安装fibos
`curl -s https://fibos.io/download/installer.sh | sh`

## 测试fibos
````测试fibos命令是否可运行
[centos@ip-*-*-*-* centos]$ fibos -help
fibos: error while loading shared libraries: libssl.so.1.0.0: cannot open shared object file: No such file or directory
````

## 安装openssl-1.0.1e
````生成libssl.so.1.0.0和libcrypto.so.1.0.0
wget https://www.openssl.org/source/old/1.0.1/openssl-1.0.1e.tar.gz
tar -xvf openssl-1.0.1e.tar.gz
cd /home/centos/libssl/openssl-1.0.1e
./config shared zlib-dynamic
make
````

