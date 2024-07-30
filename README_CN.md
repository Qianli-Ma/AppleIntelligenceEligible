## <div align="center"><b><a href="README.md">English</a> | <a href="README_CN.md">简体中文</a></b></div>

# 国行 Mac 使用 Apple Intelligence 的方法
该方案由[Kyle-Ye/XcodeLLMEligible](https://github.com/Kyle-Ye/XcodeLLMEligible)修改而来。

在不禁用系统完整性保护 (SIP) 的情况下在国行 Mac 上使用 Apple Intelligence 的方法。

对于需要禁用系统完整性保护 (SIP) 的旧方法，请参阅 "相关链接" 部分。

![屏幕截图](images/screenshot.png)

## 注意事项

这个项目仅用于学习和研究目的。

如果您选择使用此项目，您将自行承担风险，并有责任遵守任何适用法律。

本项目的作者对您使用本项目可能产生的任何后果概不负责。

## 使用方式

> [!NOTE]
> 仅在macOS 15 Beta 1 ~ Beta 4下可以开启Xcode LLM
> macOS 15.1 Beta 1 中可以开启Apple Intelligence
> 只要苹果不删除或更改 eligibility 服务的 override 实现，预计可以在 macOS 15.x 的后续版本上工作

### 脚本执行

#### 安装

```shell
curl -L https://raw.githubusercontent.com/Qianli-Ma/AppleIntelligenceEligible/main/override_AI.sh | bash
```

#### 卸载

```shell
curl -L https://raw.githubusercontent.com/Qianli-Ma/AppleIntelligenceEligible/main/override_AI.sh | bash -s -- uninstall
```

### 手动执行

#### 方案一（推荐）

在脚本执行期间需要临时禁用一次 SIP。

1. 在恢复模式下通过 `csrutil disable` 禁用 SIP
2. 添加启动参数 `sudo nvram boot-args="amfi_get_out_of_my_way=1"`并重启
3. 从[发布页面](https://github.com/Kyle-Ye/XcodeLLMEligible/releases)下载 `eligibility_util` 并执行 `./eligibility_util forceDomainAnswer --domain-name OS_ELIGIBILITY_DOMAIN_XCODE_LLM --answer 4`
4. 执行 `./eligibility_util forceDomainAnswer --domain-name OS_ELIGIBILITY_DOMAIN_GREYMATTER --answer 4`
5. 在恢复模式下通过 `csrutil enable` 恢复 SIP
6. 删除启动参数 `sudo nvram -d boot-args`

> 如果你不熟悉 SIP 操作，请阅读 [Disabling and Enabling System Integrity Protection](https://developer.apple.com/documentation/security/disabling_and_enabling_system_integrity_protection)。
>
> 你只能在恢复模式或禁用 SIP 的普通模式下设置boot-args。
>
> 设置boot-args后，记得重新启动以使更改生效。

#### 方案二

> [!NOTE]
> 方案二存在已知问题。详见 [#3](https://github.com/Kyle-Ye/XcodeLLMEligible/issues/3)。
>
> 如果此方案对你不起作用，请尝试方案一。

完全不需要禁用 SIP。

1. 从[发布页面](https://github.com/Kyle-Ye/XcodeLLMEligible/releases)下载 `eligibility_overrides.data` 文件
2. 在`~/Library/Daemon Containers/<UUID>`下找到`eligibilityd`的正确容器路径
3. 将下载的文件移动到相应的 Deamon 容器的 `Data/Library/Caches/NeverRestore/` 文件夹中。如果您不确定哪个是 eligibilityd 的容器目录，您可以一个一个地尝试，或者将下载的文件添加到所有 Deamon 容器中。

## 相关链接

- https://github.com/Kyle-Ye/eligibility/
- https://github.com/Kyle-Ye/XcodeLLMEligible

- https://gist.github.com/Kyle-Ye/4ad1aa92df3a31bd812487af65e16947
- https://gist.github.com/unixzii/6f25be1842399022e16ad6477a304286

## 版权信息

MIT。详见 LICENSE 文件。