# SCP 离线计划
> 没有假惺惺的告别。没有留下任何数据。没有任何的反应时间。服务器续费到期，自动关闭。公司破产并彻底注销。Wikidot 不需要赔偿，用户条款里已经把它的责任抛的干干净净。数万个页面就此不翼而飞，像是被泥头车创进异世界那样无影无踪。\
>  -- [新世纪维基农场：终，又名宇宙史大结局](https://scp-wiki-cn.wikidot.com/the-end-of-wikidolion)

SCP 离线计划是一个致力于使用现代互联网存档手段，提供易阅的 SCP Wikidot 完整存档的项目。\
与其他同类项目不同的是，我们侧重于提供一份可直接打开阅览的网站存档，而非每一个页面的 FTML 源代码。

当前项目主要依赖于 [openZIM](https://github.com/openzim)，未来可能会使用更加贴合 Wikidot 实际的方式。

## 如何开始
一般来讲，最终用户不需要关心这里的内容，只需要从[项目主页](https://scp-wiki-cn.wikidot.com/offline)下载即可。\
但如果您想自己制作一份存档，那么请确保拥有足够稳定的网络连接，正确配置分流，以及准备好了 Docker 环境。
```bash
mkdir crawls output
docker compose up -d
```
Browsertrix Crawler 会开始运行，这一过程可能需要几个小时到几天，具体取决于性能和网络环境。\
随后，请安装 [warc2zim](https://github.com/openzim/warc2zim) 并进行转换。
```bash
warc2zim --name "scp-wiki-zh_all" --title "SCP 基金会 中文分部 Wiki" --description "SCP 基金会中文分部数据存档" -u http://scp-wiki-cn.wikidot.com --lang zh --publisher "MCSeekeri"  --output "./output" ./crawls/collections/scp-wiki-zh_all/
```
运行需要一些时间，如果您在远程服务器上运行，推荐使用 `screen` 防止断开连接导致任务中断。

## 已知问题
项目当前主要的问题是 Browsertrix Crawler 总是无法完整抓取页面，有大概率在抓取中卡住，或者因为不明原因停止抓取。\
同时，值得注意的是，所有含有 offset 的页面都无法正常抓取。

openZIM 官方也提供了存档工具 [zimit](https://github.com/openzim/zimit)，但它的本质就是将上面两个流程自动化，任何一个环节出错都会导致运行失败，故不采用。