# mini-wesgame 用スプライトパック(PSP)

mini-wesgame が配信する Battle for Wesnoth 由来のユニットスプライト一式を、
**改変可能な形式(PSPパック)**として公開するもの。
GPLv3 第6条(d)の「オブジェクト(CDN配信画像)と同等アクセスのソース提供」に相当する
(方針の全体像はリポジトリ直下の wesnoth_license_notes.md 参照)。

## 内容

| ファイル | 中身 | 収録数 |
|---|---|---|
| units-loyalists.psp | 人間族(忠誠軍)ユニットの全アニメフレーム | 262 |
| units-northerners.psp | オーク(北方勢)同上 | 179 |
| units-knalgan.psp | オーク陣営が雇用する盗賊系(スプライトの原典フォルダがknalganのため別パック) | 13 |

(2026-07-09 更新: mini-wesgame から undead / rebels / drakes の陣営データを物理削除したため、
対応するパックも削除。全陣営版は本家プロジェクト側で管理)

## 形式(PSP1)

独自の連結形式。レイアウト:

```
[0..4)   magic "PSP1"
[4..8)   index長 N (uint32 LE)
[8..8+N) index JSON: { files: [{ path, offset, size }] }   ※pathは "/sprites/..." 相当
[8+N..)  ペイロード(全PNGのバイト列を連結)
```

エンコード/デコードの参照実装: mini-wesgame リポジトリの
`packages/frontend/src/lib/assets/packFormat.ts`(encodePack / parsePack)。
中身は無加工のPNG群であり、展開すれば個々のPNGとして改変できる。

## 出所と再生成

- 原典: Battle for Wesnoth 公式リポジトリ(https://github.com/wesnoth/wesnoth)
  `data/core/images/` 配下(GPL v2 or later / 一部 CC BY-SA 4.0。
  個別のライセンスは各PNGのEXIFタグに埋め込まれている — 展開して確認可能)
- 再生成手順(mini-wesgame リポジトリにて):
  ```
  cd packages/frontend
  node scripts/fetch-demo-sprites.mjs      # 上流から個別PNGを取得
  npx tsx scripts/build-sprite-packs.mts   # public/packs/*.psp を生成
  ```

## ライセンス

本フォルダの内容は **GPL v2 or later**(本リポジトリの方針によりGPLv3として統合可能)。
CC BY-SA 4.0 由来の素材の表示義務(Title/Author/Source/License)は上流のEXIFタグを
一次情報とする。「Battle for Wesnoth」はWesnoth, Inc.の商標であり、
本プロジェクトはWesnothプロジェクトとは無関係の非公式利用である。
