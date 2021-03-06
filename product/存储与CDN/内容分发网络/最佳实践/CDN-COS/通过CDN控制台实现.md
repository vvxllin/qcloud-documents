本文详细描述了通过 CDN 加速 COS 的整体操作流程和具体的操作方法。

## 前提条件

1. 完成腾讯云账号注册、实名认证。
2. 创建 COS 存储桶，详情请参见 [创建存储桶](https://cloud.tencent.com/document/product/436/13309)。

## 操作指南

### 添加域名

登录 [CDN 控制台](https://console.cloud.tencent.com/cdn)，在左侧导航栏中，单击【域名管理】进入域名管理页面，单击【添加域名】。 ![img](https://main.qcloudimg.com/raw/3d2e458b232cdc89fafb546045a92bc1.png)

### 选择 COS 作为源站

#### 第一部分：域名配置

在域名处填充您需要加速的自身的服务域名，为其选择项目、加速区域及业务类型：
![](https://main.qcloudimg.com/raw/14c8ed2cdc340731c886b28339fb2377.png)

**配置项详解：**

| 配置项   | 配置说明                                                     |
| -------- | ------------------------------------------------------------ |
| 域名     | 1. 域名长度不超过50个字符。<br/>2. 域名已经在工信部进行过备案。<br/>3.域名为 a.test.com、a.b.test.com 等形式子域名或 \*.test.com、\*.a.test.com 形式泛域名。<br/>4.若域名为泛域名或已被其他用户接入，需要进行 [所有权验证](#m1) 后方可接入或取回。<br/><br/><strong>注意事项：</strong><br/>1. 接入泛域名后，暂不支持子域名或二级泛域名在其他账号接入。<br/>2. 暂不支持 *.test.com 与 *.a.test.com 同时接入。 |
| 所属项目 | 项目为腾讯云所有云产品共享资源集概念，[项目管理](https://console.cloud.tencent.com/project) 中可进行项目相关操作。 |
| 加速区域 | 中国境内：全球用户访问均会调度至中国境内加速节点进行服务。<br/>中国境外：全球用户访问均会调度至中国境外的加速节点进行服务。<br/>全球：全球用户访问将会择优调度至最近节点进行服务。<br/><br/><strong>注意事项：</strong><br/>中国境内与中国境外加速服务分开计费，计费策略 [单击查看](https://cloud.tencent.com/document/product/228/2949)。 |
| 业务类型 | 腾讯云 CDN 针对不同业务类型进行了针对性的加速性能优化，<br/>建议选择与自身业务更加贴近的业务类型，来获取更优质的加速效果。<br/><br/>静态加速：适用于电商类、网站类、游戏图片类小型资源加速场景。<br/>下载加速：适用于游戏安装包、音视频源文件下载、手机固件分发等下载场景。<br/>流媒体点播加速：适用于在线教育、在线视频点播等场景。 |
| 加速协议 | IPv4：节点仅支持 IPv4 访问。<br/>IPv4 + IPv6：节点同时支持 IPv4、IPv6 访问，仅当勾选此选项时，可配置 IPv6 源站。<br/><br/><strong>注意事项：</strong><br/>1. 仅中国境内支持 IPv6。<br/>2. IPv4+IPv6 协议支持需 [申请免费试用](https://cloud.tencent.com/apply/p/own2eu41dg8)。 |

#### 第二部分：源站配置

配置业务源站相关信息，CDN 节点在缓存无资源时，会回源站拉取并缓存：
![](https://main.qcloudimg.com/raw/4f42fd947674dbe59ed5c49cddaa65a9.png)

1. 在**域名配置**中的**源站类型**中选择：COS源（对象存储）。
2. 选择对应的**存储桶**的域名。
3. 开启**私有存储桶访问**，需先对 CDN 服务授权。确认授权后可手动开启。
4. 根据源站支持情况，选择回源请求协议。

#### 第三部分：服务配置

配置节点加速服务相关配置：
![](https://main.qcloudimg.com/raw/9796ca5c1cfc2f2816026de0ea450180.png)

**配置项详解：**

| 配置项   | 配置说明                                                     |
| -------- | ------------------------------------------------------------ |
| 基础配置 | 节点缓存资源遵循 Key-Value 映射，其中 Key 为资源 URL 。<br/>开启过滤参数，Key 会忽略 URL 中 “?” 之后参数进行映射。<br/>不开启过滤参数，Key 为完整资源 URL。<br/>静态加速类型默认不开启，下载、流媒体点播加速类型默认开启。 |
| 分片回源 | 配置回源时是否进行分片，源站需要支持分片才可开启。<br/>对象存储源站默认开启分片回源。 |
| 缓存规则 | 节点缓存过期时间配置，默认情况下所有文件缓存过期时间为30天。<br/>配置的节点缓存过期时间为最长过期时间，受节点存储资源影响，实际缓存时间视情况而定。 |

### 接入完成

输入**添加域名**页面所有配置后，单击【确认提交】完成添加域名操作，请耐心等待域名配置下发至全网节点，下发时间约5 - 10分钟。

### 配置 CNAME

添加域名成功后，在**域名管理**页面，可以查看到 CDN 为您的域名分配的加速 CNAME，您需要前往接入域名的 DNS 服务商（如 Dnspod）处，为此域名添加一条 CNAME 记录，待 **DNS 配置生效后**，即可进行加速服务。详情请参见 [CNAME 配置](https://cloud.tencent.com/doc/product/228/3121)。
