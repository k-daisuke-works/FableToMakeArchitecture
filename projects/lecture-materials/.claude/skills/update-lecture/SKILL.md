---
name: update-lecture
description: 既存の講義資料やマスターテンプレートを最新の生成AI動向に更新するとき。「前回の資料を最新情報に更新して」「この資料、内容が古くなってないか見て」という依頼で使う。
---

対象: $ARGUMENTS

1. 対象を特定する(`lectures/<日付-テーマ>/` の講義一式、または `masters/base-outline.md`)。
   $ARGUMENTS は `tools/request-form.html` が生成した要件定義書(【対象の資料】…形式)の
   場合もある。指定がなければ `lectures/` の最新ディレクトリを対象とし、その旨を報告に含める。
2. 対象の outline / slides / script を読み、事実の主張(モデル名・機能・料金・統計・規制)を
   洗い出す。
3. WebSearch で 1 点ずつ現在の情報と照合し、古くなった箇所を差分表にまとめる:
   「箇所(ファイル:見出し)/ 現記述 / 最新情報 / 出典 URL・参照日」。
4. 差分を自律で反映する。**outline → slides → script の順**に直し、三者の整合を保つ
   (スライドだけ直して台本が古いまま、を防ぐ)。出典・参照日も更新する。
5. 更新履歴(日付・変更点の要約)を追記する。講義一式が対象なら `outline.md` 末尾、
   `masters/base-outline.md` が対象ならその「更新履歴」節に書く。
6. 講義一式が対象なら `/publish-check` を実行する。masters 単体が対象なら
   `/publish-check` は使わず、masters のパスと `.claude/skills/publish-check/checklist.md`
   を添えて content-reviewer に直接レビューさせる。差分表・チェック結果を報告する。

## 注意

- `masters/base-outline.md` を対象にする場合は、反映前に差分表を提示して
  **人間の承認を得てから**書き換える(CLAUDE.md のディレクトリ規約)。
- 「変わっていないこと」の確認も価値がある。照合して現状維持と判断した項目も
  差分表に「変更なし(確認日)」として残す。
