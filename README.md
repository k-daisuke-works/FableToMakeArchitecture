# FableToMakeArchitecture

指示した開発内容に応じて、プロジェクト専用の Claude Code エージェント環境
(CLAUDE.md・スキル・サブエージェント・settings.json)を自動設計・構築する
メタエージェントのリポジトリ。

## 使い方

### 方法1: 自由文で指示する

このリポジトリで Claude Code を起動し、作りたいものを伝えるだけ:

```
ECサイトを開発するエージェント設計をして
```

または明示的にスキルを呼ぶ:

```
/design-agents 社内向け経費精算SaaSの開発体制
```

### 方法2: 要件定義フォームを使う(推奨)

`tools/requirements-form.html` をブラウザで開き、フォームを埋めて「指示文を生成」→
コピーした指示文を Claude Code に貼り付ける。自由文より要件の漏れが減り、
確認の往復が少なくなる。

### 生成されるもの

`projects/<プロジェクト名>/` に、そのプロジェクト専用のエージェント環境一式が生成される:

- `.claude/` + `CLAUDE.md` — エージェント環境本体。実際の開発リポジトリにコピーすれば
  すぐ使える(コピー手順は各プロジェクトの README.md に記載される)
- `tools/request-form.html` — **プロジェクト別 要件定義フォーム**。ブラウザで開いて
  項目を埋めると、そのプロジェクトのスキルにそのまま貼り付けられる指示文
  (【ラベル】形式の要件定義書)を生成できる。項目は生成されたスキルの入力に
  合わせて自動で設計される

生成済みの設計を見直したいとき:

```
/review-design projects/<プロジェクト名>
```

## 仕組み

| 構成物 | 役割 |
|---|---|
| `CLAUDE.md` | メタエージェントの動作定義。設計依頼を `/design-agents` へ誘導 |
| `.claude/skills/design-agents/` | 要件分析→設計→構築→レビューのメインワークフロー |
| `.claude/skills/review-design/` | 生成済み環境の監査・改善 |
| `.claude/agents/requirements-analyst.md` | 指示文から要件を構造化 |
| `.claude/agents/architecture-designer.md` | 要件から設計書(生成ファイル仕様)を作成 |
| `.claude/agents/design-reviewer.md` | 生成物を独立した目で監査 |
| `references/` | 設計パターンとファイル形式の知識ベース(生成品質の核) |
| `templates/` | 生成物の雛形(要件定義フォームHTMLは入力部品組み替え式の部品カタログ) |
| `tools/requirements-form.html` | 要件定義フォーム(ブラウザで開いて指示文を生成) |
| `projects/` | 生成されたプロジェクト別環境の置き場 |

## 設計思想

- **最小構成がデフォルト**: スキルやサブエージェントは「ないと困る理由」があるものだけ
  生成する。判断基準は `references/design-patterns.md` にある。
- **知識はリポジトリに置く**: 生成品質を上げたいときは references/ を育てる
  (新しいパターンの追記、スタック別定石の追加)。プロンプトを毎回工夫する必要はない。
- **独立レビューを必ず通す**: 生成物は design-reviewer の監査を経てから納品される。
