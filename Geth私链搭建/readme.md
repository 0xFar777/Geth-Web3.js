<a name="RNZqm"></a>
# 一：准备genesis.json文件
文件内容如下：
```json
{
    "config": {
        "chainId": 2008, 
        "homesteadBlock": 0,
        "eip150Block": 0,
        "eip155Block": 0,
        "eip158Block": 0,
        "byzantiumBlock": 0,
        "constantinopleBlock": 0,
        "petersburgBlock": 0,
        "istanbulBlock": 0,
        "berlinBlock": 0,
        "londonBlock": 0
    },
    "alloc": {},
    "coinbase": "0x0000000000000000000000000000000000000000",
    "difficulty": "0x20000",
    "extraData": "",
    "gasLimit": "0x2fefd8",
    "nonce": "0x0000000000000042",
    "mixhash": "0x0000000000000000000000000000000000000000000000000000000000000000",
    "parentHash": "0x0000000000000000000000000000000000000000000000000000000000000000",
    "timestamp": "0x00"
}
```

<a name="T9PSX"></a>
# 二：命令行启动（标准模式）
2.1     打开cmd（管理员模式），进入到geth所在的文件夹<br />2.2     执行命令`geth --datadir data init genesis.json`<br />![捕获1.PNG](https://cdn.nlark.com/yuque/0/2022/png/28544794/1660837411470-7df52ea3-1a74-41dd-951e-a407d475f553.png#averageHue=%2313a10e&clientId=ucf5c93d2-2478-4&from=ui&id=uf7fc0bda&originHeight=357&originWidth=966&originalType=binary&ratio=1&rotation=0&showTitle=false&size=24817&status=done&style=none&taskId=u1d889b0a-fa4e-4daa-8ff7-a4917055c72&title=)<br />————此页面代表执行成功<br />注意：此时，geth文件夹中出现了data文件夹：<br />![捕获5.PNG](https://cdn.nlark.com/yuque/0/2022/png/28544794/1661173407946-d62e32b8-5e16-4bb8-8bab-6ec994d77edf.png#averageHue=%23fbfaf8&clientId=u2024b9cf-2be4-4&from=ui&id=uc3925ee8&originHeight=173&originWidth=507&originalType=binary&ratio=1&rotation=0&showTitle=false&size=10506&status=done&style=none&taskId=u81182eeb-6273-4e2d-b258-63dbdc2d9f9&title=)<br />2.3     执行命令<br />`geth --datadir ./data --networkid 2008 --port 30303 --http --http.addr 0.0.0.0 --http.vhosts "*" --http.api "db,net,eth,web3,personal" --http.corsdomain "*" --snapshot=false --allow-insecure-unlock`<br />**四个要注意的细节：**

- "--datadir ./data"，与2.2命令结合起来看，代表要将区块的数据放入哪个文件夹，因此两处命令的文件夹名称必须一致
- networkid一定要和genesis.json中的chainId参数一致
- 与http相关的所有参数可以不写，如果不写，将无法和MetaMask等钱包进行交互（之前的版本使用的是rpc，后来改成了http）
- --allow-insecure--unlock参数可以不写，如果不写，意味着每次转账都需要先解锁账户，这个后面会再次提到

![捕获2.PNG](https://cdn.nlark.com/yuque/0/2022/png/28544794/1660837638402-b2e87720-a03e-4555-8de3-c3f968ade362.png#averageHue=%231c1c1c&clientId=ucf5c93d2-2478-4&from=ui&height=601&id=u35de5d9f&originHeight=679&originWidth=993&originalType=binary&ratio=1&rotation=0&showTitle=false&size=78363&status=done&style=none&taskId=ubfa76189-6bdb-4b40-a320-8e2b683f7d5&title=&width=879)<br />————此页面代表执行成功<br />2.4     新打开一个cmd（管理员模式），无需进入geth所在文件夹，直接执行命令：`geth attach ipc:\\.\pipe\geth.ipc`<br />特别注意：之前的那个cmd不能关闭<br />![捕获3.PNG](https://cdn.nlark.com/yuque/0/2022/png/28544794/1660838409060-0afbf0f1-bbd0-4763-90e5-f6c377496fa5.png#averageHue=%2300130c&clientId=ucf5c93d2-2478-4&from=ui&id=u59f06c8e&originHeight=220&originWidth=576&originalType=binary&ratio=1&rotation=0&showTitle=false&size=9773&status=done&style=none&taskId=u892a466e-7bda-457d-a05c-26e55539069&title=)<br />————此页面代表执行成功<br />**至此，标准模式下的Geth已启动，私链已初始化完成**

<a name="MI1gz"></a>
# 一些补充：
<a name="tWCuw"></a>
## 1. Geth关闭后，如何重启
经常有朋友把geth关了后，就不知道怎么重启了，这里统一做个说明

- 如下有两个界面：

上面这个界面称为A，下面称为B<br />![捕获3.PNG](https://cdn.nlark.com/yuque/0/2022/png/28544794/1661172994910-969c94e8-5a4b-4763-90e5-f63e7007f0a5.png#averageHue=%2300130c&clientId=u2024b9cf-2be4-4&from=ui&id=u65608928&originHeight=329&originWidth=873&originalType=binary&ratio=1&rotation=0&showTitle=false&size=16331&status=done&style=none&taskId=u2790cf01-be6f-47b6-8619-bff2ea586df&title=)<br />![捕获4.PNG](https://cdn.nlark.com/yuque/0/2022/png/28544794/1661173005328-42a2bc1b-a61a-48c9-9416-82c65ad77971.png#averageHue=%232b2b2b&clientId=u2024b9cf-2be4-4&from=ui&id=uf8d49f3b&originHeight=377&originWidth=851&originalType=binary&ratio=1&rotation=0&showTitle=false&size=37883&status=done&style=none&taskId=udcf989a9-b77a-425a-ae0d-6e609fe6e89&title=)<br />——如果仅关闭A：<br />**重启方案：**以管理员模式打开cmd，无需进入到geth文件夹，直接输入`geth attach ipc:\\.\pipe\geth.ipc`<br />——如果A与B都关了：<br />**重启方案：**以管理员模式打开cmd，进入geth文件夹，再输入：`geth --datadir ./data --networkid 2008 --port 30303 --http --http.addr 0.0.0.0 --http.vhosts "*" --http.api "db,net,eth,web3,personal" --http.corsdomain "*" --snapshot=false --allow-insecure-unlock`<br />然后，用管理员模式打开一个新的cmd，无需进入geth文件夹，直接输入`geth attach ipc:\\.\pipe\geth.ipc`<br />——如果仅关闭B：此情况不存在，因为把B关了A就不运行了，等于A也同时被关了
<a name="iNzWZ"></a>
## 



