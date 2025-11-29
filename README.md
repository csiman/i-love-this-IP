# i-love-this-IP

# Cloudflare 优选 IP 列表

本仓库用于自动收集、筛选并定时更新 Cloudflare 的优选 IP，以便在自建代理、加速访问 Cloudflare CDN、Workers 以及各类需要高质量回源链路的场景中使用。

## 📌 项目简介

本项目通过脚本定时探测 Cloudflare IP 的连通性、延迟与可用性指标，生成实时可用度较高的优选 IP 列表。更新频率、检测逻辑及筛选标准均按照规范流程执行，确保数据来源可追溯、步骤透明、结果稳定。

## 🔄 自动更新

* **更新方式：** GitHub Actions 定时任务
* **更新频率：** 每日自动更新（如需可自行修改 `cron` 配置）
* **数据来源：** Cloudflare 公布的全球 IP 网段
* **检测步骤：**

  1. 遍历指定网段
  2. 单点连通性测试（ICMP）
  3. RTT 延迟统计
  4. 多次采样后筛选出稳定节点
  5. 将结果按规范格式写入输出文件

## 📊 数据结构示例

```text
# 更新时间：2025-xx-xx xx:xx:xx
# 检测节点数：xxxx
# 可用 IP 数：xxx
# 平均延迟：xx ms
# 数据来源：Cloudflare 公网网段
```

输出文件示例：

```text
104.16.0.1
104.19.45.12
172.67.22.91
```

## 📁 文件结构

```bash
your-repo/
├── ip.txt               # 最新优选 IP 列表
├── scripts/             # 扫描和筛选脚本
├── .github/workflows/   # Actions 配置文件
└── README.md
```

## 🚀 使用方式

可将 `ip.txt` 中的 IP 应用于：

* 自建代理服务（如 sing-box、Xray、Hysteria 等）
* Cloudflare CDN 自定义回源
* Cloudflare Workers 访问优化
* 本地测试或研究用途

示例（以 sing-box 为例）：

```json
{
  "outbounds": [
    {
      "type": "http",
      "server": "<从 ip.txt 中选任意可用 IP>",
      "server_port": 443,
      "tls": {
        "enabled": true,
        "server_name": "your-domain.com"
      }
    }
  ]
}
```

## ⚠️ 注意事项

* 本仓库仅收集公开可用 IP，不包含任何敏感信息。
* 结果的可用性受运营商、地区网络状况影响，建议本地再次验证。
* 如需进行大量扫描，请遵循网络使用规范与 Cloudflare 使用政策。

---

如需扩展检测逻辑、添加地区节点测试、或生成专门格式的输出文件，可提交 Issue 或 PR。
