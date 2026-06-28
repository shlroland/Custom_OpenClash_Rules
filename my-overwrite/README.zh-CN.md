# 我的 OpenClash 远程覆写

## 当前用法

当前 iStoreOS 推荐使用这条链路：

```text
上游订阅
  -> Sub-Store 聚合订阅链接
  -> 本地 subconverter 使用 my-cfg/Custom_Clash.ini 转换
  -> OpenClash 远程覆写模块的 EN_KEY1
```

OpenClash 远程覆写模块使用：

```text
https://raw.githubusercontent.com/shlroland/Custom_OpenClash_Rules/refs/heads/main/my-overwrite/IStoreOS_Current_ConfigURL.conf
```

在 OpenClash 中：

1. 打开远程覆写模块。
2. 新增模块：
   - 类型：`http`
   - URL：填写上面的 `IStoreOS_Current_ConfigURL.conf` 地址。
3. 添加环境变量：
   - `EN_KEY1`：本地转换器输出的最终 Clash/Mihomo YAML 地址。
4. 保存并应用配置。

`EN_KEY1` 应该填写本地转换器返回的最终配置地址，不是原始 Sub-Store 聚合订阅地址。这个最终地址里应当已经包含：

- Sub-Store 聚合订阅，作为转换器的 `url` 输入。
- 本仓库转换模板，作为转换器的 `config` 输入：

```text
https://raw.githubusercontent.com/shlroland/Custom_OpenClash_Rules/refs/heads/main/my-cfg/Custom_Clash.ini
```

本地转换器 URL 形状大致如下：

```text
http://<local-converter-host>:<port>/sub?target=clash&url=<urlencoded-sub-store-collection-url>&config=<urlencoded-template-url>
```

这样 OpenClash 只负责插件侧设置和下载最终 YAML。订阅聚合与模板转换由 Sub-Store 和本地转换器负责。

## 文件说明

- `IStoreOS_Current_ConfigURL.conf`：按当前 `192.168.20.10` 上 iStoreOS/OpenClash 状态整理的远程覆写模块。
- `Custom_Overwrite_ConfigURL.conf`：通用版远程覆写模块，从 `EN_KEY1` 下载最终转换配置。
- `Custom_Overwrite_Multi.conf`：多 provider 版本远程覆写模块。
- `Custom_Clash.yaml`：供多 provider 版本下载的最终 Clash/Mihomo YAML 示例。

## 关键地址

iStoreOS 当前配置版远程覆写模块：

```text
https://raw.githubusercontent.com/shlroland/Custom_OpenClash_Rules/refs/heads/main/my-overwrite/IStoreOS_Current_ConfigURL.conf
```

当前使用的转换模板：

```text
https://raw.githubusercontent.com/shlroland/Custom_OpenClash_Rules/refs/heads/main/my-cfg/Custom_Clash.ini
```

## 说明

- `IStoreOS_Current_ConfigURL.conf` 会保留当前路由器的插件侧姿态：Meta 核心、`fake-ip`、关闭 IPv6、开启绕过大陆 IP、开启自定义 DNS、开启 sniffer、关闭 Smart。
- `EN_KEY1` 必填，并且必须返回完整 Clash/Mihomo YAML。
- `my-cfg/*.ini` 是订阅转换模板，不能直接作为 OpenClash 远程覆写配置使用。
- 不要把私人订阅、dashboard 密码或认证密码写进仓库；这些应只放在 OpenClash 环境变量或你的本地服务中。
