### slint的官方上手
#[build.rs]位于src/外的文件，指导编译器将非rust资源转换，比如.slint
main.rs中进行‘初始界面载入’通过1、slint::include_modules!();
在main函数生成MainWindow的实例。
#[编译]cargo编译报错(feature `edition2024` is required)，要求弃用2024版本rust语言（cargo 1.82.0默认使用2021版）|>要求更新cargo。通过`rustup update stable`完成。清华镜像源加速：powershell中`$env:RUSTUP_DIST_SERVER="https://mirrors.tuna.tsinghua.edu.cn/rustup"` `ls env:`以确认。
#[编译]上一次cargo编译被手动中断，导致`cargo run`触发亮绿色Blocking。通过删除.cargo\\.package-cache解决。手动打断是因为网络慢，更换为“清华源+sparse克隆模式”需要在\USER\\.cargo下增加config.toml并填入配置。
经过漫长20分钟的下载，编译成功。
### 消消看
2025-12-18 00:59:32
https://docs.slint.dev/latest/docs/slint/tutorial/from_one_to_multiple_tiles/