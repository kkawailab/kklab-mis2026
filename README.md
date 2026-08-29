# 経営情報 2026

2026年度「経営情報」（担当教員：河合勝彦）の講義サイトです。

## 公開サイト

<https://kklab.mobi/kklab-mis2026/>

## ローカルでの確認

```bash
python3 -m http.server 8000
```

ブラウザで <http://localhost:8000> を開いてください。

## 更新方法

`main` ブランチへのプッシュを契機に、GitHub Actions が GitHub Pages へ自動デプロイします。

## 出席確認のしくみ

- `attendance/attendance01.html` 〜 `attendance15.html` — 受講生向けのAIクイズ（各回5問）。全問正解するとサーバー時刻を取得して確認コードを計算し、出席カード（結果画面）を表示します。受講生はそのスクリーンショットを Teams の Class Notebook に提出します。
- `attendance/index.html` — 出席確認の一覧（問題集）。
- 出席確認・検証の各ページは、トップページと同じヘッダー／フッター（`assets/css/style.css` + `assets/css/pages.css`）を使います。
- `verify.html` — 教員用。スクリーンショットの学籍番号・氏名・提出時間・確認コードを転記すると、コードを再計算して一致／不一致を判定します（不一致なら改ざんの可能性）。
- `code.js` — 上記のページで共用するコード計算ロジック（SHA-256 と、時刻APIからのJST取得）。

注意点:

- `code.js` の `computeCode` / `fieldNormalize` / `codeKey` / `formatJst` や、クイズページ・学習ページの `PAGE_ID` を変更すると、既に提出された確認コードの検証ができなくなります。
- 確認コードの計算に `crypto.subtle` を使うため、ページは HTTPS でのみ動作します（`http:` は自動で `https:` にリダイレクト）。ローカル確認は `http://localhost:8000` を使ってください。
- 問題の正解はページ内の JavaScript に平文で書かれています（改ざん防止ではなく、出席の記録が目的の設計です）。

## プログラミング入門

- `programming/index.html` — プログラミング入門のハブページ。30日カリキュラムのほか、オンライン実習環境（JupyterLab など）、体験型教材・データ可視化デモ、サンプルコード・チュートリアル（いずれも外部サイト）へのリンクをまとめています。
- `programming/htmlcss30/index.html` — 「プログラミング入門30日」（HTML → HTML/CSS → JavaScript → Python）の目次。
- `programming/htmlcss30/day01.html` 〜 `day30.html` — 各日の学習ページ（解説・練習・理解度チェック）。理解度チェックに正解すると、出席確認と同じしくみで確認コード付きの提出用画面が表示されます（`PAGE_ID` は `htmlcss30-day01` 〜 `htmlcss30-day30`）。`verify.html` で照合できます。
- `assets/css/pages.css` — 下層ページ（プログラミング入門・出席確認・検証）用の共通スタイル（`assets/css/style.css` の後に読み込む）。

教材は [kklab-is2026](https://kklab.mobi/kklab-is2026/) で公開しているものを本サイトのデザインに合わせて取り込んだものです。学習ページの確認コードは本サイトの `code.js`（鍵が異なる）で計算されるため、kklab-is2026 で取得したコードとは互換性がありません。
