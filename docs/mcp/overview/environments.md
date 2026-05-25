# Environments

## Runtime model

MCP 运行时接入的核心动作只有几类：

- 选择环境
- 配置凭证
- 生成签名参数
- 调用能力

## Supported environment idea

当前文档默认至少存在以下环境语义：

- `sandbox`
- `production`

实际环境地址、可用能力与发布规则，应以正式平台配置和当前已发布参考资料为准。

## Prerequisites

环境切换不是开户流程的一部分。  
在进入运行时前，应当已经完成：

- 商户注册
- 访问开通
- API 凭证下发

## Operational guidance

- 在不同环境中分开管理 `apiKey` 与 `secretKey`
- 不要混用沙箱和生产请求参数
- 部署前先在目标环境重新验证签名、状态流与回调行为
