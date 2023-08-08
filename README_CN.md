# device_samsung_a60q

[中文](./README_CN.md) / [English](./README.md)

20230808：从A70那边抄了一些东西过来，现在可以在 twrp 中 使用 `adb` 命令了！其实主要用处就是 push 和 pull 文件。解密还没有解决。目前只能用来构建 twrp-9.0（貌似是因为 twrp-9.0 是非动态分区能使用的最后一个版本）。

之前研究的记录是 https://github.com/Jason-summer/twrp_samsung_a60q 的 from-a70q 分支。有什么问题请在 issues 中提出。

## 刷入 Twrp

将 recovery.img 放进 Odin 的 AP 槽中，连接手机进入**下载模式**刷入即可。在 Linux 环境下也可使用 odin 程序的命令：

```
./odin4 -a recovery.img
```

## 自行构建

1. 同步源代码

可以更换清华大学的 [git-repo](https://mirror.tuna.tsinghua.edu.cn/help/git-repo/) 加快源码下载速度。下载和展开后需要至少28GB的硬盘空间。

```
repo init --depth=1 -u https://github.com/minimal-manifest-twrp/platform_manifest_twrp_omni.git -b twrp-9.0
repo sync
```

2. 下载和放置设备树

```
git clone https://github.com/Jason-summer/device_samsung_a60q.git -b twrp-9.0
```

将下载得到的所有文件放置到 `<source-dir>/samsung/a60q` 目录下！`<source-dir>` 就是下载源码的那个目录。

3. 配置 python 环境

twrp-9.0 诞生于 python3 出来之前，所以使用的是 python2。这里我使用 conda 来配置 python 环境，你可以按照你喜欢的方法配置。

```
conda create -n py27 python=2.7
conda activate py27
```

4. 构建

构建前确保电脑内存+虚拟内存>16G，不然可能构建失败。Samusng A6060 带有 recovery 分区，因此我们要构建的是 `recoveryimage`。构建途中出现 warnings 没关系，只要不是红色的 error 就行。

```
cd <source-dir>
export ALLOW_MISSING_DEPENDENCIES=true
. build/envsetup.sh
lunch omni_a60q-eng
mka recoveryimage
```

5. 得到生成的文件

当 bash 中出现类似下面内容的时候说明构建完成了。

```
[100% 12881/12881] ----- Making recovery image ------
#### build completed successfully (03:10 (mm:ss)) ####
```

这时可以在 `<source-dir>/out/target/product/a60q` 目录下找到生成的 `recovery.img`
