# 基于 Github Action 的自动化 TWRP 编译指南

## 广告

1. OrangeFox 项目 [点击此处访问](https://github.com/azwhikaru/Action-OFRP-Builder)

## 重要声明

1. Github Actions 服务**并非**无限资源，为避免浪费，请勿使用未经验证的源代码，建议仅用于自动化构建已稳定的代码仓库

2. 修改前请确认操作的是自己的仓库。**如需提交代码请使用"Fork"，否则请使用"Use this template"**

3. 问题和拉取请求**可能不会**得到回复。如确有必要，请通过个人资料中的邮箱联系

4. Debian (Ubuntu) 已**移除** Python 2 支持。如需编译 Android 8.1 及以下版本，请使用 *Recovery Build (Legacy)* 工作流

5. 请勿咨询与您源代码相关的问题，例如：
	- 缺少编译规则...
	- 镜像... 超出大小限制

## 致谢

感谢所有贡献者

## 参数说明

| 参数名               | 说明                                       | 示例                                                         |
|----------------------|--------------------------------------------|--------------------------------------------------------------|
| `MANIFEST_URL`       | 源码仓库地址                               | https://github.com/minimal-manifest-twrp/platform_manifest_twrp_aosp.git |
| `MANIFEST_BRANCH`    | 源码分支                                   | twrp-12.1                                                    |
| `DEVICE_TREE_URL`    | 设备树地址                                 | https://github.com/TeamWin/android_device_asus_I003D         |
| `DEVICE_TREE_BRANCH` | 设备树分支                                 | android-12.1                                                 |
| `DEVICE_PATH`        | 设备树存放路径                             | device/asus/I003D                                            |
| `COMMON_TREE_URL`    | 公共设备树地址                             | https://github.com/TeamWin/android_device_asus_sm8250-common |
| `COMMON_PATH`        | 公共设备树存放路径                         | device/asus/sm8250-common                                    |
| `DEVICE_NAME`        | 设备型号                                   | I003D                                                        |
| `MAKEFILE_NAME`      | Makefile 名称                              | twrp_I003D                                                   |
| `BUILD_TARGET`       | 编译目标分区 (boot/recovery/vendorboot)    | recovery                                                     |

-----

## 使用指南

```
示例用户名：JohnSmith
```

#### 0. 如需提交代码，请点击仓库右上角的"Fork"（不建议）

![分叉按钮](https://user-images.githubusercontent.com/37921907/177914706-c92476c5-7e14-4fb3-be94-0c8a11dae874.png)

#### 1. 如需直接使用模板，请点击右上角的"Use this template"

![模板按钮](https://github.com/azwhikaru/Action-TWRP-Builder/assets/37921907/fae6ce3c-bd4c-4bbe-8050-5dd29dff2522)

#### 2. 自动跳转后可见以您用户名创建的仓库

![用户仓库](https://user-images.githubusercontent.com/37921907/177915106-5bde6fc9-303c-479e-b290-22b48efd1e4e.png)

#### 3. （可选）在[workflow文件](https://github.com/CaptainThrowback/Action-Recovery-Builder/blob/main/.github/workflows/Recovery%20Build.yml#L100-L101)中修改用户名和邮箱为您的Github信息

## SSH密钥配置（可选）

#### 4. 进入仓库设置 -> Deploy keys -> 添加部署密钥

#### 5. 在安卓设备上安装 [Termux](https://github.com/termux/termux-app/releases)

#### 6. 在Termux中安装openssh并生成SSH密钥（不要设置密钥密码）

注意：生成密钥时请在注释中包含仓库地址（示例命令）：
（将"owner"替换为您的Github用户名）

```bash
pkg install openssh
ssh-keygen -t ed25519 -C "git@github.com:owner/Action-Recovery-Builder.git"
```

#### 7. 添加公钥到仓库：

```bash
cd /data/data/com.termux/files/usr/etc/ssh
cat ssh_host_ed25519_key.pub
```
复制输出内容到部署密钥的Key栏

#### 8. 添加私钥到仓库Secrets：

```bash
cat ssh_host_ed25519_key
```
复制私钥内容，在仓库设置 -> Secrets -> Actions -> 新建仓库密钥：
名称：SSH_PRIVATE_KEY
值：粘贴私钥内容

## 开始编译

#### 9. 进入Actions -> Recovery Build

![工作流选择](https://user-images.githubusercontent.com/37921907/177915304-8731ed80-1d49-48c9-9848-70d0ac8f2720.png)

#### 10. 点击"Run workflow"并参照参数说明填写配置

![工作流配置](https://user-images.githubusercontent.com/37921907/177915346-71c29149-78fb-4a00-996f-5d84ffc9eb8c.png)

#### 11. 填写完成后点击"Run workflow"启动编译

-----

## 编译结果

可在 [Release页面](../../releases) 下载

-----

## 完整源代码

本项目的完整源代码可通过以下方式获取：

1. **使用模板创建**：
   - 点击仓库顶部的"Use this template"按钮
   - 选择目标仓库名称和所有者
   - 新创建的仓库将包含所有构建脚本和配置文件

2. **直接访问源码仓库**：
   ```bash
   git clone https://github.com/CaptainThrowback/Action-Recovery-Builder.git
   ```

项目结构概要：
```
├── .github
│   └── workflows
│       └── Recovery Build.yml   # 主构建工作流配置
├── cleanup.sh                   # 环境清理脚本
├── README.md                    # 说明文档
└── build                        # 构建脚本目录
    ├── build.sh                 # 主构建脚本
    └── envsetup.sh              # 环境配置脚本
```

核心构建脚本示例 (build/build.sh):
```bash
#!/bin/bash
# 设置编译环境
source build/envsetup.sh

# 初始化仓库
repo init -u ${MANIFEST_URL} -b ${MANIFEST_BRANCH} --depth=1
repo sync -c -j$(nproc --all) --force-sync --no-clone-bundle --no-tags

# 克隆设备树
git clone ${DEVICE_TREE_URL} -b ${DEVICE_TREE_BRANCH} ${DEVICE_PATH}
[ -n "${COMMON_TREE_URL}" ] && git clone ${COMMON_TREE_URL} -b ${DEVICE_TREE_BRANCH} ${COMMON_PATH}

# 开始编译
export ALLOW_MISSING_DEPENDENCIES=true
lunch twrp_${DEVICE_NAME}-eng
mka -j$(nproc --all) ${MAKEFILE_NAME}
```

注意事项：
1. 编译环境基于Ubuntu 22.04 LTS
2. 默认使用Java 11进行编译
3. 自动处理依赖安装和缓存优化
4. 支持并行编译加速（根据CPU核心数自动调整）

如需修改编译参数，可编辑以下文件：
- `.github/workflows/Recovery Build.yml` 调整运行环境
- `build/build.sh` 修改编译流程
- `cleanup.sh` 自定义清理规则
