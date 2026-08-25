# skill-preflight-check

在動手改程式碼之前，先核對套件／框架／雲端服務的官方最新版本、breaking changes 與 deprecation，避免依賴過期知識導致重工。適用於 Claude Code、opencode 等支援 Agent Skills 的環境。

## 這是什麼

一個文字型技能（Agent Skill）。觸發後，AI 會：

1. **辨識目標** — 從任務中列出實際會用到的套件、CLI 工具、雲端服務或框架
2. **查詢官方資訊** — 搜尋最新版本、changelog、breaking changes、migration guide（以官方來源為準）
3. **比對本地版本** — 讀取 lockfile / 設定檔中鎖定的版本，標記落差
4. **輸出 Preflight Report** — 在開始任務前回報狀態與建議
5. **接續原任務** — Report 是提醒不是關卡，除非有重大 breaking change 才先確認

## 觸發時機

使用者訊息提到具體套件名稱、SDK、雲端服務或框架時主動觸發（例如「幫我升級 Next.js」「串接 Stripe API」），不需明確要求檢查版本。

## 安裝

### opencode

```bash
mkdir -p ~/.config/opencode/skills/preflight-check
curl -o ~/.config/opencode/skills/preflight-check/SKILL.md \
  https://raw.githubusercontent.com/itmanapp/skill-preflight-check/master/preflight-check.md
```

安裝後重啟 opencode 生效。

### Claude Code

```bash
mkdir -p ~/.claude/skills/preflight-check
curl -o ~/.claude/skills/preflight-check/SKILL.md \
  https://raw.githubusercontent.com/itmanapp/skill-preflight-check/master/preflight-check.md
```

### 強制執行（選用）

技能由模型自行判斷觸發，非系統層級強制。想要更強的保證：

- **opencode**：在 `~/.config/opencode/opencode.jsonc` 的全域 `instructions` 注入 MUST 級規則（詳見技能文件末「選用 B」）
- **Claude Code**：加 `SessionStart` hook 注入提醒（詳見技能文件末「選用 A」）

## 授權

MIT
