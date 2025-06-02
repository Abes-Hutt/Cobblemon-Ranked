**其他语言版本: [English](README.md)，[中文](README_zh.md)。**

---

# 📊 CobblemonRanked 排位系统说明文档

## 🎯 功能总览

- ✅ 内置中英文消息切换，可另外添加其他语言支持
- ✅ 支持配置多个对战场地位置（匹配成功后自动传送，战斗结束后返回原位置）
- ✅ 自定义段位名称和 Elo 分数阈值，支持任意增删段位
- ✅ 支持 1v1 与 2v2（车轮战）两种战斗模式
- ✅ 使用 Elo 排名系统，每种模式独立计算 Elo
- ✅ 奖励系统按模式独立记录，并可自定义指令奖励
- ✅ 内置赛季功能（自动轮换、奖励清空）
- ✅ 匹配队列系统，支持 Elo 区间限制与等待时间放宽机制
- ✅ 玩家逃跑或断线会双倍扣分，对手不加分
- ✅ 2v2 自由组队功能
- ✅ 支持点击式 GUI 图形界面操作
- [ ] 双打模式

---

## 📌 指令总览

> 以下所有命令均以 `/rank` 开头

| 指令 | 功能描述 |
|------|----------|
| `/rank gui` | 打开主菜单 GUI |
| `/rank gui_top` | 打开排行榜选择界面 |
| `/rank gui_info` | 查看自己详细的 Elo 对战信息 |
| `/rank gui_info_players` | 查看其他玩家的 Elo 信息 |
| `/rank gui_queue` | 快速加入/退出匹配队列 |
| `/rank gui_invite <页码>` | 邀请玩家组成双人队伍（翻页） |
| `/rank gui_reward` | 管理员奖励发放界面 |
| `/rank gui_reward_format <模式>` | 指定模式下发放奖励（如1v1/2v2） |
| `/rank gui_info_format <玩家> <模式>` | 查看某玩家在某模式下的 Elo 信息 |
| `/rank gui_myinfo` | 查看自己所有历史对战信息 |
| `/rank gui_reset` | 管理员专用段位重置 GUI |
| `/rank reset <玩家> <模式>` | 重置玩家 Elo（管理员专用） |
| `/rank reload` | 重新加载配置文件（无需重启服务端） |
| `/rank queue join [模式]` | 加入 Elo 匹配队列（可指定模式） |
| `/rank queue leave` | 退出所有队列 |
| `/rank info <模式> <赛季>` | 查看当前玩家在指定赛季和模式下的数据 |
| `/rank info <玩家> <模式> [赛季]` | 查看其他玩家在某赛季的数据 |
| `/rank top [模式] [赛季] [页码] [数量]` | 分页显示 Elo 排行榜 |
| `/rank season` | 查看当前赛季信息 |
| `/rank season end` | 结束当前赛季（管理员专用） |
| `/rank reward <玩家> <模式> <段位>` | 发放指定段位奖励（管理员） |
| `/rank duo invite <玩家>` | 向他人发送 2v2 组队邀请 |
| `/rank duo leave` | 退出双排队列或解散队伍 |
| `/rank duo accept` | 接受 2v2 邀请 |
| `/rank duo status` | 查看当前 2v2 队伍或匹配状态 |

---

## ⚙️ 配置文件详解 (`cobblemon_ranked.json`)

<details>
<summary>点击展开查看配置详情（含中文注释）</summary>

```json
{
  // 默认语言：zh（中文）或 en（英文）
  "defaultLang": "zh",

  // 默认对战模式：'1v1' 或 '2v2'
  "defaultFormat": "1v1",

  // 最少携带宝可梦数量
  "minTeamSize": 1,

  // 最多携带宝可梦数量
  "maxTeamSize": 6,

  // 匹配 Elo 差上限（超过此差值将无法匹配）
  "maxEloDiff": 200,

  // 最大匹配等待时间（秒）——时间越久 Elo 差越可以放宽
  "maxQueueTime": 300,

  // Elo 放宽倍率最大值（随等待时间线性增长）
  "maxEloMultiplier": 3.0,

  // 每个赛季持续天数
  "seasonDuration": 30,

  // 赛季初始 Elo 分
  "initialElo": 1000,

  // Elo 算法中的 K 系数（影响 Elo 变动幅度）
  "eloKFactor": 32,

  // Elo 最低分限制（不会低于此分数）
  "minElo": 0,

  // 禁止使用的宝可梦（禁用传说/神兽等）
  "bannedPokemon": ["Mewtwo", "Arceus"],

  // 支持的战斗模式
  "allowedFormats": ["1v1", "2v2"],

  // 宝可梦最大等级，0 表示不限制
  "maxLevel": 0,

  // 是否允许队伍中出现重复宝可梦（如两个皮卡丘）
  "allowDuplicateSpecies": false,

  // 匹配后可选的战斗场地列表，每组坐标表示玩家传送点（支持多个场地）
  "battleArenas": [
    {
      "world": "minecraft:overworld",
      "playerPositions": [
        { "x": 0.0, "y": 70.0, "z": 0.0 },
        { "x": 10.0, "y": 70.0, "z": 0.0 },
        { "x": 0.0, "y": 70.0, "z": 10.0 },
        { "x": 10.0, "y": 70.0, "z": 10.0 }
      ]
    },
    {
      "world": "minecraft:overworld",
      "playerPositions": [
        { "x": 100.0, "y": 65.0, "z": 100.0 },
        { "x": 110.0, "y": 65.0, "z": 100.0 },
        { "x": 100.0, "y": 65.0, "z": 110.0 },
        { "x": 110.0, "y": 65.0, "z": 110.0 }
      ]
    }
  ],

  // 段位奖励（每种模式独立配置）
  "rankRewards": {
    "1v1": {
      "青铜": ["give {player} minecraft:apple 5"],
      "白银": ["give {player} minecraft:golden_apple 3"],
      "黄金": [
        "give {player} minecraft:diamond 2",
        "give {player} minecraft:emerald 5"
      ],
      "白金": [
        "give {player} minecraft:diamond_block 1",
        "effect give {player} minecraft:strength 3600 1"
      ],
      "钻石": [
        "give {player} minecraft:netherite_ingot 1",
        "give {player} minecraft:elytra 1"
      ],
      "大师": [
        "give {player} minecraft:netherite_block 2",
        "give {player} minecraft:totem_of_undying 1",
        "effect give {player} minecraft:resistance 7200 2"
      ]
    },
    "2v2": {
      "青铜": ["give {player} minecraft:bread 5"],
      "白银": ["give {player} minecraft:gold_nugget 10"],
      "黄金": ["give {player} minecraft:emerald 1"],
      "白金": ["give {player} minecraft:golden_apple 1"],
      "钻石": ["give {player} minecraft:totem_of_undying 1"],
      "大师": ["give {player} minecraft:netherite_ingot 2"]
    }
  },

  // Elo 段位分数阈值（越高段位越靠前）
  "rankTitles": {
    "3500": "大师",
    "3000": "钻石",
    "2500": "白金",
    "2000": "黄金",
    "1500": "白银",
    "0": "青铜"
  }
}
