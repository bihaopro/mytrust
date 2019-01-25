# MyTrust DAPP接口

## 概述

欢迎使用MyTrust DAPP接口，MyTrust提供SWTC、Ripple、Stellar、Callchain、Stream的钱包DAPP服务。MyTrust开放了第三方DAPP接口，让开放者可以开发自己的DAPP，为用户提供更多的DAPP构建能力。

## DAPP开发流程

 - DAPP开发者，只需要提供一个URL地址给MyTrust就行，就可以使用MyTrust APP里面用户钱包资产。
 - DAPP开发者不需要引以MyTrust的代码，也不需要开放任何代码给MyTrust。
 - 在DAPP加载完成之后，MyTrust钱包会在当前界面中注入DAPP SDK代码，当前DAPP需要监听window.MTL对象是否存在。当window.MTL对象存在时，即可以从中获得相应的当前钱包地址和接口等。
 - MyTrust提供测试版本的MyTrust便于用户进行自行添加、测试DAPP。


## 使用

在使用MyTrust DAPP接口的第一步是判断在浏览器中是否已经存在了MyTrust DAPP接口，可以通过如下方式进行检测

```js
if (typeof window.MTL === 'undefined') {
    alert('请在MyTrust APP中打开DAPP');
} else {
    // TODO 业务处理
}
```

## 属性

MyTrust在DAPP启动之后提供如下一些属性，可以供用户进行DAP片开发：

1. version, 当前钱包版本号
2. platform, 当前钱包的操作系统，ios或android
3. system, 当前钱包的钱包体系，jingtum, ripple, stellar, callchain, stream
4. address, 当前用户的钱包地址
5. contracts: [{ name: '名称', address: '地址'}], 当前钱包的联系人信息
6. locale, 当前钱包的语言设置, zh_cn

## 接口

接口提供异步的调用方式，返回信息格式为{success: boolean, msg: string, data: object}，其中success表示请求是否成功或者失败，msg表示请求回应的消息，data表示返回的接口。

接口定义如下两种特殊的类型，分别是address和amount，其中address是钱包地址的字符串，amount是金额的对象，其格式如下：
```json
{
    value: number,
    currency: string,
    issuer: string
}
```

### MTL.balance(callback)

返回用户当前钱包的余额

```js
MTL.balance(function(data) {
    console.dir(data);
});
```

### MTL.getOrders(callback)

获取用户的当前挂单列表，返回的数据格式根据每个钱包体系的而定。


### MTL.getTransaction(hash, callback)

获取用户的交易记录详情，返回的数据格式根据每个钱包体系的而定。


### MTL.getTransactions(options, callback)

获取用户的交易记录详情，返回的数据格式根据每个钱包体系而定。
其中options的格式基本为
```json
{
    cursor: {},
    limit: number
}
```
其中jingtum的cursor为{ledger: number, seq: number}，ripple的cursor为{ledger: number}，stellar的cursor为paging_token，callchain的cursor为{ledger: number}。


### MTL.getTrusts(address, callback)

获取用户的授信列表


### MTL.getOrderbook(base, counter, callback)

获取base/counter交易对的账本信息
base和counter格式为{currency: string, issuer: address}


### MTL.isValidAddress(address, callback)

提供工具类的方法，判断用户钱包地址是否有效


### MTL.pay(options, callback)

提交用户的支付请求，其中options的格式如下：
```json
{
    dest: address,
    amount: amount
    memo: string
}
```

### MTL.trust(options, callback)

用户进行资产授信，其中options的格式如下：
```json
{
    amount: amount,
    memo: string

}
```

### MTL.createOrder(options, callback)

用户进行挂单操作，其中options的格式如下：
```json
```

### MTL.cancelOrder(options)

用户进行取消挂单操作，其中options的格式如下：
```json
{
    sequence: integer
}
```
