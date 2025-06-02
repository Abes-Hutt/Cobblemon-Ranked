**其他语言版本: [English](README.md)｜[中文](README_zh.md)**

---

# 📊 CobblemonRanked 排位系统说明文档

💡 *本模组目前仅需安装在服务端。*

---

## 🎯 功能总览

### ✅ 已完成功能

- 内置中英文消息切换，支持添加其他语言
- 可配置多个战场，自动传送与归位
- 支持自定义段位名称与 Elo 阈值，灵活增删
- 支持 单打 和 双打 模式
- Elo 排名系统，各模式独立计算
- 模式独立的奖励系统，支持自定义指令
- 内置赛季系统，自动轮换与奖励重置
- 匹配队列支持 Elo 限制与等待放宽机制
- 掉线正常扣分，计入逃跑次数，对手分数不变
- 提供点击式 GUI 界面操作

### 🔧 未来计划功能

- [ ] 2v2车轮战模式
- [ ] 客户端可视化 GUI
- [ ] 跨服匹配支持
- [ ] 逃跑双倍扣分，对手分数不变

---

## 📌 指令总览

> 以下所有命令均以 `/rank` 开头

---

## 🎮 玩家命令

| 指令 | 功能描述 |
|------|----------|
| `/rank gui` | 打开主菜单 GUI |
| `/rank gui_top` | 打开排行榜选择界面 |
| `/rank gui_info` | 查看自己详细的 Elo 对战信息 |
| `/rank gui_info_players` | 分页查看在线玩家，支持点击查看他人战绩 |
| `/rank gui_myinfo` | 快速查看自己的战绩入口 |
| `/rank gui_queue` | 打开匹配队列加入面板 |
| `/rank gui_reward_format <format>` | 显示该格式的段位奖励可领取项 |
| `/rank gui_info_format <player> <format>` | 以 GUI 形式查看指定玩家指定格式的赛季战绩 |
| `/rank queue join [format]` | 加入匹配队列，可选格式（如 singles 或 doubles） |
| `/rank queue leave` | 退出所有匹配队列 |
| `/rank status` | 查看当前匹配状态（是否正在排队） |
| `/rank info <format> <season>` | 查看自己在指定格式与赛季下的 Elo 信息 |
| `/rank info <player> <format> [season]` | 查看他人在指定格式与赛季下的 Elo 信息 |
| `/rank top` | 查看当前赛季默认格式排行榜（前10名） |
| `/rank top <format> [season] [page] [count]` | 分页查看排行榜，可自定义格式、赛季、页数与数量 |
| `/rank season` | 查看当前赛季信息（开始/结束时间、参与人数等） |

---

## 🛡️ 管理员命令（需要 OP 权限）

| 指令 | 功能描述 |
|------|----------|
| `/rank gui_reward` | 打开段位奖励选择界面 |
| `/rank gui_reset` | 分页展示玩家列表，可点击执行重置战绩操作 |
| `/rank reset <player> <format>` | 重置指定玩家某格式的当前赛季战绩 |
| `/rank reward <player> <format> <rank>` | 向玩家发放指定段位奖励（支持自动提示） |
| `/rank season end` | 强制结束当前赛季 |
| `/rank reload` | 重载插件配置（语言、多段位格式等） |

---

## ⚙️ 配置文件详解 (`cobblemon_ranked.json`)

<details>
<summary>点击展开查看配置详情（含中文注释）</summary>

```json
{
  // 默认语言：zh（中文）或 en（英文）
  "defaultLang": "zh",

  // 默认对战模式：'singles'
  "defaultFormat": "singles",

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
  "allowedFormats": ["singles", "doubles"],

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
        { "x": 10.0, "y": 70.0, "z": 0.0 }
      ]
    },
    {
      "world": "minecraft:overworld",
      "playerPositions": [
        { "x": 100.0, "y": 65.0, "z": 100.0 },
        { "x": 110.0, "y": 65.0, "z": 100.0 }
      ]
    }
  ],

  // 段位奖励（每种模式独立配置）
  "rankRewards": {
    "singles": {
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
    "doubles": {
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
