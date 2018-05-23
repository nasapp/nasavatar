# nasavatar
为每个星云链上的钱包地址生成一个头像(avatar),可以在任何基于星云链的应用中使用个性化的头像代替钱包地址。需使用Chrome浏览器并且安装星云钱包插件。

头像保存在星云链的合约中，合约地址:n1fyqCkaLEYSA5njL3bBauzBKH6cNgiZBjR ,调用函数:get ,参数:钱包地址

在线演示地址：[https://avatar.chinacloudsites.cn/](https://avatar.chinacloudsites.cn/)

上传个人头像后，第三方Dapp可以通过执行合约中的函数，获取到钱包地址对应的头像，提高辨识度。

获取方法如下：

	function getavatar(address,imageid) {
            address = address.trim();
            if (!address) {
                console.log("请输入您的星云链地址");
                return;
            }
            var value = "0";
            var gas_price = "1000000"
            var gas_limit = "2000000"
            var callFunction = "get";
            var callArgs = JSON.stringify([address]);
            var contract = {
                "function": callFunction,
                "args": callArgs
            }
            neb.api.call(currentUserAddress, dappAddress, value, '0', gas_price, gas_limit, contract).then(function (resp) {
                if (resp.execute_err.length > 0) {
                    throw new Error(resp.execute_err);
                }
                var result = JSON.parse(resp.result);
                if (!result) {
                    throw new Error(resp);
                }
                $(imageid).attr('src',result); 
            }).catch(function (err) {
              throw new Error(err.message);
            });
        }
        
在HTML中留出头像位置：
`<img id="navimage" src="" alt="" width="100px" height="100px">      `

执行JS函数获取头像：
`getavatar(currentUserAddress,"#navimage");`

在上述JS函数中，JQuery语句将获取到的头像数据（BASE64编码）添加到头像中。
`$(imageid).attr('src',result); `