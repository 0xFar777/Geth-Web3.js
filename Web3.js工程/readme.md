<a name="SxJuT"></a>
# 前言：Solidity+Web3.js = Dapp，Geth=测试环境
Solidity：提供智能合约，即核心的业务逻辑<br />web3.js：与区块链上的智能合约进行交互（读写数据），并可通过编写前端代码（Javascript/Typescript，html，css）设计交互网页（用户页面）的工具<br />Geth：提供一条测试链，进行合约功能测试的工具

——现在我们来在Web3.js上部署合约并实现交互（调用/修改）吧
<a name="RpZEP"></a>
# 一：需要准备的东西
<a name="d3qNU"></a>
## 1.1  安装node.js（以win10）为例
<a name="nBm62"></a>
### 1.1.1 下载node.js

-  进入node.js官网：

[下载 | Node.js](https://nodejs.org/zh-cn/download/)

- 选择**"长期维护版本"**的window安装包

![捕获8.PNG](https://cdn.nlark.com/yuque/0/2022/png/28544794/1661174812108-8e086a08-4077-425d-a853-29ca53d2ae80.png#averageHue=%23b5cfad&clientId=ua3557398-4c52-4&from=ui&id=u58cfdda9&originHeight=324&originWidth=592&originalType=binary&ratio=1&rotation=0&showTitle=false&size=30048&status=done&style=none&taskId=ue845007a-3511-4508-bfb5-2ebb8ffc6c7&title=)

- 下载完后，点击安装，**设置安装路径**，然后下面的这个要勾上：<br />![捕获9.PNG](https://cdn.nlark.com/yuque/0/2022/png/28544794/1661175036985-17c739f1-7179-472c-b136-e9e189af6718.png#averageHue=%23edece9&clientId=ua3557398-4c52-4&from=ui&id=u1af5963d&originHeight=261&originWidth=491&originalType=binary&ratio=1&rotation=0&showTitle=false&size=18581&status=done&style=none&taskId=u7f2c3281-da13-49b3-a6cd-da1db4c5153&title=)
- 检查是否安装成功：打开cmd，分别执行：`node -v`和`npm -v`，只要能顺利获取到版本，意味着安装成功

![捕获10.PNG](https://cdn.nlark.com/yuque/0/2022/png/28544794/1661175238054-905d21c3-c005-44dc-a822-f84f28f25e4b.png#averageHue=%2300130c&clientId=ua3557398-4c52-4&from=ui&id=u83262c88&originHeight=136&originWidth=371&originalType=binary&ratio=1&rotation=0&showTitle=false&size=4052&status=done&style=none&taskId=uaeff1c87-bc33-4be5-9790-2fe45effedf&title=)
<a name="kejuC"></a>
### 1.1.2 配置node.js环境

- 管理员模式打开一个全新的cmd
- 进入到自己node.js的安装目录，即：`cd 安装目录`
- 创建两个文件夹，分别是 node_global、node_cache，表示node.js的全局路径、缓存路径 ，执行：`mkdir node_global`和`mkdir node_cache`
- 启用刚创建的两个路径：

`npm config set prefix "安装路径\node_global"`<br />`npm config set prefix "安装路径\node_cache"`

- 左下角直接搜索env，或者右键打开"我的电脑"中的属性，进入到高级环境变量
- 新建一个"系统环境"变量：NODE_PATH<br />变量名：NODE_PATH<br />变量值：安装路径\node_global\node_modules  

![捕获15.PNG](https://cdn.nlark.com/yuque/0/2022/png/28544794/1661175965722-d4c4c2fd-8a0d-4856-93e2-9fa96b571bd4.png#averageHue=%23998360&clientId=ua3557398-4c52-4&from=ui&id=u7a1397de&originHeight=603&originWidth=1789&originalType=binary&ratio=1&rotation=0&showTitle=false&size=414601&status=done&style=none&taskId=u6494b893-009f-4f68-94d1-0ebacee6a52&title=)

- 在系统变量Path里添加%NODE_PATH%  

![捕获20.PNG](https://cdn.nlark.com/yuque/0/2022/png/28544794/1661176112558-7fdfd655-9789-4866-99df-aca9e4a9bc66.png#averageHue=%23f2efee&clientId=ua3557398-4c52-4&from=ui&id=u3b5b456a&originHeight=649&originWidth=1133&originalType=binary&ratio=1&rotation=0&showTitle=false&size=84606&status=done&style=none&taskId=u25dead74-c836-440c-a3f3-91331dd6402&title=)

-  将安装路径\node_global添加到"用户变量"的Path里

![捕获36.PNG](https://cdn.nlark.com/yuque/0/2022/png/28544794/1661176294624-3e8b2d11-91a2-4856-9ff9-4c1709ef90b3.png#averageHue=%23f2f1ef&clientId=ua3557398-4c52-4&from=ui&id=u41df5cc3&originHeight=602&originWidth=1152&originalType=binary&ratio=1&rotation=0&showTitle=false&size=99355&status=done&style=none&taskId=u704efa82-b0bd-4f8e-9b45-c195da298cd&title=)
<a name="ToSzP"></a>
### 1.1.3 设置镜像源

- 管理员模式打开一个新的cmd，执行`npm config set registry https://registry.npm.taobao.org`
- 查看镜像源：`npm config get registry`

<a name="GBiNz"></a>
## 1.2  合约的ABI文件和Web3Deploy文件

- 打开Remix，随便找个合约部署
- 部署成功后找到Compilation Details

![捕获39.PNG](https://cdn.nlark.com/yuque/0/2022/png/28544794/1661177170633-60275cf3-b442-401c-aaa6-7ab45e11e100.png#averageHue=%233d4056&clientId=ua3557398-4c52-4&from=ui&id=u81a294ba&originHeight=267&originWidth=359&originalType=binary&ratio=1&rotation=0&showTitle=false&size=17147&status=done&style=none&taskId=u9a086ccb-5d30-45d7-8b48-da0a7481e92&title=)

- 复制ABI和WEB3DEPLOY并分别保存，后面有用

![捕获40.PNG](https://cdn.nlark.com/yuque/0/2022/png/28544794/1661177187695-6db8df69-5e3b-443b-beda-669f5fa2255c.png#averageHue=%23242538&clientId=ua3557398-4c52-4&from=ui&id=u70deaefc&originHeight=314&originWidth=417&originalType=binary&ratio=1&rotation=0&showTitle=false&size=13065&status=done&style=none&taskId=uf982933a-4028-4563-8546-c7ea52b89a1&title=)

<a name="KblU2"></a>
## 1.3  Geth搭建一条测试链

- 提前准备好两个账户
- 提前挖点自己搭建的链的测试币

<a name="XolOm"></a>
# 二：Web3.js上部署合约
<a name="YVGWp"></a>
## 2.1 新建并启动Web3.js工程

- **管理员模式**新打开一个cmd，执行：`mkdir Web3Project`，即创建一个名为"Web3Project"的文件夹

![捕获66.PNG](https://cdn.nlark.com/yuque/0/2022/png/28544794/1661178449919-92f29f0a-1203-488a-9246-757550f64484.png#averageHue=%2300130c&clientId=ua3557398-4c52-4&from=ui&id=u2447ca29&originHeight=86&originWidth=377&originalType=binary&ratio=1&rotation=0&showTitle=false&size=4163&status=done&style=none&taskId=u68ceea9a-a2f7-4695-9ccc-76f06d68dd3&title=)

- **管理员模式**打开Vscode，打开刚刚创建的文件夹

（路径为：C:/Windows/system32/Web3Project）

- Vscode内打开一个终端，执行`npm install web3`命令

![捕获70.PNG](https://cdn.nlark.com/yuque/0/2022/png/28544794/1661179161054-88622039-099a-4123-b8b8-4da0c0d7b5b8.png#averageHue=%23242322&clientId=ua3557398-4c52-4&from=ui&height=142&id=uf9338c6a&originHeight=170&originWidth=719&originalType=binary&ratio=1&rotation=0&showTitle=false&size=26787&status=done&style=none&taskId=u5c3bcde3-b0fc-4292-b00c-8449caafbba&title=&width=599)

- 新建一个名为"deploy.js"的文件，并将以下三行代码写进去：

`var Web3 = require('web3');`<br />（文件当前有web3对象吗？没有就创建一个）<br />`var web3 = new Web3(new Web3.providers.HttpProvider('http://localhost:8545'));`<br />（将web3对象连接到“http://localhost:8545”）<br />`web3.eth.getAccounts().then(console.log);`<br />（如果上面的...8545连接成功，会将在Geth中搭建的测试链中的所有地址打印在终端）
```javascript
var Web3 = require('web3');
var web3 = new Web3(new Web3.providers.HttpProvider('http://localhost:8545'));
web3.eth.getAccounts().then(console.log);
```

- 在Vscode的终端执行命令：`node deploy.js`

——会发现将自己Geth搭建的测试链中的所有地址打印出了出来（当然，前提是这条测试链中你已经创建了至少一个地址）<br />![捕获99.PNG](https://cdn.nlark.com/yuque/0/2022/png/28544794/1661179609942-de8ed0fc-5e76-4e9e-a08a-4140d5aa9cf2.png#averageHue=%23222120&clientId=ua3557398-4c52-4&from=ui&id=u66f49034&originHeight=153&originWidth=671&originalType=binary&ratio=1&rotation=0&showTitle=false&size=33507&status=done&style=none&taskId=u9079750a-5c33-4cc2-9c93-f5a4e2c8a70&title=)<br />——注意，必须要先启用这条私链，才可以执行上述命令，否则会报错：<br />![捕获102.PNG](https://cdn.nlark.com/yuque/0/2022/png/28544794/1661179979173-e51e80da-cd3f-430e-b3fa-fd17fe5c0b13.png#averageHue=%23292826&clientId=ua3557398-4c52-4&from=ui&id=u462f0377&originHeight=302&originWidth=940&originalType=binary&ratio=1&rotation=0&showTitle=false&size=106668&status=done&style=none&taskId=u0ffc0792-701a-468a-963c-854ed3a2994&title=)

<a name="rCmax"></a>
## 2.2  合约部署
——在deploy.js上继续编写代码：

- 将之前复制的合约的WEB3DEPLOY文件（顾名思义，就是WEB3部署文件）粘贴到deploy.js（注意，前面三行不要删）

——这个我的某个合约的WEB3DEPLOY文件（修改后的）<br />![捕获206.PNG](https://cdn.nlark.com/yuque/0/2022/png/28544794/1661183245552-9cec350e-1287-48f9-853b-da18df991363.png#averageHue=%23252120&clientId=ua3557398-4c52-4&from=ui&id=btizR&originHeight=534&originWidth=1016&originalType=binary&ratio=1&rotation=0&showTitle=false&size=185889&status=done&style=none&taskId=u0a601109-e622-48db-beb0-c7f9f62bb0c&title=)

- 只需改动两个地方，①：from中的地址要改成Geth搭建的测试链的第一个地址的具体地址**（不能表达成web3.eth.accounts[0]）**；②：WEB3DEPLOY文件复制过来后，要在末尾加上图示矩形中这两行，**否则合约在Web3.js上部署后拿不到合约地址**
- data中的数据就是合约的bytecode字节码，粘进去一般会报错，需要修改Vscode中的一个参数：

![捕获130.PNG](https://cdn.nlark.com/yuque/0/2022/png/28544794/1661181047668-b66a37bf-f323-455e-ae02-43a2307bb1a1.png#averageHue=%23c0ac7b&clientId=ua3557398-4c52-4&from=ui&height=180&id=u18264ac9&originHeight=217&originWidth=782&originalType=binary&ratio=1&rotation=0&showTitle=false&size=43190&status=done&style=none&taskId=u402c42fa-0244-4096-8757-5b023cc0592&title=&width=649)<br />——修改方法：打开vscode的设置，输入“editor.maxTokenizationLineLength”参数，并将其修改为"1e+36"![捕获140.PNG](https://cdn.nlark.com/yuque/0/2022/png/28544794/1661181144324-8d786dd8-ddab-494d-8f9b-a92f1b889270.png#averageHue=%23282726&clientId=ua3557398-4c52-4&from=ui&height=269&id=u7deb0962&originHeight=337&originWidth=863&originalType=binary&ratio=1&rotation=0&showTitle=false&size=35555&status=done&style=none&taskId=u2dad0d06-896f-47d2-8d46-bdb959b124a&title=&width=688)

- 如果合约包含构造参数，就改动代码的"arguments"参数：

![捕获200.PNG](https://cdn.nlark.com/yuque/0/2022/png/28544794/1661182412480-b83b60cc-5a2a-43c9-b7b5-5869d49ce334.png#averageHue=%23252120&clientId=ua3557398-4c52-4&from=ui&id=u9afb68c7&originHeight=521&originWidth=912&originalType=binary&ratio=1&rotation=0&showTitle=false&size=96045&status=done&style=none&taskId=u8f4ed942-a079-4585-9a91-6a2dc700abf&title=)<br />———假设某份合约里定义了：合约创建时要传入两个构造参数，分别是name（代币名）和totalSupply（代币总供应量），那么应先在deploy.js文件的最上面定义这两个变量，然后才在arguments中写入两个变量的值<br />**————"deploy.js"完整代码：**
```javascript
var _name;
var _totalSupply;
var Web3 = require('web3');
var web3 = new Web3(new Web3.providers.HttpProvider('http://localhost:8545'));
web3.eth.getAccounts().then(console.log);
var fabi01Contract = new web3.eth.Contract([{ "inputs": [{ "internalType": "string", "name": "_name", "type": "string" }, { "internalType": "uint256", "name": "_totalSupply", "type": "uint256" }], "stateMutability": "nonpayable", "type": "constructor" }, { "anonymous": false, "inputs": [{ "indexed": true, "internalType": "address", "name": "from", "type": "address" }, { "indexed": true, "internalType": "address", "name": "to", "type": "address" }, { "indexed": false, "internalType": "uint256", "name": "tokens", "type": "uint256" }], "name": "Transfer", "type": "event" }, { "inputs": [{ "internalType": "address", "name": "_address", "type": "address" }], "name": "balanceOf", "outputs": [{ "internalType": "uint256", "name": "", "type": "uint256" }], "stateMutability": "view", "type": "function" }, { "inputs": [], "name": "decimals", "outputs": [{ "internalType": "uint256", "name": "", "type": "uint256" }], "stateMutability": "view", "type": "function" }, { "inputs": [], "name": "name", "outputs": [{ "internalType": "string", "name": "", "type": "string" }], "stateMutability": "view", "type": "function" }, { "inputs": [], "name": "totalSupply", "outputs": [{ "internalType": "uint256", "name": "", "type": "uint256" }], "stateMutability": "view", "type": "function" }, { "inputs": [{ "internalType": "address", "name": "_receiver", "type": "address" }, { "internalType": "uint256", "name": "amount", "type": "uint256" }], "name": "transfer", "outputs": [{ "internalType": "bool", "name": "success", "type": "bool" }], "stateMutability": "nonpayable", "type": "function" }]);
var fabi01 = fabi01Contract.deploy({
    data: '0x60806040523480156200001157600080fd5b506040516200108f3803806200108f8339818101604052810190620000379190620002f9565b8160009081620000489190620005a0565b506012600281905550600254600a6200006291906200080a565b816200006f91906200085b565b60018190555080600360003373ffffffffffffffffffffffffffffffffffffffff1673ffffffffffffffffffffffffffffffffffffffff168152602001908152602001600020819055503373ffffffffffffffffffffffffffffffffffffffff16600073ffffffffffffffffffffffffffffffffffffffff167fddf252ad1be2c89b69c2b068fc378daa952ba7f163c4a11628f55a4df523b3ef6001546040516200011b9190620008cd565b60405180910390a35050620008ea565b6000604051905090565b600080fd5b600080fd5b600080fd5b600080fd5b6000601f19601f8301169050919050565b7f4e487b7100000000000000000000000000000000000000000000000000000000600052604160045260246000fd5b620001948262000149565b810181811067ffffffffffffffff82111715620001b657620001b56200015a565b5b80604052505050565b6000620001cb6200012b565b9050620001d9828262000189565b919050565b600067ffffffffffffffff821115620001fc57620001fb6200015a565b5b620002078262000149565b9050602081019050919050565b60005b838110156200023457808201518184015260208101905062000217565b60008484015250505050565b6000620002576200025184620001de565b620001bf565b90508281526020810184848401111562000276576200027562000144565b5b6200028384828562000214565b509392505050565b600082601f830112620002a357620002a26200013f565b5b8151620002b584826020860162000240565b91505092915050565b6000819050919050565b620002d381620002be565b8114620002df57600080fd5b50565b600081519050620002f381620002c8565b92915050565b6000806040838503121562000313576200031262000135565b5b600083015167ffffffffffffffff8111156200033457620003336200013a565b5b62000342858286016200028b565b92505060206200035585828601620002e2565b9150509250929050565b600081519050919050565b7f4e487b7100000000000000000000000000000000000000000000000000000000600052602260045260246000fd5b60006002820490506001821680620003b257607f821691505b602082108103620003c857620003c76200036a565b5b50919050565b60008190508160005260206000209050919050565b60006020601f8301049050919050565b600082821b905092915050565b600060088302620004327fffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffff82620003f3565b6200043e8683620003f3565b95508019841693508086168417925050509392505050565b6000819050919050565b6000620004816200047b6200047584620002be565b62000456565b620002be565b9050919050565b6000819050919050565b6200049d8362000460565b620004b5620004ac8262000488565b84845462000400565b825550505050565b600090565b620004cc620004bd565b620004d981848462000492565b505050565b5b818110156200050157620004f5600082620004c2565b600181019050620004df565b5050565b601f82111562000550576200051a81620003ce565b6200052584620003e3565b8101602085101562000535578190505b6200054d6200054485620003e3565b830182620004de565b50505b505050565b600082821c905092915050565b6000620005756000198460080262000555565b1980831691505092915050565b600062000590838362000562565b9150826002028217905092915050565b620005ab826200035f565b67ffffffffffffffff811115620005c757620005c66200015a565b5b620005d3825462000399565b620005e082828562000505565b600060209050601f83116001811462000618576000841562000603578287015190505b6200060f858262000582565b8655506200067f565b601f1984166200062886620003ce565b60005b8281101562000652578489015182556001820191506020850194506020810190506200062b565b868310156200067257848901516200066e601f89168262000562565b8355505b6001600288020188555050505b505050505050565b7f4e487b7100000000000000000000000000000000000000000000000000000000600052601160045260246000fd5b60008160011c9050919050565b6000808291508390505b60018511156200071557808604811115620006ed57620006ec62000687565b5b6001851615620006fd5780820291505b80810290506200070d85620006b6565b9450620006cd565b94509492505050565b60008262000730576001905062000803565b8162000740576000905062000803565b816001811462000759576002811462000764576200079a565b600191505062000803565b60ff84111562000779576200077862000687565b5b8360020a91508482111562000793576200079262000687565b5b5062000803565b5060208310610133831016604e8410600b8410161715620007d45782820a905083811115620007ce57620007cd62000687565b5b62000803565b620007e38484846001620006c3565b92509050818404811115620007fd57620007fc62000687565b5b81810290505b9392505050565b60006200081782620002be565b91506200082483620002be565b9250620008537fffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffff84846200071e565b905092915050565b60006200086882620002be565b91506200087583620002be565b9250817fffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffff0483118215151615620008b157620008b062000687565b5b828202905092915050565b620008c781620002be565b82525050565b6000602082019050620008e46000830184620008bc565b92915050565b61079580620008fa6000396000f3fe608060405234801561001057600080fd5b50600436106100575760003560e01c806306fdde031461005c57806318160ddd1461007a578063313ce5671461009857806370a08231146100b6578063a9059cbb146100e6575b600080fd5b610064610116565b6040516100719190610474565b60405180910390f35b6100826101a4565b60405161008f91906104af565b60405180910390f35b6100a06101aa565b6040516100ad91906104af565b60405180910390f35b6100d060048036038101906100cb919061052d565b6101b0565b6040516100dd91906104af565b60405180910390f35b61010060048036038101906100fb9190610586565b6101f9565b60405161010d91906105e1565b60405180910390f35b600080546101239061062b565b80601f016020809104026020016040519081016040528092919081815260200182805461014f9061062b565b801561019c5780601f106101715761010080835404028352916020019161019c565b820191906000526020600020905b81548152906001019060200180831161017f57829003601f168201915b505050505081565b60015481565b60025481565b6000600360008373ffffffffffffffffffffffffffffffffffffffff1673ffffffffffffffffffffffffffffffffffffffff168152602001908152602001600020549050919050565b600081600360003373ffffffffffffffffffffffffffffffffffffffff1673ffffffffffffffffffffffffffffffffffffffff16815260200190815260200160002054101561027d576040517f08c379a0000000000000000000000000000000000000000000000000000000008152600401610274906106a8565b60405180910390fd5b81600360003373ffffffffffffffffffffffffffffffffffffffff1673ffffffffffffffffffffffffffffffffffffffff16815260200190815260200160002054106103dd5781600360003373ffffffffffffffffffffffffffffffffffffffff1673ffffffffffffffffffffffffffffffffffffffff168152602001908152602001600020600082825461031291906106f7565b9250508190555081600360008573ffffffffffffffffffffffffffffffffffffffff1673ffffffffffffffffffffffffffffffffffffffff1681526020019081526020016000206000828254610368919061072b565b925050819055508273ffffffffffffffffffffffffffffffffffffffff163373ffffffffffffffffffffffffffffffffffffffff167fddf252ad1be2c89b69c2b068fc378daa952ba7f163c4a11628f55a4df523b3ef846040516103cc91906104af565b60405180910390a3600190506103de565b5b92915050565b600081519050919050565b600082825260208201905092915050565b60005b8381101561041e578082015181840152602081019050610403565b60008484015250505050565b6000601f19601f8301169050919050565b6000610446826103e4565b61045081856103ef565b9350610460818560208601610400565b6104698161042a565b840191505092915050565b6000602082019050818103600083015261048e818461043b565b905092915050565b6000819050919050565b6104a981610496565b82525050565b60006020820190506104c460008301846104a0565b92915050565b600080fd5b600073ffffffffffffffffffffffffffffffffffffffff82169050919050565b60006104fa826104cf565b9050919050565b61050a816104ef565b811461051557600080fd5b50565b60008135905061052781610501565b92915050565b600060208284031215610543576105426104ca565b5b600061055184828501610518565b91505092915050565b61056381610496565b811461056e57600080fd5b50565b6000813590506105808161055a565b92915050565b6000806040838503121561059d5761059c6104ca565b5b60006105ab85828601610518565b92505060206105bc85828601610571565b9150509250929050565b60008115159050919050565b6105db816105c6565b82525050565b60006020820190506105f660008301846105d2565b92915050565b7f4e487b7100000000000000000000000000000000000000000000000000000000600052602260045260246000fd5b6000600282049050600182168061064357607f821691505b602082108103610656576106556105fc565b5b50919050565b7f536f7272792c796f75722062616c616e63652069736e277420656e6f75676800600082015250565b6000610692601f836103ef565b915061069d8261065c565b602082019050919050565b600060208201905081810360008301526106c181610685565b9050919050565b7f4e487b7100000000000000000000000000000000000000000000000000000000600052601160045260246000fd5b600061070282610496565b915061070d83610496565b9250828203905081811115610725576107246106c8565b5b92915050565b600061073682610496565b915061074183610496565b9250828201905080821115610759576107586106c8565b5b9291505056fea264697066735822122097991a00d8a0c1e2779afb5eea275599ccf6b545868b33c466fb55bcad0792d264736f6c63430008100033',
    arguments: [
        _name = "CHC",
        _totalSupply = "21000000",
    ]
}).send({
    from: '0x4C5B3fc298Cc00f2f5A6E06232D403cB9C5bD9Ba',
    gas: '4700000'
}, function (e, contract) {
    console.log(e, contract);
    if (typeof contract.address !== 'undefined') {
        console.log('Contract mined! address: ' + contract.address + ' transactionHash: ' + contract.transactionHash);
    }
}).then(function (contract) {
    console.log("Contract address:", contract.options.address);
});
```

- 接下来正式执行合约部署，输入命令：`node deploy.js`

![捕获203.PNG](https://cdn.nlark.com/yuque/0/2022/png/28544794/1661182650086-f22240ae-cdaa-4ef1-8aa9-b5e1d15832ed.png#averageHue=%23292725&clientId=ua3557398-4c52-4&from=ui&id=uebd019a0&originHeight=305&originWidth=876&originalType=binary&ratio=1&rotation=0&showTitle=false&size=139771&status=done&style=none&taskId=u3bfa8fae-3530-4dd5-80ee-81c3310efe6&title=)<br />——发现这里报了一个错，原因是部署合约的账户没有解锁<br />——解决方法：管理员模式新打开一个cmd，执行命令：`geth attach ipc:\\.\pipe\geth.ipc`，然后执行：`personal.unlockAccount(eth.accounts[0])`，输入密码即可解锁

- 解锁后再次执行命令：`node deploy.js`

——出现下图这种情况代表智能合约交易创建成功：<br />![捕获233.PNG](https://cdn.nlark.com/yuque/0/2022/png/28544794/1661182922937-3a2852d9-c638-4825-8d41-6754714e5574.png#averageHue=%23222121&clientId=ua3557398-4c52-4&from=ui&id=u286b5b25&originHeight=230&originWidth=951&originalType=binary&ratio=1&rotation=0&showTitle=false&size=73801&status=done&style=none&taskId=u6d2459c3-7621-4836-afa4-29d2afadc97&title=)

- 但是还没有成功上链，因此在之前打开的cmd终端执行`miner.start()`，即挖矿

——成功界面如下，箭头所指的即为合约地址，**注意保存**<br />![捕获306.PNG](https://cdn.nlark.com/yuque/0/2022/png/28544794/1661183449850-e7f44d06-daaf-4fed-8488-14d1b9eb0fa7.png#averageHue=%23252423&clientId=ua3557398-4c52-4&from=ui&id=u7b580372&originHeight=213&originWidth=957&originalType=binary&ratio=1&rotation=0&showTitle=false&size=94432&status=done&style=none&taskId=ud543a7cc-7141-4c02-b0aa-72015b39c8f&title=)

<a name="g35uK"></a>
## <br />
<a name="j3nZM"></a>
# 三：Web3.js上调用合约
——————说明：这仅仅只是一个简易的demo，目的是熟悉过程
<a name="oC2MR"></a>
## 3.1  node.js安装express框架

- 直接在Vscode终端执行命令（还是在当前目录下）：`npm install -g express`
- 继续在Vscode终端执行命令：`npm install express-generator -g`

![捕获300.PNG](https://cdn.nlark.com/yuque/0/2022/png/28544794/1661184305438-142bef12-29a4-4518-af2f-a2b5e41ea3c7.png#averageHue=%23262523&clientId=ua3557398-4c52-4&from=ui&id=u337af169&originHeight=313&originWidth=1101&originalType=binary&ratio=1&rotation=0&showTitle=false&size=99746&status=done&style=none&taskId=u5817c70d-efc7-4d03-a270-1bc5f0f007f&title=)

<a name="O1wea"></a>
## 3.2  新建合约调用工程

- 执行命令：`express -e contractTest`，然后会发现"Web3Project"文件夹下立马多了一个名为"contractTest"的文件夹
- 在vscode终端执行：`cd contractTest`；`npm install`；`npm install web3`

——出现以下界面代表执行成功<br />![捕获400.PNG](https://cdn.nlark.com/yuque/0/2022/png/28544794/1661184758273-ef8acf2e-0a01-42b2-87e9-4f5d76ca90b8.png#averageHue=%232b2925&clientId=ua3557398-4c52-4&from=ui&id=u8f98fc7f&originHeight=644&originWidth=1383&originalType=binary&ratio=1&rotation=0&showTitle=false&size=297875&status=done&style=none&taskId=u279ea58b-3d0a-4a7c-a9b9-f073d99d968&title=)

<a name="GP1SP"></a>
## 3.3  修改文件
<a name="RZKX4"></a>
### 3.3.1  修改"app.js"文件
app.js，这个文件是使合约能够进行网页调用的"入口程序"
> ``app.js（全部粘贴，无需改动，不同合约此文件都一样，当然只是demo啦）

```javascript
var createError = require('http-errors');
var express = require('express');
var path = require('path');
var cookieParser = require('cookie-parser');
var logger = require('morgan');

var indexRouter = require('./routes/index');
var usersRouter = require('./routes/users');

var app = express();
var ejs = require('ejs');

// view engine setup
app.set('views', path.join(__dirname, 'views'));
app.engine('.html', ejs.__express);
app.set('view engine', 'html');
// app.set('view engine', 'ejs');

app.use(logger('dev'));
app.use(express.json());
app.use(express.urlencoded({ extended: false }));
app.use(cookieParser());
app.use(express.static(path.join(__dirname, 'public')));

app.use('/', indexRouter);
app.use('/users', usersRouter);

// catch 404 and forward to error handler
app.use(function (req, res, next) {
  next(createError(404));
});

// error handler
app.use(function (err, req, res, next) {
  // set locals, only providing error in development
  res.locals.message = err.message;
  res.locals.error = req.app.get('env') === 'development' ? err : {};

  // render the error page
  res.status(err.status || 500);
  res.render('error');
});

module.exports = app;
```
<a name="prS6F"></a>
### 3.3.2   （初步）修改route目录下的index.js  
index.js，这个文件是用来进行合约调用的"调用命令文件"，用户所有可以在Dapp中实现的"获取合约状态信息"和"修改合约状态"的等等操作都在此文件被定义
```javascript
var express = require('express');
var router = express.Router();

var Web3 = require('web3');
var web3 = new Web3(new Web3.providers.HttpProvider('http://localhost:8545'));

web3.eth.getAccounts().then(console.log);

/* GET home page. */
router.get('/', function(req, res, next) {
  res.render('index', { title: 'Express' });
});

module.exports = router;
```
<a name="GjxFP"></a>
### 3.3.3    在view的目录下新建index.html  
index.html，这个文件是DAPP的前端展示文件，页面会呈现给用户什么东西都在这个文件实现（当然，还得有css文件啦，但demo搞得这么花里胡哨意义不大，那些东西都是前端人员去解决的）
```javascript
<!DOCTYPE html>
<html>

<head>
    <title>调用合约</title>
    <link rel='stylesheet' href='/stylesheets/style.css' />
</head>

<body>
    <h1>调用合约</h1>
</body>

</html>
```
——在vscode终端执行：`npm start`，如果能成功执行，即可进行后面的**3.4**<br />![捕获436.PNG](https://cdn.nlark.com/yuque/0/2022/png/28544794/1661187031048-e575b633-901a-427d-8f3e-ce7c673ae95e.png#averageHue=%23212020&clientId=u1ef706a1-5452-4&from=ui&id=ue5614f23&originHeight=289&originWidth=818&originalType=binary&ratio=1&rotation=0&showTitle=false&size=45382&status=done&style=none&taskId=ub4c8b473-9b96-431c-ae9a-4c051c56f71&title=)

<a name="PLFqh"></a>
## 3.4  核心：修改index.js文件
前面说过，index.js文件，是用来进行合约调用的"调用命令文件"，用户所有可以在Dapp中实现的"获取合约状态信息"和"修改合约状态"的等等操作都在此文件被定义<br />————先提供一份简单的合约：<br />**这合约随便写的，你很容易会发现合约没有权限设置，甚至连安全数学都没搞，突出一个离谱**<br />emmm，你可能还发现第18行代码有错误：应该改为（`balance[msg.sender] = totalSupply`），没有那条下划线......（这里是我故意写错的，目的后面再说）
```javascript
//SPDX-License-Identifier: MIT
pragma solidity ^0.8.16;
interface ERC20 {
    function balanceOf(address _address) external view returns (uint);
    function transfer(address _receiver, uint amount) external returns (bool success);
    event Transfer(address indexed from, address indexed to, uint tokens);   
}

contract fabi01 is ERC20 {
    string public name  ;  
    uint public totalSupply ;
    uint public decimals;   
    mapping(address=>uint) balance;  
    constructor(string memory _name, uint256 _totalSupply) {
        name = _name;
        decimals = 18;
        totalSupply = _totalSupply * 10**decimals;
        balance[msg.sender] = _totalSupply;    
        emit Transfer(address(0), msg.sender, totalSupply);   
    }
  
    function balanceOf(address _address) public view returns(uint) {
        return(balance[_address]);     
    }
  
    function transfer(address  _receiver, uint amount) public returns(bool success) {
        if(balance[msg.sender] < amount) {
            revert("Sorry, your balance is insufficient"); 
        }
        if(balance[msg.sender] >= amount) {
            balance[msg.sender] -= amount;
            balance[_receiver] += amount;
            emit Transfer(msg.sender, _receiver, amount);
            return true;       
        }
    }
}
```
——现在转移至index.js文件进行修改：

- **首先**，在index.js定义要调用的合约
<a name="ZYjHJ"></a>
### 3.4.1  定义要调用的合约
`var myContract = new web3.eth.Contract(合约的ABI文件,合约地址)`<br />合约的地址和ABI文件前面已经保存了，直接复制粘贴过来即可
```javascript
var express = require('express');
var router = express.Router();

var Web3 = require('web3');
var web3 = new Web3(new Web3.providers.HttpProvider('http://localhost:8545'));

web3.eth.getAccounts().then(console.log);

var myContract = new web3.eth.Contract(
  [
    {
      "inputs": [
        {
          "internalType": "string",
          "name": "_name",
          "type": "string"
        },
        {
          "internalType": "uint256",
          "name": "_totalSupply",
          "type": "uint256"
        }
      ],
      "stateMutability": "nonpayable",
      "type": "constructor"
    },
    {
      "anonymous": false,
      "inputs": [
        {
          "indexed": true,
          "internalType": "address",
          "name": "from",
          "type": "address"
        },
        {
          "indexed": true,
          "internalType": "address",
          "name": "to",
          "type": "address"
        },
        {
          "indexed": false,
          "internalType": "uint256",
          "name": "tokens",
          "type": "uint256"
        }
      ],
      "name": "Transfer",
      "type": "event"
    },
    {
      "inputs": [
        {
          "internalType": "address",
          "name": "_address",
          "type": "address"
        }
      ],
      "name": "balanceOf",
      "outputs": [
        {
          "internalType": "uint256",
          "name": "",
          "type": "uint256"
        }
      ],
      "stateMutability": "view",
      "type": "function"
    },
    {
      "inputs": [],
      "name": "decimals",
      "outputs": [
        {
          "internalType": "uint256",
          "name": "",
          "type": "uint256"
        }
      ],
      "stateMutability": "view",
      "type": "function"
    },
    {
      "inputs": [],
      "name": "name",
      "outputs": [
        {
          "internalType": "string",
          "name": "",
          "type": "string"
        }
      ],
      "stateMutability": "view",
      "type": "function"
    },
    {
      "inputs": [],
      "name": "totalSupply",
      "outputs": [
        {
          "internalType": "uint256",
          "name": "",
          "type": "uint256"
        }
      ],
      "stateMutability": "view",
      "type": "function"
    },
    {
      "inputs": [
        {
          "internalType": "address",
          "name": "_receiver",
          "type": "address"
        },
        {
          "internalType": "uint256",
          "name": "amount",
          "type": "uint256"
        }
      ],
      "name": "transfer",
      "outputs": [
        {
          "internalType": "bool",
          "name": "success",
          "type": "bool"
        }
      ],
      "stateMutability": "nonpayable",
      "type": "function"
    }
  ], '0xad15CF6527970B9769BB6b6b9dF39B7e0f5868d3'
)
```

- **后面的修改就完全自由了，取决于合约的具体功能：**
- **我们合约的每种功能都举个例子：**
<a name="IxJu8"></a>
### 3.4.2  call方法：
call方法针对的是合约里的"view"和"pure"修饰的状态变量或函数：<br />`myContract.methods.合约里的函数名(参数1, 参数2...).call({(可选)from:_, gasPrice:_, gasLimit:_}, function(error, result){_}).then((可选:执行其他逻辑)console log)`<br />其中，call里面的参数可以一个都没有；`function(error, result){_}`，这个是可选的回调函数，其第二个参数为合约方法的执行结果，第一个参数为错误对象的返回值<br />——说这么多没用，这里直接举两个例子：
```javascript
//example 1
myContract.methods.name().call({
	//这里的name()函数在合约纸面上是没有定义的,但合约里定义了一个名为name的状态变量,并且用了
	//public去修饰,solidity规定:只要状态变量用public去修饰,就会生成与状态变量同名的函数
  from: '0x475adc73ebec71d14ca6f40350eb920591b006d6',
  gasPrice: '4700000',
	//from, gasPrice, gasLimit三个参数是可选参数,可以一个都不写
}, function (error, result) {
  console.log(error, result)
	//这个function(error, result){___}的执行逻辑是：如果能成功获取到name()的值，error就
	//等于null，console.log(error, result)会打印 null 和 name 的结果；如果前面获取不到
	//name()的返回值，就会将错误传入error，最终程序打印出error的具体信息
}).then(console.log); //最后这个console.log打印的是name()的结果


//example 2
myContract.methods.totalSupply().call({
	//想要获取合约内代币的总量
  from: '0x475adc73ebec71d14ca6f40350eb920591b006d6'
}, function (error, result) {
  if (error === null) {
    console.log("result1:", result)
  }
}).then(
	//如果上面的部分代码都执行成功，会继续执行合约里的balanceOf(address _address)函数
  myContract.methods.balanceOf('0x4c5b3fc298cc00f2f5a6e06232d403cb9c5bd9ba').call()
		//将获取到的代币余额规定为balance参数，以便最后格式化输出该账户的代币余额
    .then(function (balance) {
      (console.log("result2:", balance))
    }));
```
——执行截图：<br />项目方的余额比总供应量少了18个0（自合约发布起，还没有进行过任何一次转账），这个bug前面提到过了，目的是为了演示上面的js代码具体的运行逻辑是怎样的<br />那么问题来了：这个项目可操控的合约代币数量（实际总供应量）有多少个呢？答案是只有零点零零零.......二一个，而不是2100万个，异常的稀缺是吧<br />![捕获99.PNG](https://cdn.nlark.com/yuque/0/2022/png/28544794/1661265319545-26c9e91b-e4b0-4483-b934-d155e37fc631.png#averageHue=%23202020&clientId=u37f4934e-7008-4&from=ui&id=u2653a013&originHeight=305&originWidth=645&originalType=binary&ratio=1&rotation=0&showTitle=false&size=38673&status=done&style=none&taskId=uabdfa48a-f7ec-423c-a230-129e8547222&title=)<br />——你可能会遇到这种截图：<br />就是输出顺序与"index.js"中的代码顺序是不一致的，这个很正常。因为有".then()"函数的存在，".then()"是要等前面的代码拿到区块链上的数据才被执行的，即异步调用。<br />![捕获100.PNG](https://cdn.nlark.com/yuque/0/2022/png/28544794/1661265736118-54f0a693-e69e-49c2-8af4-e73979613796.png#averageHue=%23212020&clientId=u37f4934e-7008-4&from=ui&id=ue0010526&originHeight=288&originWidth=650&originalType=binary&ratio=1&rotation=0&showTitle=false&size=38685&status=done&style=none&taskId=u8a218f8c-1ba5-4c41-84e2-a08cb82c7c1&title=)
<a name="DjTme"></a>
### 3.4.3  send方法：
只要是需要改变区块链上的状态的，统一都用send，比如本篇用到的合约中的transfer(address  _receiver，uint  amount)函数，向其他地址发送合约代币，是需要改变"balance"这个状态变量的<br />——send( )方法与call( )方法类似：<br />`myContract.methods.合约里的函数名(参数1, 参数2...).send({(必选)from:_, (后面三个为可选)gasPrice:_, gasLimit:_, value:_(这个value指的是发送的ETH的数量，不是合约的代币数量)}, function(error, result){_}).then((可选:执行其他逻辑)console log)`<br />直接上代码：
```javascript
//example:
myContract.methods.transfer('0x475adc73ebec71d14ca6f40350eb920591b006d6', 10000).send(
  { from: '0x4c5b3fc298cc00f2f5a6e06232d403cb9c5bd9ba',
	//从 '0x4c5b3fc298cc00f2f5a6e06232d403cb9c5bd9ba' 地址转10000(往前移18个0)个合约代币
	//给'0x475adc73ebec71d14ca6f40350eb920591b006d6'
	     /*value: '6000000000000000000'：这个的意思是可以在一笔交易里既转合约的代币，
			 又转ETH，但很尴尬，我这里的合约代码没有写payable相关的函数，无法演示*/
	}).then(console.log);


//下面的这几行用来查看'0x475adc73ebec71d14ca6f40350eb920591b006d6'的合约代币余额
/*但是要注意(很重要):第一次执行的时候地址的代币余额还是0,不会变为10000，因为异步调用的问题，修改合约的状态是需要挖矿的，需要的时间肯定比call方法获取到代币的余额慢，还没等balance状态修改完毕，就已经输出结果了；同样的，第二次执行时代币余额为10000，而不是20000
*/
myContract.methods.balanceOf('0x475adc73ebec71d14ca6f40350eb920591b006d6').call()
  .then(function (balance) {
    console.log("eth.accounts[1]的CHC代币余额是:", balance);
  });
```
————两次运行对比：<br />![捕获102.PNG](https://cdn.nlark.com/yuque/0/2022/png/28544794/1661269615050-05438e1e-62d4-4bc2-bcfa-c40821ff4cc5.png#averageHue=%23212020&clientId=u37f4934e-7008-4&from=ui&id=u5ec17ad1&originHeight=380&originWidth=686&originalType=binary&ratio=1&rotation=0&showTitle=false&size=63766&status=done&style=none&taskId=u59f1bc97-33bd-4674-ab32-3f219149325&title=)<br />![捕获103.PNG](https://cdn.nlark.com/yuque/0/2022/png/28544794/1661269621513-b7c4b6a0-a05a-4a8b-8bd0-ffdd7c7dfa60.png#averageHue=%23212120&clientId=u37f4934e-7008-4&from=ui&height=369&id=u8f16b087&originHeight=392&originWidth=730&originalType=binary&ratio=1&rotation=0&showTitle=false&size=89727&status=done&style=none&taskId=u1addf43b-a149-462f-8259-cd88d73ae17&title=&width=688)

