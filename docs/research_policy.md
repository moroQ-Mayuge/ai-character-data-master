# AIキャラクターデータ調査方針

## 1. 目的

本リポジトリは、Illustrious系画像生成モデルで利用しやすいキャラクターマスターデータを整備するための調査・正規化リポジトリである。

主な目的は以下とする。

- 中国語で記載されたアニメ・ゲーム等のキャラクター名を、日本で一般的に使われる正式名称へ確認・正規化する。
- Danbooru系キャラクタータグを確認する。
- 原作アニメ名、ゲーム名、漫画名、VTuber所属等の作品・出典情報を確認する。
- `character-master-data` 側の `docs/character_gacha_spec.md` の「10. キャラ情報フォーマット」に沿って、キャラクター情報を整理する。
- 将来的に JSON、CSV、画像生成タグ、ガチャ用キャラ情報へ変換できるマスターデータを作成する。

## 2. 調査用原本

調査用原本CSVは以下のパスに配置する。

```text
data/org_charlist.csv
```

想定カラムは以下とする。

```csv
index,source_name,danbooru_tag
```

- `index`: 原本の行番号または通番。
- `source_name`: 中国語・混在表記などの元キャラクター名。
- `danbooru_tag`: Danbooruで使われる英字キャラクタータグ、または調査候補タグ。

原本は可能な限り加工せず保存する。調査・変換結果は別ファイルに出力し、`org_charlist.csv` を直接上書きしない。

## 3. 調査時に確認する項目

各キャラクターについて、最低限以下を確認する。

- 元キャラクター名
- 日本語正式名
- 英語名・ローマ字名
- Danbooru正式キャラクタータグ
- 原作作品名
- 原作作品の日本語表記
- 作品種別
  - anime
  - game
  - manga
  - light_novel
  - vtuber
  - original
  - other
- キャラクター情報フォーマット各項目に対応する内容
- 参照URL
- 確信度
- 要確認理由

## 4. キャラ情報フォーマット準拠

キャラクター情報は、`character-master-data` の `docs/character_gacha_spec.md` にある「10. キャラ情報フォーマット」に準拠する。

準拠ルールは以下とする。

- 必ず `キャラ情報：` から開始する。
- 改行禁止。
- 一行のみ。
- 項目追加禁止。
- 項目順変更禁止。
- 各項目は `/` 区切り。
- 性的特徴・性的表現は記載しない。
- 身体情報は画像化可能で全年齢向けの自然な情報のみとする。

ただし、調査用の内部データでは、作品名・出典・タグ・参照URLなどを別フィールドとして保持してよい。

## 5. 正規化データの基本構造

調査結果は、1キャラクター1JSONを基本とする。

```json
{
  "id": "",
  "source": {
    "source_index": "",
    "source_name": "",
    "source_tag": ""
  },
  "names": {
    "ja": "",
    "en": "",
    "zh": "",
    "aliases": []
  },
  "work": {
    "title_ja": "",
    "title_original": "",
    "title_en": "",
    "type": "",
    "franchise": ""
  },
  "danbooru": {
    "character_tag": "",
    "copyright_tag": "",
    "other_related_tags": []
  },
  "character_info_fields": {
    "本名": "",
    "あだ名": "",
    "職業": "",
    "年齢": "",
    "種族": "",
    "属性": "",
    "容姿特徴": "",
    "目色形状": "",
    "髪型髪色": "",
    "体型": "",
    "胸大きさ形": "",
    "身体特徴": "",
    "一人称自分呼方": "",
    "あなたを": "",
    "他人を": "",
    "性格": "",
    "口調": "",
    "語尾": "",
    "行動原理": "",
    "対人傾向": "",
    "癖口癖": "",
    "好物": "",
    "苦手": "",
    "服装": "",
    "装備": "",
    "スキル技能": "",
    "特殊スキル": "",
    "弱点": "",
    "秘密": "",
    "備考": ""
  },
  "character_info_line": "",
  "sources": [],
  "confidence": "low",
  "status": "unprocessed"
}
```

## 6. 調査ステータス

`status` は以下を使用する。

- `unprocessed`: 未処理。
- `confirmed`: 日本語名、作品名、Danbooruタグを十分な根拠で確認済み。
- `needs_review`: 候補はあるが曖昧さが残る。
- `unresolved`: 特定できない。
- `skipped`: 調査対象外、または安全・方針上扱わない。

## 7. 確信度

`confidence` は以下を使用する。

- `high`: 公式情報、Danbooruタグ、複数の信頼できる情報源が一致する。
- `medium`: 主要情報源でおおむね一致するが、別名・衣装差分・作品違いの可能性がある。
- `low`: 推定候補に留まる。変換結果として確定しない。

確信度が低い場合、元名称を無理に変更しない。

## 8. Web調査・クロール配慮

各サイトに迷惑をかけないよう、Web調査は低頻度・必要最小限で行う。

- 1回の自動タスクで処理する件数は最大30件とする。
- 1時間に1回程度の低頻度実行を基本とする。
- 同一サイトへの連続アクセスは避ける。
- 各キャラクター確認に必要なページのみ参照する。
- 大量クローリング、網羅取得、スクレイピング目的の取得は行わない。
- robots.txt、利用規約、アクセス制限に反する取得は行わない。
- 公式サイト、日本語公式、Danbooru、Wikipedia、Fandom等の既知情報源を必要に応じて参照する。
- 不要な再取得を避けるため、参照URLと確認結果を保存する。

## 9. 自動タスク方針

定期実行する場合、1回の処理は以下とする。

1. `data/progress.json` を読み込み、現在の進捗を確認する。
2. `data/org_charlist.csv` から未処理位置以降を最大30件だけ取得する。
3. 日本語名、作品名、Danbooruタグ、キャラ情報項目を調査する。
4. 根拠URL、確信度、ステータスを記録する。
5. 確信度が低いものは `needs_review` または `unresolved` にする。
6. 結果ファイル、参照ログ、進捗ファイルを更新する。
7. `data/run_history.jsonl` に実行履歴を追記する。
8. GitHubへコミットする。
9. コミット後に `data/progress.json` と該当結果ファイルを再取得し、実際に更新されたことを確認する。

## 10. 実行履歴ログ

各自動実行の結果は `data/run_history.jsonl` に1実行1行のJSONL形式で記録する。

最低限、以下の項目を含める。

```json
{
  "run_started_at": "",
  "run_finished_at": "",
  "status": "success/failure/partial",
  "source_file": "data/org_charlist.csv",
  "start_source_index": 0,
  "end_source_index": 0,
  "processed_count": 0,
  "result_file": "",
  "source_log_file": "",
  "progress_file": "data/progress.json",
  "commit_sha": "",
  "verified_after_commit": false,
  "error": ""
}
```

- `status` は `success`、`failure`、`partial` のいずれかとする。
- コミット後検証まで成功した場合のみ `verified_after_commit` を `true` にする。
- 失敗時は `error` に失敗理由を記録する。
- 再実行や途中失敗があっても、履歴行は削除せず追記する。

## 11. 出力候補

将来的な出力先は以下を想定する。

```text
data/org_charlist.csv                 # 調査用原本
data/progress.json                    # 進捗管理
data/run_history.jsonl                # 実行履歴ログ
data/results/                         # 調査済みJSON
data/unresolved.csv                   # 未解決・要確認一覧
data/sources.jsonl                    # 参照URLログ
exports/characters_master.csv          # 集約CSV
exports/characters_master.jsonl        # 集約JSONL
exports/illustrious_tags.csv           # Illustrious向けタグ確認表
```

## 12. 重要方針

- 原本CSVは直接変更しない。
- 調査結果は必ず別ファイルに保存する。
- 推測で確定しない。
- 取り違えるくらいなら未解決にする。
- 性的表現や全年齢向けでない情報はキャラ情報へ入れない。
- キャラ情報フォーマットの項目追加・順序変更は行わない。
