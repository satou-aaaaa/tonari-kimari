# AGENTS.md

となりのきまり：となりの家・土地とのもめごとの場面を見て、民法の相隣関係の決まりどおりの答え（枝と根・何cmはなす・費用・通る・水）を選ぶ1日5問の学習ゲーム。正解したら「これは、どの決まり？」も選ぶ。`index.html` と `data.js`（問題の種類）を GitHub Pages で公開する。

## 準備（最初に一度）

```bash
bash scripts/setup.sh   # Node.js（版は .nvmrc）を確かめ、検査の依存とブラウザを入れる。何度実行してもよい
```

Claude Code のクラウドセッションでは `.claude/settings.json` のフックが自動で実行する。

## push する前の確認

```bash
node qa/run.mjs   # 問題（22種類×3000問：選び肢・答えは1つ・答えが別に書いた解き直しどおり・出典）・今日の5問と上級（3年分）・とことん・通しで遊ぶ・続きから・2段目・復習・幅320px・PWA・引き継ぎ・見やすさ（CI の QA と同じ）
```

## このリポジトリの決まり

- 中身は e-Gov 法令検索の民法 第209〜238条（相隣関係）で確かめたことだけ（2026-11-17 に読んだ。要点 `qa/run.mjs` の resolve() の上）。距離・費用の問題は場面の数から計算して答えを決める。種類を足すときは `qa/run.mjs` の `resolve()`（別に書いた解き直し）にも足す。法律が改正されたら条文を読み直す。
- 人の名前・金額・広さは作り話（Aさん〜Dさん）。2023年3月までの古い決まり・建築基準法・地域の慣習・一人ずつの事情の判断は答えにしない（QA が見る）。
- 法律の助言にしない：遊び方と「このゲームについて」と README に、法律の助言・公式の情報の代わりではないこと、自分の場合は法テラスなどで確かめることを置く（QA が見る）。部屋を借りるきまり（chintai-kimari）・用途地域あて（youto-chiiki）と重ねない（こちらは土地の持ち主どうしの相隣関係だけ）。
- 問題は、やさしいが選び肢3つ、ふつう・むずかしいが4つ。2回目のヒントはまちがいを消して2つにしぼり、要点（`key`）も見せる。2段目は「これは、どの決まり？」の5つ（`grp`）。
- 今日の5問（やさしい2・ふつう2・むずかしい1、種類は重ねない）・上級（むずかしい3、今日のむずかしいと別）は日本時間の日付の種で決まり、だれでも同じ。`Math.random()` は練習・とことんの種だけ。乱数は `sort` の比較の中で引かない。`KINDS`・言い回しの並びは最後に足すだけ（QA の `PIN` が第1回の1問目を見る）。`data.js` を変える PR は自動ではマージされない（`hold_paths`）。
- ★はまちがい＋ヒントの数（0で★3・1で★2・2以上で★1）。上級はヒントなし。時間制限なし。
- 好きなだけ遊べる：チケット・ライフ・回数制限・🔒で開くモードを入れない（shared-workflows `routines/game-dev.md` F1〜F7。QA が見る）。とことん（`runs.e`）はまちがえても続き、「ここで降りる」で終わる。
- 保存の読み直し（2つの写しを合わせる）は共通部品 `GameStore`（index.html の `store:begin`〜`end`。shared-workflows `template/store/store.js` の写しで、ここでは直さない）。
- 1問を深くする：`depth.js` と `qa/depth-check.mjs` は shared-workflows `template/` の写しで、ここでは直さない。自信あり・2段目「これは、どの決まり？」・復習（1→3→7日後）。記録 `tnr-v1`・文字 `tnr-fs`・復習 `tnr-depth`。
- 共有は遊ぶ人が押したときに開くリンクだけ。シェアの文に問題・答えを出さない。感想は Issue フォーム（`.github/ISSUE_TEMPLATE/feedback.yml`）を開くだけ。応援は Ko-fi へのただのリンクだけ。
- 外へ通信しない（CSP の `connect-src 'self'` を広げない）。見やすさ（`<!-- a11y:begin -->`〜`end`・`qa/a11y.mjs`）と引き継ぎ（`transfer.js`・`qa/transfer-check.mjs`）は shared-workflows `template/` の写しで、ここでは直さない。
- 画面の土台は利息の上限（risoku-jogen）から写した。写し元の言葉が残っていないかを QA が見る。
- PWA：画面のファイルを足したら `sw.js` の `SHELL` にも足し、中身を大きく変えたら `CACHE` の版を上げる。アイコンは `node tools/make-icons.cjs`、`og.png` は `node tools/make-og.cjs`。
- 公開リポジトリなので、個人情報・鍵を入れない。

## 公開

- GitHub Pages は `main` のルート（`/`）を公開する。PR の QA と Secret scan が ✅ になると `automerge.yml` が自動でマージする。止めたいときは下書きにするか、ラベル「自動マージしない」を付ける。
- `sw.js`・`manifest.webmanifest`・`data.js` を変える PR は自動ではマージされない（`hold_paths`）。CodeRabbit のレビューを読み、手でマージする直前に今の head へのレビューを読み直す。CodeRabbit は PR に `@coderabbitai review` とコメントして頼む。

## 共通のきまり

- PR の作り方・マージの仕方は satou-aaaaa/shared-workflows の `routines/pr.md`、全体のきまりは `routines/common.md` に従う。
- CI の部品は shared-workflows の共通部品を呼ぶ。リポジトリ側の .github/workflows には呼び出しと設定値だけを書く。
- Claude 以外の AI・人が作業するときは、shared-workflows の `routines/tool-map.md` と `routines/without-claude.md` を読む。
