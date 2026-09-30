# 旅游手账 · 安卓 App（网页套壳版）

把你正在用的网页版「旅游手账」(v22) 直接装进一个安卓 App 里：手机桌面出现「旅游手账」图标，点开就是你现在用的那套东西——**行程时间轴、地图点位、未来 5 天天气、参考、记账、行李、预订、备份全部原样保留**，地图导航照样能一键跳高德 App。

> 本方案是「**套壳**」：不重写代码，用安卓系统的网页容器（WebView）直接加载网页。改动最小、最快上线，界面手感与网页一致。若以后想要更原生流畅的体验，可再做纯原生版（功能一模一样）。

---

## ★ 最快路径：不装 Android Studio，GitHub 一键出包（推荐）

如果你不想下载庞大的 Android Studio，只要把本项目推到 GitHub，就能在云端自动编译出 APK（无需本地装任何环境）：

1. 注册/登录 GitHub（免费）→ 新建一个仓库，比如 `travel-journal-android`
2. 把本文件夹**所有内容**推上去（`git add . && git commit -m "init" && git push`）
3. 进仓库 → 顶部 **Actions** → 找到「构建安卓 APK」工作流 → 点 **Run workflow**（或直接 push 到 `main` 也会自动触发）
4. 等 4~6 分钟，跑出绿色对勾后，右侧 **Artifacts** 下载 `旅游手账-Debug-APK.zip`
5. 解压得到 `app-debug.apk`，手机上点开安装（首次需允许「安装未知来源应用」）

> 项目已自带**完整 Gradle 构建文件 + App 图标**，不必再走下面「新建空项目」那套。
> 你本地若装了 Android Studio，直接 **Open** 这个文件夹也能 Run / Build APK。

---

## 一、你需要准备什么

| 物品 | 说明 |
|---|---|
| 一台电脑（Windows / macOS 均可） | 用来打开项目、打包 |
| 安装 **Android Studio**（免费） | 安卓官方开发工具，用来编译和装手机 |
| 一部安卓手机（安卓 7.0 以上） | 用来装 App 实测；或电脑开安卓模拟器 |

> ⚠️ 沙箱环境没有 Android Studio，所以「编译 / 装手机 / 上架」这几步必须在你自己的电脑上完成。本文档写到每一步。

---

## 二、第一步：安装 Android Studio

1. 打开官网：https://developer.android.com/studio
2. 点「Download Android Studio」→ 选你的系统（Windows / macOS）
3. 双击安装包，一路「下一步 / Next」即可（默认勾选的都保留）
4. 首次打开会让你装 SDK，直接按默认装好（RAM 不够时它会提示，按推荐点就行）

---

## 三、第二步：新建一个空项目（用来生成图标和主题）

> 这一步是为了让项目自带 App 图标、主题等基础文件，省得手写出错。新建完再把我们的文件覆盖进去。

1. 打开 Android Studio → **New Project**（新建项目）
2. 模板选 **Empty Views Activity**（空视图活动）→ 点 Next
3. 填写项目信息：
   - **Name**：`TravelJournal`
   - **Package name（包名）**：**必须填 `com.traveljournal.app`**（和代码一致，否则编译报错）
   - **Language**：选 **Kotlin**
   - **Minimum SDK**：选 **API 24（Android 7.0）**
4. 点 **Finish**，等它自动编译、同步完（底部进度条走完）

---

## 四、第三步：把我们的文件覆盖进去

等项目建好，左边出现文件树。把下面 6 个文件**逐一替换**（找到同名文件 → 全选删除 → 粘贴我们的内容）。

| 我们的文件 | 放到项目的这个位置 |
|---|---|
| `MainActivity.kt` | `app/src/main/java/com/traveljournal/app/MainActivity.kt` |
| `activity_main.xml` | `app/src/main/res/layout/activity_main.xml` |
| `AndroidManifest.xml` | `app/src/main/AndroidManifest.xml` |
| `themes.xml` | `app/src/main/res/values/themes.xml` |
| `strings.xml` | `app/src/main/res/values/strings.xml` |
| `index.html` | `app/src/main/assets/index.html`（**没有 `assets` 文件夹就新建一个**） |

> 新建项目一般没有 `assets` 文件夹：在 `app/src/main/` 上右键 → **New → Directory**，输入 `assets`，把 `index.html` 拖进去即可。
> `index.html` 就是你网页版的完整内容（行程、地图、天气……全在里面），原样加载，不用改。

**覆盖清单（包里已给好的文件）：**

```
travel-journal-android/
├── README.md                                  ← 本文档
└── app/src/main/
    ├── AndroidManifest.xml                     ← 含联网权限、全屏配置
    ├── java/com/traveljournal/app/
    │   └── MainActivity.kt                     ← 安卓壳核心：开 WebView 加载网页
    ├── res/layout/
    │   └── activity_main.xml                   ← 一个全屏 WebView
    ├── res/values/
    │   ├── themes.xml                          ← 全屏无标题栏主题
    │   └── strings.xml                         ← App 名称「旅游手账」
    └── assets/
        └── index.html                          ← 你的网页版 v22（原样）
```

---

## 五、第四步：连接手机或开模拟器

**用真机（推荐）：**
1. 手机：设置 → 关于手机 → 连点「版本号」7 下，开启「开发者模式」
2. 返回 → 系统和更新 → 开发者选项 → 打开 **USB 调试**
3. 数据线连电脑，手机上弹「允许 USB 调试」点允许

**用模拟器：** Android Studio 顶部 **Device Manager** → 新建一个虚拟设备 → 选机型 → 下载系统镜像 → 启动。

---

## 六、第五步：点一下，装进手机

1. Android Studio 顶部中间，设备下拉选你的手机（或模拟器）
2. 点绿色 ▶ **Run**（或菜单 Run → Run 'app'）
3. 稍等几十秒，手机上出现「旅游手账」图标 → 点开即用

恭喜，你的旅游手账已经是安卓 App 了 🎉

---

## 七、第六步：打包成 APK（发给别人装）

1. 菜单 **Build → Build Bundle(s) / APK(s) → Build APK(s)**
2. 右下角点 **locate**，找到 `app-debug.apk`
3. 把这个文件发给别人，手机上点开就能安装（需开启「允许安装未知来源应用」）

> 这是调试版 APK，能直接用。正式上架需要签名版（见下一步）。

---

## 八、第七步：签名 + 上架应用商店

**1）生成签名密钥**
菜单 **Build → Generate Signed Bundle / APK** → 选 **APK** → 点 **Create new…**
- Key store path：选个地方存（记住密码！）
- Key alias / Password：自己设，记好
- Validity：25 年
- 一路 Next → 生成 `app-release.apk`

**2）上架**
- **Google Play**：https://play.google.com/console → 建应用 → 传 release APK/AAB → 填资料 → 提交审核
- **国内商店**（华为/小米/应用宝等）：各自开发者平台注册企业/个人账号 → 传包 → 审核
- 个人账号即可上架工具类 App；若涉及支付/定位等敏感权限，部分商店需补充材料

---

## 九、网页在安卓上的已知注意点（诚实说明）

| 功能 | 安卓表现 | 处理 |
|---|---|---|
| 行程 / 地图 / 天气 / 参考 / 记账 / 行李 / 预订 | 全部正常 | 原样保留 |
| 本地存储（数据不丢） | 正常 | 已开启 `domStorageEnabled` |
| 拍/选照片 | 正常 | 已桥接系统相册（可多选） |
| **备份「导入」** | 正常 | 已修好，能选 `.json` 文件 |
| **备份「导出」** | ⚠️ 可能不弹下载窗 | 安卓 WebView 限制 `Blob 下载`；变通：用「云备份」链接，或把备份内容复制出来存备忘录 |
| 高德导航唤起 | 正常 | 点导航链接自动跳高德 App |
| 外链（如高德申请页） | 正常 | 在当前页打开 |

> 备份导出在安卓上若不便，最稳的办法是用网页里的「云备份」生成链接，或在电脑浏览器里导出 JSON 再传到手机。后续若需要，可加一段原生桥接让安卓直接存文件（见下）。

---

## 十、常见问题

**Q：编译报 `ic_launcher` 找不到？**
A：你新建项目时没用 Empty Views Activity，或包名不是 `com.traveljournal.app`。重做第二步，包名务必一致。

**Q：点 Run 没反应 / 白屏？**
A：确认 `assets/index.html` 已放对位置；手机 USB 调试已开；看 Android Studio 底部 Logcat 红字。

**Q：想改 App 图标？**
A：项目里 `res` → 右键 `mipmap` → **New → Image Asset** → 选你的图片生成。

**Q：网页以后更新了怎么办？**
A：把新的网页内容覆盖到 `assets/index.html`，重新 Run / 打包即可。

**Q：这和微信小程序比，哪个好？**
A：安卓这套能直接跳高德 App 导航，体验更顺；小程序对个人账号限制多（不能跳高德）。想覆盖 iPhone 用户再做小程序或 iOS 版。

---

## 十一、后续可优化（按需）

1. **备份导出原生化**：在 `MainActivity.kt` 用 `addJavascriptInterface` 暴露 `saveFile(name, base64)`，让网页导出时直接写进手机「下载」目录，安卓上也能正常存文件。
2. **纯原生版**：若想要原生地图、原生列表滑动手感，可用 Kotlin/Compose 或 Flutter 重写，功能与网页一致（工作量更大）。
3. **自动更新**：网页放服务器，App 启动时拉最新网页，免重新打包。

---

*本项目为「网页套壳」方案，最快把你现有的旅游手账变成可安装的安卓 App，零重写、全功能保留。*
