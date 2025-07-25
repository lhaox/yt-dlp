## 目录结构以及功能说明
该项目目录结构清晰地分为核心代码、文档、脚本、测试和工具等部分：

- `yt_dlp/`：主程序包，包含核心功能模块，如视频下载逻辑（`YoutubeDL.py`）、格式解析、插件、网络处理等。子目录如 `extractor/`、`downloader/`、`postprocessor/` 分别负责解析、下载和后处理。
- `yt_dlp/__main__.py`：程序入口，负责命令行解析和主流程控制。
- `yt-dlp.sh`、`yt-dlp.cmd`：启动脚本，分别用于类 Unix 和 Windows 环境，便于用户直接运行主程序。
- `README.md`、`LICENSE`、`CONTRIBUTING.md` 等：项目说明、许可证和贡献指南，帮助用户了解和参与项目。
- `Makefile`、`pyproject.toml`、`setup.cfg`：构建、依赖和安装配置文件，支持打包和分发。
- `devscripts/`：开发辅助脚本，如自动生成文档、补全脚本、测试工具等，提升开发效率。
- `test/`：测试代码，覆盖各类功能和模块，保证项目稳定性。
- `bundle/`：打包相关脚本和配置，支持如 PyInstaller、Docker 等多种分发方式。
- 其他文件如 `public.key`、`supportedsites.md` 用于安全和支持站点列表。

整体结构便于维护和扩展，核心功能与辅助工具分离，文档和测试齐全，适合开源协作开发。

## 执行流程/模块
`python -m yt_dlp https://www.youtube.com/watch?v=xxxxxx` 时，项目的执行流程大致如下：
1. **入口模块**  
   `-m yt_dlp` 会执行 `yt_dlp` 包下的 `__main__.py` 文件。这里会解析命令行参数（如 URL），并初始化主程序。
2. **参数解析**  
   主模块会调用参数解析器（如 `argparse`），识别你输入的 URL、下载选项等。
3. **信息提取**  
   解析完参数后，主程序会根据 URL 选择合适的 extractor（如 `yt_dlp.extractor.youtube`），负责解析视频页面，获取视频的元数据和下载地址。
4. **格式选择**  
   提取到视频信息后，程序会根据你的参数和默认设置，选择合适的视频/音频格式。
5. **下载器调用**  
   选定格式后，主程序会调用下载器模块（如 `yt_dlp.downloader`），选择合适的下载方式（如 `FFmpegFD`, `Aria2cFD`, `CurlFD` 等），并开始下载。
6. **后处理**  
   下载完成后，可能会进行后处理（如合并音视频、转码、添加元数据），由 `yt_dlp.postprocessor` 模块负责。
7. **输出结果**  
   最终将下载的文件保存到本地，并输出相关信息。

**各模块作用简述：**
- `yt_dlp/__main__.py`：程序入口，参数解析，主流程控制。
- `yt_dlp/extractor/`：负责解析不同网站的视频信息。
- `yt_dlp/downloader/`：负责实际下载文件，支持多种下载工具。
- `yt_dlp/postprocessor/`：下载后的处理，如合并、转码等。
- `yt_dlp/utils.py`：工具函数，辅助各模块工作。

整个流程就是：参数解析 → 信息提取 → 格式选择 → 下载 → 后处理 → 输出。