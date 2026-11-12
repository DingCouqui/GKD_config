# GKD_config

个人 GKD 订阅，基于已停止维护的官方规则库 `gkd-kit/subscription` v186，并加入本地自定义规则。

## 订阅地址

```
https://raw.githubusercontent.com/DingCouqui/GKD_config/main/dist/gkd.json5
```

GKD → 订阅 → 添加订阅 → 粘贴上面的链接。

> 国内访问 raw.githubusercontent.com 不稳定的话，可以用镜像前缀，例如
> `https://ghfast.top/https://raw.githubusercontent.com/DingCouqui/GKD_config/main/dist/gkd.json5`

## 内容

| 项 | 说明 |
|---|---|
| id | 233 |
| 基础 | gkd-kit/subscription v186（610 应用 / 1371 组规则，2024-05 归档） |
| 自定义 | 哔哩哔哩 · 视频下方卡片广告 |
| 默认开启 | 开屏广告（全局）、全屏广告、局部广告、分段广告、更新提示 |
| 其余 | 默认关闭，按需在「订阅 → 规则类别」里批量打开，或到具体应用里单独开 |

### 自定义规则：哔哩哔哩 · 视频下方卡片广告

视频页下方的推广卡，右上角 ⋮ 的 view-id 是 `more_layout`（推荐卡用的是「更多操作」，不会误伤），
点开屏蔽菜单后选「与视频内容不相关」，卡片消失并上报 B 站。

```json5
{
  key: 0,
  name: '视频下方卡片广告',
  rules: [
    { key: 0, name: '点广告卡片更多', matches: ['[vid="more_layout"]'] },
    { key: 1, name: '点与视频内容不相关', matches: ['[text="与视频内容不相关"]'] },
    { key: 2, name: '点不喜欢此商品', matches: ['[text="不喜欢此商品"]'] },
  ],
}
```

注意：GKD 现在把菜单文案改成了「屏蔽推广 / 不喜欢此商品 / 与视频内容不相关」，
旧规则里的「不感兴趣」已经不存在，这也是官方那条规则默认关闭的原因。

### 自定义规则：知乎 · 相关搜索下方、评论上方广告

回答页里「相关搜索」和「评论」之间插的推广卡。关闭按钮是个 69×71 的无名 Image，
紧挨在「XX 的广告」这个标签**前面**，所以用标签当锚点反查它前面的叶子节点。

```json5
{
  key: 0,
  name: '相关搜索下-评论上 广告',
  rules: [{
    key: 0,
    name: '点广告卡右上角X',
    matches: [
      '[text$="的广告"] - @[childCount=0][width<150]',
      '[text$="的广告"] - @[childCount=0]',
    ],
  }],
}
```

注意：旧规则写的是 `[text="广告"]`，而知乎现在是「无期迷途 的广告」「跃动小子 的广告」，
所以老规则一条都命中不了。

## 更新

改完规则后：

1. 提高 `dist/gkd.json5` 里的 `version`（同时改 `dist/gkd.version.json`）
2. 提交并推送
3. GKD 订阅页下拉检查更新

## 免责

规则来自公开仓库，仅供个人使用；因规则误触造成的任何问题自行承担。
原规则库版权归 [gkd-kit/subscription](https://github.com/gkd-kit/subscription) 作者所有。
