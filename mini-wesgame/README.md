# mini-wesgame 用スプライト配信物

mini-wesgame が配信する Battle for Wesnoth 由来の画像アセット一式を、
GPLv3 第6条(d)の「オブジェクト(CDN配信画像)と同等アクセスのソース提供」を
満たす形で公開するもの(方針の全体像はリポジトリ直下の wesnoth_license_notes.md 参照)。
mini-wesgame からは `NEXT_PUBLIC_SPRITE_PACK_BASE` / `NEXT_PUBLIC_ASSET_BASE` が
jsDelivr(https://cdn.jsdelivr.net/gh/belre/wesnoth-contents-delivery@main/mini-wesgame)
経由でこのフォルダを既定で参照する(`packages/frontend/next.config.ts`)。

## 内容

| パス | 中身 | 形式 |
|---|---|---|
| units-loyalists.psp | 人間族(忠誠軍)ユニットの全アニメフレーム(262点) | PSP1パック |
| units-northerners.psp | オーク(北方勢)同上(179点) | PSP1パック |
| sprites/ | 個別URLフォールバック層一式: 地形タイル(terrain/)・base立ち絵(unit-base/)・ユニット個別フレーム・halo・飛び道具 | 無加工PNG(mini-wesgame の `public/sprites/` と同一構成) |

(2026-07-09 更新: mini-wesgame から undead / rebels / drakes / knalgan(盗賊系)の
陣営データを物理削除したため、対応するパックも削除。全陣営版は本家プロジェクト側で管理)
(2026-07-10 追加: sprites/ を追加。従来はterrain・base立ち絵をmini-wesgame自身の
ビルド成果物に同梱していたが、.pspパックと同じ「別リポジトリでの同等アクセス」原則に
揃えるため、個別PNG一式もここへ寄せた)

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
  node scripts/fetch-demo-sprites.mjs      # 上流から個別PNGを public/sprites/ に取得
  npx tsx scripts/build-sprite-packs.mts   # public/packs/*.psp を生成
  ```
  `sprites/`(このフォルダ)は上記コマンド後の `public/sprites/` を丸ごとコピーしたもの
  (`unit-base/` のみ例外: `UNIT_SPRITES` の各エントリの `base` 画像を
  `spriteKey.replaceAll("/", "_") + ".png"` の名前でコピーする。
  参照実装は mini-wesgame リポジトリの git 履歴上の
  `scripts/generate-unit-base-images.mts`(2026-07-10 CDN化に伴い削除済み)を参照)

## ライセンス

本フォルダの内容は **GPL v2 or later**(本リポジトリの方針によりGPLv3として統合可能)。
CC BY-SA 4.0 由来の素材の表示義務(Title/Author/Source/License)は上流のEXIFタグを
一次情報とする。「Battle for Wesnoth」はWesnoth, Inc.の商標であり、
本プロジェクトはWesnothプロジェクトとは無関係の非公式利用である。
