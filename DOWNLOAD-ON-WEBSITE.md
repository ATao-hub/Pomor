# 把安装包放到官网上供下载

官网的「下载 Windows 安装包」按钮需要填一个**直链**。安装包可以这样放：

---

## 方法一：用 GitHub Releases（推荐）

安装包不占仓库体积，也没有 100MB 单文件限制，适合放 exe。

### 1. 在本机打出安装包

在项目根目录执行：

```bash
npm run dist
```

在 **`release/`** 里会生成 **`Pomor Timer Setup 1.0.0.exe`**（版本号以 `package.json` 的 `version` 为准）。

### 2. 在 GitHub 上发一个 Release 并上传 exe

1. 打开 **https://github.com/wuyanzu03/Pomor**
2. 右侧点 **Releases** → **Create a new release**
3. **Tag**：点 “Choose a tag”，输入 `v1.0.0`，选 “Create new tag”
4. **Release title**：填 `v1.0.0` 或 `1.0.0 发布`
5. **Description**：可写更新说明，也可留空
6. 把本机的 **`release/Pomor Timer Setup 1.0.0.exe`** 拖到 “Attach binaries” 区域上传
7. 点 **Publish release**

### 3. 复制下载链接

发布成功后，在 Release 页面里点 **`Pomor Timer Setup 1.0.0.exe`**，浏览器会开始下载，地址栏就是直链，格式类似：

```
https://github.com/wuyanzu03/Pomor/releases/download/v1.0.0/Pomor.Timer.Setup.1.0.0.exe
```

（实际文件名里的空格可能变成 `.`，以页面显示为准。）**复制这个地址。**

### 4. 填到官网

打开 **`docs/app.js`**，找到 **`DOWNLOAD_URL`**（约第 89 行），把空字符串改成你复制的地址，例如：

```js
const DOWNLOAD_URL = 'https://github.com/wuyanzu03/Pomor/releases/download/v1.0.0/Pomor.Timer.Setup.1.0.0.exe';
```

保存后执行：

```bash
git add docs/app.js
git commit -m "Set download link for installer"
git push
```

等 GitHub Pages 更新后，官网上的「下载 Windows 安装包」就会指向这个安装包。

---

## 方法二：把 exe 放在仓库里（仅当安装包 < 100MB）

若安装包小于 100MB，可以放进仓库，由 GitHub Pages 直接提供下载。

1. 在 **`docs/`** 下新建文件夹 **`download`**
2. 把 **`Pomor Timer Setup 1.0.0.exe`** 放进 **`docs/download/`**
3. 提交并推送：
   ```bash
   git add docs/download
   git commit -m "Add installer for download"
   git push
   ```
4. 在 **`docs/app.js`** 里设置：
   ```js
   const DOWNLOAD_URL = 'https://wuyanzu03.github.io/Pomor/download/Pomor%20Timer%20Setup%201.0.0.exe';
   ```
   （文件名里的空格要写成 `%20`，或改成无空格文件名如 `Pomor-Timer-Setup-1.0.0.exe` 再填）

注意：Git 单文件限制 100MB，若 exe 超过会推送失败，请用方法一（Releases）。

---

## 小结

| 方法 | 做法 | 适用 |
|------|------|------|
| **GitHub Releases** | 发 Release → 上传 exe → 复制下载链接 → 填到 `docs/app.js` 的 `DOWNLOAD_URL` | 推荐，无大小顾虑 |
| **放仓库** | exe 放到 `docs/download/`，`DOWNLOAD_URL` 填 Pages 地址 | 仅当 exe < 100MB |

填好 **`DOWNLOAD_URL`** 并推送后，官网上的按钮就会变成可用的下载链接。
