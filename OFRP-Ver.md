# ------------------------------------
#
# 为OrangeFox恢复定制构建变量
#
# 这些变量应该在构建之前声明 - 在shell脚本中（例如，在"vendorsetup.sh"中），或在命令行中
#
# 版权所有 (C) 2019-2022 OrangeFox恢复项目
# 日期：2022年2月4日
#
# -----------------------------------
#
#
"TARGET_ARCH"
   - 根据您的设备是32位还是64位，将其设置为"arm"或"arm64"
   - 例如："export TARGET_ARCH=arm"
   - 默认 = arm64
#
"OF_AB_DEVICE"
    - 设备是否为A/B设备
    - 如果您的设备是A/B设备，请设置为1（**确保它确实是**）
    - 如果启用此选项，将自动设置以下所有选项为"1"：
    		OF_USE_MAGISKBOOT
    		OF_USE_MAGISKBOOT_FOR_ALL_PATCHES
    - 默认 = 0
#
"FOX_PORTS_TMP" 
    - 指向用于创建zip安装程序的自定义临时目录
#
"FOX_PORTS_INSTALLER" 
    - 指向用于修改/添加额外安装程序文件的自定义目录
    - 在创建zip安装程序之前，内容将被简单复制
#
"FOX_LOCAL_CALLBACK_SCRIPT"
    - 指向一个自定义的"回调"脚本，该脚本将在创建最终恢复映像之前执行
    - 例如，一个用于删除某些文件或向ramdisk添加文件的脚本
#
"BUILD_2GB_VERSION" [已弃用]
    - 设置为1以构建适用于2GB RAM设备的精简版"lite"版本
    - 如果使用此选项，您*必须*每次都进行干净构建
    - 任何"lite"构建都需要非常彻底的测试以确保其正常工作（或至少能工作）
    - 默认 = 0
#
"FOX_REPLACE_BUSYBOX_PS"
    - 设置为1以替换（精简版）busybox版本的"ps"命令
    - 如果定义了此选项，busybox的"ps"命令将被更完整的（arm64）版本替换
    - 默认 = 0
    - 此选项不应为arm32设备启用
#
"FOX_REPLACE_TOOLBOX_GETPROP"
    - 设置为1以替换（精简版）toolbox版本的"getprop"命令
    - 如果定义了此选项，toolbox的"getprop"命令将被更完整的版本（resetprop）替换
    - 默认 = 0
#
"FOX_RECOVERY_INSTALL_PARTITION"
    - !!! 通常应保持原样 !!!
    - 仅当您的设备的恢复分区位于默认位置"/dev/block/bootdevice/by-name/recovery"之外时才设置此选项
    - 默认 = "/dev/block/bootdevice/by-name/recovery"
#
"FOX_RECOVERY_SYSTEM_PARTITION" [新] [R11仅]
    - !!! 通常应保持原样 !!!
    - 仅当您的设备的系统分区位于默认位置"/dev/block/bootdevice/by-name/system"之外时才设置此选项
    - （例如，一些三星/HTC设备的系统分区位置不同）
    - 默认 = "/dev/block/bootdevice/by-name/system"
#
"FOX_RECOVERY_VENDOR_PARTITION" [新] [R11仅]
    - !!! 通常应保持原样 !!!
    - 仅当您的设备的供应商分区位于默认位置"/dev/block/bootdevice/by-name/vendor"之外时才设置此选项
    - （例如，一些三星设备的供应商位置不同）
    - 默认 = "/dev/block/bootdevice/by-name/vendor"
#
"FOX_RECOVERY_BOOT_PARTITION" [新] [R11.1仅]
    - !!! 通常应保持原样 !!!
    - 仅当您的设备的boot分区位于默认位置"/dev/block/bootdevice/by-name/boot"之外时才设置此选项
    - （例如，一些设备的boot位置不同）
    - 默认 = "/dev/block/bootdevice/by-name/boot"
#
"FOX_USE_LZMA_COMPRESSION"
    - 如果您希望使用（速度慢但压缩效果更好的）lzma压缩方式对ramdisk进行压缩，请设置此选项为1；
    - * 这要求您的构建系统中有一个更新的lzma二进制文件，并且
    - * 在BoardConfig中设置（如果您没有自行设置，将自动设置）：
    -      LZMA_RAMDISK_TARGETS := recovery
    - * 您的内核还必须内置有lzma压缩支持
    - 默认 = 0（意味着使用标准gzip压缩（速度快，但压缩效果不如lzma））
#
"FOX_USE_TAR_BINARY"
    - 如果您希望添加gnu tar二进制文件（/sbin/gnutar），请设置此选项为1
    - 这必须在shell脚本或命令行中设置，构建之前
    - 这将使恢复映像的大小增加约420kb
    - 默认 = 0
#
"FOX_USE_SED_BINARY" [新]
    - 如果您希望添加gnu sed二进制文件（/sbin/gnused），请设置此选项为1
    - 这必须在shell脚本或命令行中设置，构建之前
    - 这将使恢复映像的大小增加约200kb
    - 默认 = 0
#
"FOX_USE_ZIP_BINARY" [自20200723起，此选项已过时]
#
"FOX_USE_NANO_EDITOR"
    - 如果您希望添加nano编辑器，请设置此选项为1
    - 这必须在shell脚本或命令行中设置，构建之前
    - 这将使恢复映像的大小增加约300kb
    - 默认 = 0
#
"FOX_USE_BASH_SHELL"
    - 如果您希望使用bash shell替换busybox "sh"，请设置此选项为1
    - 默认 = 0
    - 如果未设置，bash仍将被复制，但不会替换"sh"
#
"FOX_ASH_IS_BASH"
    - 如果您希望使用bash shell替换busybox "ash"，请设置此选项为1
    - 默认 = 0
    - 如果未设置，bash仍将被复制，但不会替换"ash"
#
"FOX_REMOVE_BASH"
    - 如果您希望从恢复中完全移除bash，请设置此选项为1
    - 默认 = 0
#
"OF_USE_MAGISKBOOT"
    - 设置为1以使用magiskboot对ROM的boot映像进行修补
    - 否则，将使用mkbootimg/unpackbootimg
    - magiskboot对boot映像的修补效果更好，但速度慢
    - 默认 = 0
#
"OF_USE_MAGISKBOOT_FOR_ALL_PATCHES"
    - 设置为1以使用magiskboot对boot映像和恢复映像进行所有修补
    - 这意味着将删除mkbootimg/unpackbootimg
    - 如果设置了此选项，此脚本还将自动将OF_USE_MAGISKBOOT设置为1
    - 默认 = 0
#
"OF_FORCE_MAGISKBOOT_BOOT_PATCH_MIUI"
    - 设置为1以强制使用magiskboot对ROM的boot映像进行修补，当
    - 安装MIUI ROM或在MIUI ROM上安装OrangeFox zip时（无论
    - DM-Verity或强制加密设置如何，如果用户选择了这些设置，将保持不变）- 这是为了阻止MIUI用其自己的stock recovery覆盖OrangeFox
    - "OF_USE_MAGISKBOOT"也必须设置为"1"才能使用此功能
    - 默认 = 0
#
"OF_USE_NEW_MAGISKBOOT" [已弃用]
#
"OF_DISABLE_UPDATEZIP"
    - 设置为1以禁用恢复zip的创建
    - 默认 = 0
#
"OF_SUPPORT_PRE_FLASH_SCRIPT" [已弃用]
#
"OF_DONT_PATCH_ON_FRESH_INSTALLATION"
    - 设置为1以防止在刷入OrangeFox zip时修补dm-verity和强制加密
    - 如果ROM启用了dm-verity，并且此选项打开，将会出现启动循环
    - 默认 = 0
#
"OF_TWRP_COMPATIBILITY_MODE" ; "OF_DISABLE_MIUI_SPECIFIC_FEATURES"
    - 设置任一选项为1以启用stock TWRP兼容模式
    - 在此模式下，MIUI OTA和dm-verity/强制加密修补将被禁用
    - 默认 = 0
#
"OF_DONT_PATCH_ENCRYPTED_DEVICE"
    - 设置为1以避免在加密设备上应用强制加密修补
    - 默认 = 0
    - 除非默认设置在您的设备上造成问题，否则不应使用此选项
    - 最新的小米设备似乎需要此选项
#
"OF_KEEP_DM_VERITY"; 
    - 设置为1以在每次启动时*不取消*OrangeFox "Disable DM-Verity"选项
    - 默认 = 0
#
"OF_KEEP_FORCED_ENCRYPTION"; 
    - 设置为1以在每次启动时*不取消*OrangeFox "Disable Forced Encryption"选项
    - 默认 = 0（在lavender中，默认=1）
#
"OF_KEEP_DM_VERITY_FORCED_ENCRYPTION"; 
    - 设置为1以在每次启动时*不取消*OrangeFox "Disable DM-Verity"和"Disable Forced Encryption"选项
    - 默认 = 0
#
"OF_FORCE_DISABLE_DM_VERITY"; [新]
    - 设置为1以在每次启动时**勾选**OrangeFox "Disable DM-Verity"选项
    - 默认 = 0
#
"OF_FORCE_DISABLE_FORCED_ENCRYPTION"; [新]
    - 设置为1以在每次启动时**勾选**OrangeFox "Disable Forced Encryption"选项
    - 默认 = 0
#
"OF_FORCE_DISABLE_DM_VERITY_FORCED_ENCRYPTION"; [新]
    - 设置为1以在每次启动时**勾选**OrangeFox "Disable DM-Verity"和"Disable Forced Encryption"选项
    - 默认 = 0
#
"OF_DISABLE_DM_VERITY"; [新]
    - 设置为1以在新安装时**勾选**OrangeFox "Disable DM-Verity"选项（默认）
    - 默认 = 0（对于较旧的小米设备如kenzo, libra, mido应设置为"1"）
#
"OF_DISABLE_FORCED_ENCRYPTION"; [新]
    - 设置为1以在新安装时**勾选**OrangeFox "Disable Forced Encryption"选项（默认）
    - 默认 = 0
#
"OF_DISABLE_DM_VERITY_FORCED_ENCRYPTION"; [新]
    - 设置为1以在新安装时**勾选**OrangeFox "Disable DM-Verity"和"Disable Forced Encryption"选项（默认）
    - 默认 = 0
#
"OF_CLASSIC_LEDS_FUNCTION"
    - 设置为1以使用旧版R9.x Leds功能
    - 默认 = 0
#
"OF_SKIP_FBE_DECRYPTION" 
    - 设置为1以跳过FBE解密例程（防止在Fox标志或Redmi/Mi标志处挂起）
    - 默认 = 0
#
"OF_SKIP_FBE_DECRYPTION_SDKVERSION" [新]
    - 此选项允许您指示OrangeFox避免尝试解密具有特定Android SDK/API级别（及更高）的ROM
    - 设置为应跳过的Android SDK/API级别。您需要确保提供的值是合理且正确的
    - 每当触发跳过代码时，将在日志屏幕上打印警告，并且不会尝试解密分区数据
    - 例如："export OF_SKIP_FBE_DECRYPTION_SDKVERSION=31"（即，不要尝试解密Android 12（SDK/API 31）或更高版本）
    - 有效的SDK/API级别数字可以在："https://source.android.com/setup/start/build-numbers"找到
    - 注意：如果您正在使用此变量，请勿使用"OF_SKIP_FBE_DECRYPTION"，因为它将取代此变量
    - 默认 = 无（即尝试解密所有Android API级别）
#
"OF_OTA_RES_DECRYPT"
    - 设置为1以尝试在MIUI OTA恢复期间解密内部存储（而不是因错误退出）
    - 默认 = 0
#
"OF_NO_MIUI_OTA_VENDOR_BACKUP"
    - 设置为1以防止在准备增量MIUI OTA时备份vendor_image（在刷入MIUI ROM时）
    - 对于新的（2019年及以后的）小米设备（例如，lavender, violet, raphael, ginkgo等）需要此备份
    - 默认 = 0
#
"OF_NO_RELOAD_AFTER_DECRYPTION"
    - 设置为1以防止OrangeFox在解密后重新运行启动过程
    - 默认 = 0
#
"OF_NO_TREBLE_COMPATIBILITY_CHECK"
    - 设置为1以禁用检查ROM中的compatibility.zip
    - 默认 = 0
#
"FOX_USE_TWRP_RECOVERY_IMAGE_BUILDER"
    - 设置为1以使用官方TWRP构建系统的工具来构建恢复映像
    - 目前推荐设置此选项为1
    - 默认 = 0
#
"FOX_RESET_SETTINGS"
    - 设置为"disabled"以在安装OrangeFox zip后**不**重置OrangeFox设置为默认值
    - 不建议禁用此选项
    - 默认 = 1
#
"FOX_DELETE_AROMAFM"
    - 设置为1以从zip安装程序中删除AromaFM（对于不工作的设备）
    - 默认 = 0
#
"OF_USE_HEXDUMP"
    - 设置为1以使用外部hexdump工具获取文件头信息（而不是内部代码）
    - 默认 = 0
#
"OF_USE_GREEN_LED"
    - 设置为0以移除"绿色LED"设置
    - 默认 = 1
#
"OF_FLASHLIGHT_ENABLE"
   - 是否启用闪光灯功能
   - 默认为"1"
#
"OF_FL_PATH1" 和 "OF_FL_PATH2"
   - 设置自定义闪光灯路径（如果闪光灯不工作）
   - 例如. OF_FL_PATH1="/sys/class/leds/led_torch_2"
#
"FOX_VERSION"
   - 发布版本号
#
"OF_MAINTAINER"
   - 维护者的名字
#
"OF_MAINTAINER_AVATAR" [新]
 - 维护者头像的完整路径，将在关于页面显示
 - 图像应为32位PNG，192x192像素
 - 图像大小应尽可能小（约50kb），使用"advdef -4z xxx.png"压缩图像
 - 不建议在头像中使用不适宜的内容
 - 如果您为屏幕分区较小的手机构建，不推荐使用
 - 例如 'export OF_MAINTAINER_AVATAR="/home/yillie/avatar.png"'
 - 默认 = 无
#
"OF_SCREEN_H"
   - 如果您的屏幕宽高比不是16:9，请使用此选项
   - 要计算OF_SCREEN_H，如果您的屏幕宽度不是1080，请使用以下公式：<宽高比高度>*120
   - 例如，如果宽高比是19:9，则使用19*120 (=2280)
   - 使用"export OF_SCREEN_H=2280"来调整恢复到19:9屏幕
   - 默认 = 1920
#
"OF_STATUS_H"
   - 仅当设备有刘海时使用此选项
   - 您不需要从OF_SCREEN_H中添加或减去OF_STATUS_H
   - 使用"export OF_STATUS_H=144"来改变状态栏大小为144
   - 小贴士：在Android中截屏，并使用任何图形编辑器（例如MSPaint）计算状态栏高度
   - 默认 = 72
#
"OF_STATUS_INDENT_LEFT" 和 "OF_STATUS_INDENT_RIGHT"
   - 当设备有圆角时使用OF_STATUS_INDENT_LEFT和OF_STATUS_INDENT_RIGHT
   - 这些变量的推荐设置为48（当设备有圆角时）
   - 默认 = 20
# 
"OF_HIDE_NOTCH"
   - 添加一个设置选项，使状态栏变黑（隐藏刘海）
   - 默认 = 0
# 
"OF_CLOCK_POS"
   - 用于控制具有刘海的设备的时钟位置选项
   - 0 => 左侧、中间和右侧的时钟位置都可用
   - 1 => 仅左侧和右侧的时钟位置可用
   - 2 => 仅左侧的时钟位置可用
   - 默认 = 0
# 
"OF_ALLOW_DISABLE_NAVBAR"
   - 如果设备没有硬件导航按钮，请设置为0
   - 如果OF_ALLOW_DISABLE_NAVBAR设置为0，用户将无法隐藏屏幕上的导航栏
   - 默认 = 1
# 
"OF_DONT_KEEP_LOG_HISTORY" [新]
   - 将在/sdcard/Fox/logs/中保存带有时间戳的recovery.log副本（.zip格式）
   - 这意味着之前保存的恢复日志不会被覆盖
   - 这些将以.zip格式保存；用户可能需要定期清除它们
   - 启用此功能以关闭此特性（意味着每次重启恢复时lastrecoverylog.log将被覆盖）
   - 默认 = 0
#
"FOX_BUGGED_AOSP_ARB_WORKAROUND" [新]
 - 使用此选项设置一个旧的UTC时间，以绕过一些ROM中声称的反向升级保护问题
 - 例如，export FOX_BUGGED_AOSP_ARB_WORKAROUND="1546300800" [2019年1月1日 00:00:00 GMT]
 -      export FOX_BUGGED_AOSP_ARB_WORKAROUND="1420041600" [2014年12月31日 16:00:00 GMT]
 - 默认 = 无
#
"FOX_REPLACE_BOOTIMAGE_DATE" [新]
 - FOX_BUGGED_AOSP_ARB_WORKAROUND不替换ro.bootimage.build.date.utc
 - 设置此选项为1以替换bootimage时间
 - 默认 = 0
#
"TARGET_DEVICE_ALT" [新]
 - 如果设备有多个代码名，请使用此选项，以便OrangeFox zip安装器可以支持备用代码名，而不会直接失败
 - 例如，export TARGET_DEVICE_ALT="kate"（对于kenzo/kate）
 -      export TARGET_DEVICE_ALT="willow"（对于ginkgo/willow）
 -      export TARGET_DEVICE_ALT="blue, green, yellow, orange"（对于多个备用设备）
 - 默认 = 无
#
"OF_TARGET_DEVICES" [新]
- 如果设备有多个代码名，但ROM和其他zip安装器从不检查所有代码名，因此总是导致"E3004错误7"问题（例如，raphael/raphaelin）
- 此选项的作用是让OrangeFox临时切换设备名称，以防止错误7的发生
- 注意，这是一个临时解决方案，仅持续到恢复重启之前。
- 您应该列出设备的所有有效代码名。
- 例如，export OF_TARGET_DEVICES="raphaelin,raphael"
-
- 注意：此变量的目的与TARGET_DEVICE_ALT（仅影响OrangeFox zip安装器的创建）非常不同。此变量实际上影响了使用OrangeFox在刷入zip时的操作。这就是为什么它被映射到一个单独的构建变量的原因。这样做是为了能够在实时恢复会话中触发所有这些影响。只有在所有可能的场景中进行了充分测试后，您才应该部署使用此变量的构建。
#
"OF_SKIP_ORANGEFOX_PROCESS" [新]
 - 使用此选项为1以跳过dm-verity/强制加密/boot映像的修补
 - 如果启用了此选项，则默认也会启用"OF_DONT_PATCH_ON_FRESH_INSTALLATION"
 - 默认 = 0
#
"OF_VANILLA_BUILD" [新]
 - 设置此选项为1以制作一个普通的构建，跳过所有OrangeFox的修补
 - 如果启用了此选项，还会自动启用许多其他变量来禁用各种OrangeFox附加功能，包括上面的"OF_SKIP_ORANGEFOX_PROCESS"（详情请参阅bootable/recovery/Android.mk）
 - 启用此选项后，OrangeFox很可能（甚至很可能）会被stock recovery覆盖，因此用户可能需要刷入某种"fcrypt" zip和/或magisk
 - 默认 = 0
#
"OF_SUPPORT_OZIP_DECRYPTION" [新]
 - 设置此选项为1以启用对Realme oZip解密的支持
 - 除非您清楚自己在做什么，请勿使用此选项（参见下文）
 - 如果启用了此选项，您还必须设置"TW_OZIP_DECRYPT_KEY"
 - 注意：对于搭载Android 10或更高版本发布的realme设备，由于realme已在updater二进制文件中添加了解密器，因此不需要外部解密器。对于升级到Android 10的设备，此功能仅在降级到pie或以下版本时需要。
 - 默认 = 0
#
"FOX_REMOVE_AAPT" [新]
 - 设置此选项为1以从构建中移除aapt二进制文件
 - 此二进制文件大小为1.7mb，使用此变量有助于减小恢复的大小
 - 默认 = 0
#
"FOX_DRASTIC_SIZE_REDUCTION" [新] [实验性!]
 - 设置此选项为1以尝试大幅度减小恢复映像的大小
 - 这将从构建中移除一些大文件（包括一些字体）；如果在恢复的调试屏幕看到资源错误消息，不必担心
 - 每次启用此变量构建时，您必须始终进行干净构建
 - 默认 = 0
#
"FOX_EXTRE


https://gitlab.com/OrangeFox/vendor/recovery/-/blob/master/orangefox_build_vars.txt