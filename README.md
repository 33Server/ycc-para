# ycc-para 
YCC平行链搭建教程

## 前期准备
> 获取可用的已正常运行的ycc主链节点地址，grpc接口地址；<br>
> 获取与主链节点匹配的平行链节点程序包；

## 平行链部署
### (1)上传资源包
将平行链软件包上传至目标服务器，并解压ycc-para:

  * linux
    * ycc			    #节点程序
    * ycc-cli		  #命令行工具
    * ycc.toml		#配置文件
    * Makefile		#启动工具
### (2)修改配置文件
```ycc.toml
#平行链title，用来唯一标识一条平行链，修改其中的xxx
Title="user.p.xxx."

[rpc]
#避免与主链配置冲突
jrpcBindAddr="0.0.0.0:8921" #jrpc接口服务
grpcBindAddr="0.0.0.0:8922" #grpc接口服务

#平行链创世积分所在的地址，管理员地址，可以修改成自己的地址
[consensus]
genesis="0x862f23de306f0a35c122b11a6f0769570913d542"

[consensus.sub.para]
#主链的grpc地址，根据主链节点的具体情况修改
ParaRemoteGrpcClient="121.52.224.86:9902"
#验证账户，可同管理员地址
authAccount="0x862f23de306f0a35c122b11a6f0769570913d542"

[exec.sub.relay]
#同管理员地址
genesis="0x862f23de306f0a35c122b11a6f0769570913d542" 

[exec.sub.manage]
#同管理员地址
superManager=["0x862f23de306f0a35c122b11a6f0769570913d542"]
```
  * 注：管理员地址获取方式，可通过币钱包app获取，下载链接见附录；获取方式：创建钱包，取以太坊（ETH）的账户地址。
### (3)启动平行链
  `make run`
<br>或者<br>
  `nohup ./ycc -f ycc.toml > /dev/null 2>&1 &`

### (4)节点验证
```Python
#查询节点高度
./ycc-cli --rpc_laddr="http://localhost:8921" block last_header
#查询节点同步状态
./ycc-cli --rpc_laddr="http://localhost:8921"  para is_sync
```

## 导入钱包和地址
### 方式一：通过目标服务器
  * 环境：Linux
  * 目录：程序文件解压目录./ycc-para/
  * 操作：
    * ①修改配置，添加钱包助记词与账户私钥
      * vim Makefile
    * ②修改以下包含汉字的内容
      * @./ycc-cli --rpc_laddr=$(http_port) seed save -s "助记词"  -p 密码
      * @./ycc-cli --rpc_laddr=$(http_port) account import_key -k "私钥" -t 2 -l "genness"
      * @./ycc-cli --rpc_laddr=$(http_port) wallet unlock -p 密码
    * ③导入钱包与账户
      * 执行指令：make init	

### 方式二：通过其他节点
  * 环境：本地或其他具有主链或平行链节点运行的环境
  * 目录：节点程序所在目录
  * 操作(平行链jrpc地址示例：http://123.56.90.150:8921)：
    * ①	导入助记词(替换汉字)
      > ./ycc-cli --rpc_laddr="平行链jrpc地址" seed save -s "助记词"  -p 密码
    * ②	解锁钱包(替换汉字)
      > ./ycc-cli --rpc_laddr="平行链jrpc地址" wallet unlock  -p 密码
    * ③	导入私钥账户(替换汉字)
      > ./ycc-cli --rpc_laddr="平行链jrpc地址" account import_key -k "私钥" -t 2 -l "genness"
    * ④	查询
      >钱包状态：./ycc-cli --rpc_laddr="平行链jrpc地址" wallet status <br>
      >账户地址：./ycc-cli --rpc_laddr="平行链jrpc地址" account list

## 附件：资源
>[YCC平行链资源包:](https://ycc12.oss-cn-heyuan.aliyuncs.com/ycc-para.zip) 见仓库目录./installfile/<br>
>`YCC主链节点grpc地址：`121.52.224.86:9902<br>
>`币钱包下载地址:`https://d.biqianbao.net/



