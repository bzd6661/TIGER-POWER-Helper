# TIGER POWER 本机助手

## 这是什么

[tigerpower.online](https://tigerpower.online) 的本机钱包助手:装在你自己的电脑上,让网站能用你自己的 **币安 Agentic Wallet**(`baw` 命令行)下单。

**第一次用,跟着图文教程走一遍最快:<https://tigerpower.online/guide>**(从下载、启动、扫码登录到连上网站,每一步都有截图)。

- 本机页面 <http://127.0.0.1:4400> 只有这几样:钱包扫码登录、网站连接时的 6 位配对码、网站请求的批准框、已连接的网站(可以一键断开)。
- 买卖、持仓、设置、自动转出、跟单、转账都在网站上操作。网站一直在更新,你不用跟着更新助手。
- 网站发来的动钱 / 放松限制的请求 —— 转账、打赏、改设置 / 改网页限额、设置或打开自动转出、开真实跟单 / 恢复跟单、解除地址锁定 —— 要在本机助手页面点 **「批准」** 才执行;点「拒绝」或 120 秒没人点就不执行。
- 网站买入有额度:默认单笔最多 0.5 BNB(每个号分别算)、每天合计最多 2 BNB;在网站上改额度也要本机批准。

想在本机直接扫链、看 K 线、下单,用完整版 [TIGER-POWER-Terminal](https://github.com/bzd6661/TIGER-POWER-Terminal):它已经包含本机助手,装了完整版就不用再装这个。

## 安装(Windows)

1. **装 Node.js 20 以上**:<https://nodejs.org>(一路下一步)
2. **下载这个仓库**,放在一个固定的文件夹:
   - 不用 git:下载压缩包 <https://github.com/bzd6661/TIGER-POWER-Helper/archive/refs/heads/main.zip> 解压;
   - 用 git:`git clone https://github.com/bzd6661/TIGER-POWER-Helper.git`
3. **双击根目录的 `start.cmd`**。第一次会自动:检查 Node 版本 → 安装依赖(`npm install`)→ 没装币安钱包命令行(baw)就问你要不要装(`npm i -g @binance/agentic-wallet`)→ 在后台启动助手 → 打开 <http://127.0.0.1:4400>。

助手在后台运行(没有窗口),cmd 窗口可以关掉;崩了会自动拉起,日志在 `panel/panel.log`。启动不起来时 `start.cmd` 会把日志最后几行打印出来。

**买卖 Gang V2 内盘** 还要装 Gang 官方命令行(其他平台不用):

```bash
npm i -g @gangfun/cli
```

登录前,Binance App 里要准备好:

- 开通 Agentic Wallet,并**绑定 Agent ID**(four Agent 模式 / Gang / Flap 内盘都要求钱包有 Agent ID)
- 钱包设置里打开**开发者模式**(合约调用要用,开一次大约 7 天后要重开)和**允许交易所有代币**
- 「异常交易处理」建议设成 **需要确认**(Gang 内盘卖出、被标风险的转账要用;设成自动拒绝的话这类交易会直接被拒)

可选:把 `.env.example` 复制成 `.env` 再填(代理、自己的节点、Telegram 推送),改完重新双击 `start.cmd`。

## 怎么用

1. **扫码登录**:在本机助手页面 <http://127.0.0.1:4400> 点「扫码登录」,用 Binance App 扫码、在 App 里确认。登录最长 48 小时,过期了再扫一次。
2. **连接网站**:打开 <https://tigerpower.online>,点「连接本机助手」。浏览器问「允许访问本地网络上的设备」时点允许。
3. **填配对码**:本机助手页面会弹出 6 位配对码,填进网站。配对一次就行,之后网站上能看自己的持仓、一键买卖、设置自动转出和跟单。
4. **批准**:网站上转账、改设置这类操作,本机助手页面会弹出批准框,写清楚要做什么(金额、收款地址是不是你自己的等)。看清楚再点「批准」。这个页面要开着才能批准。
5. **断开**:本机助手页面的「已连接的网站」里列出配对过的浏览器,点「断开」后那个浏览器马上就不能再用助手了(它还在等的批准也一起作废);要再用得在网站上重新配对。

## 安全

- 下单、转账都由你电脑上的助手签名;钱包登录只存在你自己的电脑上(账号 1 在 Windows 凭据管理器里,账号 2 在 `baw2/.baw`)。
- 网站服务器上没有任何钱包;网站用不到你的钱包地址和余额,网站的页面也不会把它们发给网站服务器(只有你自己点开某个地址的交易人详情时,这个地址会发给网站服务器查公开成交)。
- 登录二维码只在本机助手页面显示:网站上点登录,只是请本机助手弹出二维码。
- 动钱的请求要你在本机点「批准」;批准框里的文字由助手按校验过的参数自己生成,不用网站给的文字。
- 助手只监听 `127.0.0.1`,只接受配对过的 tigerpower.online 页面;10 分钟内输错 10 次配对码、或有 3 轮配对没配成(过期 / 试太多次 / 被新的一轮顶掉),就锁 15 分钟,这期间不接受新的配对。
- 每笔交易先走币安钱包的预览(链上模拟 + 风控),不通过或有风险提示就不执行;授权只授权这一笔要用的数量。
- 不要把助手放到公网服务器上。

## 第二个账号(可选)

想同时用两个币安账号,要给账号 2 单独装一份 baw(凭证存在 `baw2/.baw`,和账号 1 互不影响):

```bash
cd baw2
npm install
```

然后刷新本机助手页面,就能给账号 2 扫码登录;网站上可以选「账号 2」或「两个号」同时下单。

账号 2 的登录永远只存在 `baw2/.baw` 里、和账号 1 分开(就算 npm install 时装上了 keytar,也不会用 Windows 凭据管理器)。

## 更新

网站更新不用你管,只有助手本身改了才需要更新。助手每 6 小时看一次 GitHub 上的 `version.json`(GitHub 打不开时改用 jsDelivr),有**重大更新**时本机助手页面会提示,双击 `start.cmd` 时也会提醒一句。只读一个公开的版本号文件,不会自动下载或运行任何东西。

- 用 git 下载的:在助手文件夹运行 `git pull`,再重新双击 `start.cmd`。
- 下载压缩包的:下载最新压缩包,解压覆盖到原来的文件夹(`.env`、登录信息和 panel 里的数据文件不在压缩包里,不会被覆盖),再重新双击 `start.cmd`。

不想要提醒:`.env` 里加一行 `AGENTBUY_NO_UPDATE_CHECK=1`。

## 常见问题

### 端口 4400 被占 / 双击 start.cmd 打开的是完整版页面

完整版 TIGER-POWER-Terminal 已经包含本机助手,两个都用 4400 端口,同时只能开一个。装了完整版就直接用完整版连网站,不用再装助手;要换成助手,重启电脑后只双击助手的 `start.cmd`。

### 登录报 Connect Timeout / 连不上币安(中国大陆网络)

登录、下单都要连 `www.binance.com`。中国大陆的普通 DNS 会把它解析到假地址,扫码登录就会报 `Connect Timeout Error`——浏览器能打开币安是因为走了代理或「安全 DNS」,而命令行程序默认两样都不用。

**助手已经自动处理 DNS 污染**:币安的域名改用阿里 / 腾讯的加密 DNS(DoH)解析,多数情况直接就能登录。如果还是连不上(说明连真实地址也被拦了),再开代理:

- **最简单**:打开 Clash / v2rayN 等代理软件的 **「系统代理」**,助手 30 秒内会自动用上,不用重启。
- **或者手动指定**:在助手文件夹的 `.env` 里加一行 `HTTPS_PROXY=http://127.0.0.1:端口`(Clash 一般是 7890,v2rayN 一般是 10809),然后重新双击 `start.cmd`。

双击 `start.cmd` 时会自动测一次能不能连上币安,并告诉你现在走没走代理。

### 怎么停掉本机助手

助手在后台运行(没有窗口):关掉本机助手页面、关掉 cmd 窗口都不会停掉它。它由一个守护进程(`panel/daemon.mjs`)看着,只结束助手本身(`panel/server.mjs`)的话几秒后会被重新拉起来,所以两个都要结束:

- **任务管理器**:按 Ctrl+Shift+Esc →「详细信息」,结束两个 `node.exe`(分不清哪个是助手的:在列标题上右键 →「选择列」勾上「命令行」,看是不是 `daemon.mjs` / `server.mjs`)。电脑上没在跑别的 Node 程序的话,把 `node.exe` 都结束就行。
- **或者在 PowerShell 里运行**(只结束占着 4400 端口的助手,和拉起它的那个守护进程;别的 Node 程序不碰。在 `.env` 里改过 `PANEL_PORT` 的话,把 4400 换成你的端口):

```powershell
$s = Get-NetTCPConnection -LocalPort 4400 -State Listen -ErrorAction SilentlyContinue | Select-Object -First 1 -ExpandProperty OwningProcess
if ($s) { $d = (Get-CimInstance Win32_Process -Filter "ProcessId=$s").ParentProcessId; Get-CimInstance Win32_Process -Filter "ProcessId=$d" | Where-Object { $_.CommandLine -match 'daemon\.mjs' } | ForEach-Object { Stop-Process -Id $_.ProcessId -Force }; Stop-Process -Id $s -Force }
```

停掉以后网站就连不上本机助手了:还能看数据,但持仓、下单、转账这些都用不了。想再用就重新双击 `start.cmd`。重启电脑后助手也不会自己起来(除非你自己把它加进了开机启动),要用时双击 `start.cmd`。

---

本工具只是帮你调用自己钱包的下单接口,不构成任何投资建议。内盘币风险极高,自己判断。
