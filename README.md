### qcloudapi-sdk-php

qcloudapi-sdk-php是为了让PHP开发者能够在自己的代码里更快捷方便的使用腾讯云的API而开发的SDK工具包。

#### 资源

* [公共参数](http://wiki.qcloud.com/wiki/%E5%85%AC%E5%85%B1%E5%8F%82%E6%95%B0)
* [API列表](http://wiki.qcloud.com/wiki/API)
* [错误码](http://wiki.qcloud.com/wiki/%E9%94%99%E8%AF%AF%E7%A0%81)

#### 入门

1. 申请安全凭证。
在第一次使用云API之前，用户首先需要在腾讯云网站上申请安全凭证，安全凭证包括 SecretId 和 SecretKey, SecretId 是用于标识 API 调用者的身份，SecretKey是用于加密签名字符串和服务器端验证签名字符串的密钥。SecretKey 必须严格保管，避免泄露。

2. 下载SDK，放入到您的程序目录。
使用方法请参考下面的例子。

#### 例子

    <?php
    require_once './src/QcloudApi/QcloudApi.php';

    $config = array('SecretId'       => '你的secretId',
                    'SecretKey'      => '你的secretKey',
                    'RequestMethod'  => 'GET',
                    'DefaultRegion'  => '区域参数');

    // 第一个参数表示使用哪个域名
    // 已有的模块列表：
    // QcloudApi::MODULE_CVM    对应  cvm.api.qcloud.com
    // QcloudApi::MODULE_CDB    对应  cdb.api.qcloud.com
    // QcloudApi::MODULE_LB     对应  lb.api.qcloud.com
    // QcloudApi::MODULE_TRADE  对应  trade.api.qcloud.com
    // QcloudApi::MODULE_SEC    对应  csec.api.qcloud.com
    // QcloudApi::MODULE_IMAGE  对应  image.api.qcloud.com
    $service = QcloudApi::load(QcloudApi::MODULE_CVM, $config);

    // 请求参数，请参考wiki文档上对应接口的说明
    $package = array('offset' => 0,
                     'limit' => 3,
                     // 'Region' => 'gz' // 当Region不是上面配置的DefaultRegion值时，可以重新指定请求的Region
                    );

    // 请求方法为对应接口的接口名，请参考wiki文档上对应接口的接口名
    $a = $service->DescribeInstances($package);

    // 生成请求的URL，不发起请求
    $a = $service->generateUrl('DescribeInstances', $package);

    if ($a === false) {
    // 请求失败，解析错误信息
        $error = $service->getError();
        echo "Error code:" . $error->getCode() . ' message:' . $error->getMessage();
    } else {
    // 请求成功
        var_dump($a);
    }