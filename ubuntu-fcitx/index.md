# Ubuntu16.04安装中文输入法


这篇文章展示了如何在Ubuntu16.04系统中安装中文输入法.

<!--more-->

{{< admonition note "系统环境">}}  
* Ubuntu 16.04  
{{< /admonition >}}

## 1 安装Fcitx

{{< admonition note "配置">}}  
* `System Settings` -> `Language Support` -> `Install/Remove Languages`  
* 选择 `Chinese(simplified)` -> `Apply`
{{< /admonition >}}

## 2 重启系统

```Bash
reboot
```
## 3 设置输入法为Fcitx

{{< admonition note "配置">}}  
* `System Settings` -> `Language Support` -> `Keaboard input method system`  
* 选择 `fcitx` -> `close`
{{< /admonition >}}

## 4 重启系统

## 5 右上角出现企鹅

{{< admonition note "配置">}}  
* 点击企鹅 -> `ConfigureFcitx` -> 点击左下角"+" -> 取消勾选 `Only Show Current Language`
* 点击 `Sunpinyin` -> OK
{{< /admonition >}}

## 6 参考资料

{{< admonition note "参考资料">}}
https://blog.csdn.net/qq_38329375/article/details/81538443
{{< /admonition >}}

