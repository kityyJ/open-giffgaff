# giffgaff 保号助手

一个轻量的静态网页，用于通过浏览器下载少量测试数据，并在本地保存操作记录。适合希望定期检查 giffgaff SIM 移动数据连接的用户。

> **重要：** 本工具仅确认浏览器是否完成数据下载，不能查询 SIM 状态，也不能保证下载行为已被 giffgaff 记录为有效保号活动。

## 功能

- 单次请求下载约 **126 KB** 测试数据
- 为请求添加随机参数，并使用 `cache: "no-store"` 降低缓存影响
- 显示下载结果、响应大小和耗时
- 本地最多保存 **20 条**记录，页面显示最近 **5 条**
- 无需登录、无需构建工具，适配手机和桌面浏览器

## 使用方法

1. **确认 SIM 与资费。** 确保 giffgaff SIM 已激活，可以使用移动数据，并事先了解当前套餐、余额或漫游费用。
2. **切换到对应移动网络。** 关闭 Wi-Fi；如果使用双 SIM，检查手机的“移动数据”所选线路是 giffgaff。境外使用时，确认漫游是否可用及其收费标准。
3. **开始下载。** 使用手机浏览器打开部署好的页面，点击 **「开始下载」**。出现「下载完成」后，可查看收到的数据量和耗时。
4. **核对官方记录。** 需要时登录 [My giffgaff](https://www.giffgaff.com/dashboard) 检查余额及使用情况；完成后可关闭移动数据，避免额外消耗。

**注意：** 网页本身及其他后台服务也可能产生流量。页面显示的数据大小不是运营商最终计费流量；不要把「下载完成」理解为「保号成功」。

## giffgaff 保号规则

根据 giffgaff 官方帮助说明，SIM **连续 6 个月没有符合条件的使用**，可能被停用。官方列举的有效活动包括：

- 使用 SIM 向其他号码拨出符合条件的电话，或发送短信 / 彩信
- 使用 SIM 的**移动数据连接互联网**
- 购买 Airtime Credit 或套餐

建议在期限前主动完成并核实一次有效活动，不要等到最后一天。具体适用条件以 [giffgaff 官方停用规则](https://help.giffgaff.com/en/articles/242797-understanding-why-your-number-has-been-deactivated) 为准。

## 部署

项目是纯静态网页，部署时需要将以下两个文件放在**同一目录**：

```text
open-giffgaff/
├── index.html
└── payload.txt
```

### GitHub Pages

1. Fork 或复制本仓库到自己的 GitHub 账户。
2. 打开仓库 **Settings → Pages**。
3. 在 **Build and deployment** 中选择 **Deploy from a branch**，设置分支为 `main`、目录为 `/ (root)` 并保存。
4. 部署完成后，通过 GitHub Pages 提供的地址访问页面。

对于当前仓库，若已启用 Pages，预期地址为：
`https://kityyJ.github.io/open-giffgaff/`

### Cloudflare Pages

1. 在 Cloudflare Pages 中连接此 GitHub 仓库，或上传静态文件。
2. 无需安装依赖和构建命令；使用根目录 `.` 作为静态文件输出目录。
3. 部署后确认 `index.html` 能访问，并且同路径下的 `payload.txt` 能正常返回。

本地直接双击打开 `index.html` 可能受浏览器 `file://` 限制，建议通过 HTTP 静态服务或部署后的网址测试。

## 常见问题

**为什么已下载，却没有保号记录？**

网页只知道浏览器收到文件，不知道数据实际通过哪张 SIM 传输，也无法读取 giffgaff 的运营商活动记录。请先确认已关闭 Wi-Fi、使用的是 giffgaff 数据线路，再到官方账户查看。

**每次需要多少钱？**

费用不固定。英国本地套餐、按量计费及境外漫游可能采用不同资费；还应考虑网页加载和后台流量。尤其在英国以外使用时，应先查看 [官方漫游说明](https://help.giffgaff.com/en/articles/229548-using-your-mobile-phone-in-the-rest-of-the-world)，不要假设只花几便士。

**下载失败怎么办？**

检查 SIM 是否已激活、移动数据或漫游是否开启，以及部署地址是否能访问 `payload.txt`。如果看到 HTTP 404，优先检查两个文件是否位于同一部署目录。

**历史记录保存在哪里？**

记录保存在当前浏览器的 `localStorage`（键名 `ggHistory`）。不会自动同步到其他设备；清除浏览器站点数据可能同时删除记录。

## 技术说明

- **文件大小：** 仓库中的 `payload.txt` 为 **128,684 字节**（约 125.7 KiB）。页面约显示 126 KB。
- **请求策略：** 每次请求为文件地址追加时间戳和随机参数，并设置 `cache: "no-store"`。
- **流量统计：** 页面显示收到的文件大小，不等于网络传输字节数或运营商计费数据。
- **隐私：** 页面未集成账户登录或第三方分析脚本；历史记录只写入浏览器本地。托管平台仍可能按照自身政策保存访问日志。
- **许可证：** 当前仓库未包含独立的 `LICENSE` 文件；如需对外明确授权，应补充正式许可证。

## 官方参考

- [SIM 停用规则与有效保号活动](https://help.giffgaff.com/en/articles/242797-understanding-why-your-number-has-been-deactivated)
- [查看用量和消费记录](https://help.giffgaff.com/en/articles/258872-guide-to-the-usage-statement)
- [境外漫游说明](https://help.giffgaff.com/en/articles/229548-using-your-mobile-phone-in-the-rest-of-the-world)

---

独立社区工具，非 giffgaff 官方产品。运营商规则和资费可能变更，请以 giffgaff 公布的信息为准。
