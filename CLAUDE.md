# Spirograph Break — CLAUDE.md

## プロジェクト概要

休憩用のアンビエントタイマーアプリ。スタートすると幾何学模様（スピログラフ）が自動で描かれ始め、**カウントダウンが 0 秒になる瞬間にちょうど描き終わる**。

- 単一HTMLファイル（`index.html`）。外部依存・ビルド工程なし。
- 公開: GitHub Pages → `https://masaki0510.github.io/spirograph-break/`
- リポジトリ: `masaki0510/spirograph-break`（branch `main`）

## 絶対に壊してはいけないルール

1. **「0秒で完成」の同期** — タイマーと描画は同一の時計を共有する。`head = SAMPLES * (elapsed / totalMs)` の構造を崩さない。
2. **セッション中のUI** — 操作を増やさない。静かで瞑想的に。
3. **数学的な誠実さ** — `pointAt` / `polyR` / `innerFreqs` はコンセプトの核。シンメトリー＝差が共通の倍数、アシンメトリー＝差が互いに素、この意味を保つ。
4. **単一ファイル** — `index.html` 1ファイルで完結。PWA化時以外は外部ファイルを増やさない。

## Git / GitHub ワークフロー

```
# 機能追加・修正は必ず feature ブランチで作業
git checkout -b feature/xxx

# コミット → push → PR作成 → main にマージ
git add index.html
git commit -m "..."
git push origin feature/xxx
gh pr create --title "..." --body "..."
gh pr merge --squash --delete-branch
```

- `main` への直接 push は禁止（緊急の typo 修正のみ例外）
- PR はスカッシュマージ（`--squash`）で履歴を綺麗に保つ

## デプロイ

`main` ブランチに merge されると GitHub Pages に自動反映（最大10分）。手動操作不要。

## ファイル構成

| ファイル | 役割 |
|---|---|
| `index.html` | 本体。スピログラフ + タイマーアプリ |
| `spirograph-studio.html` | 手動調整用の実験ツール（公開不要） |
| `spirograph-break-spec.md` | 設計仕様書 |
| `CLAUDE.md` | このファイル |

## 実装の要点

- `SAMPLES = 3000` 点を `[0, 2π]` でサンプリング。全歯車の回転数が整数なので 2π で曲線が閉じる。
- `baseR = min(W, H) * 0.40`
- Canvas `globalCompositeOperation = 'lighter'` で線が重なると光る
- 音はすべて Web Audio API で合成（ファイル不要）
- `prefers-reduced-motion` 対応済み

## 実装済み

- [x] 進行バー（YouTube Shorts 風、ブラス色・先端に白ノブ）
- [x] タイマーのデザイン仕上げ（グラデーション・コロン点滅）
