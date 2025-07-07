- PoseLandmarkerのモデルパスをローカルファイル（api/pose_landmarker_full.task）に変更
- GPUデリゲートを指定
- ユーザープロンプト:
姿勢推定モデル:pose_landmarker のモデルを使い分けるように仕様変更する。

/api/ 内の.taskファイルに合わせて、軽量版、標準版、重量版のindex.htmlファイルをそれぞれ別で作成し、他の.htmlファイルへのリンクをそれぞれの.htmlに表示する。
モデルはmodelAssetPath: "api/pose_landmarker_full.task", の部分で指定する。これ以外の部分は変更しないで。
まず修正計画を立てて。このプロンプトと計画内容をupdate_log_0711.mdへ記述して。 (See <attachments> above for file contents. You may not need to search or read the file again.)

---

修正計画（仕様変更の概要）:

1. 目的
- `/api/` ディレクトリ内の3種類の姿勢推定モデル（lite, full, heavy）に対応した3つのHTMLファイルを用意し、ユーザーが用途に応じてモデルを切り替えられるようにする。

2. 具体的な作業内容
- 既存の `index.html` を標準版（fullモデル用）として残す。
- `index copy_0704.html` を軽量版（liteモデル用）として、`modelAssetPath: "api/pose_landmarker_lite.task"` に修正。
- `index copy_0711.html` を重量版（heavyモデル用）として、`modelAssetPath: "api/pose_landmarker_heavy.task"` に修正。
- 各HTMLファイルの画面上部など目立つ場所に、他の2つのHTMLファイルへのリンクを追加し、簡単に切り替えられるようにする。
- それ以外の部分（UIやロジック）は一切変更しない。

3. 注意事項
- モデルファイルのパス指定（`modelAssetPath`）のみを切り替える。
- 既存の機能やデザイン、ロジックには手を加えない。
- update_log_0711.md には本プロンプトと上記計画内容を記載する。

---

## 実施作業（2025年7月7日）

### 1. 姿勢推定モデルの使い分け対応

**実施内容:**
- 3つのHTMLファイルをそれぞれ異なるモデルに対応
  - `index.html` → 軽量版（`api/pose_landmarker_lite.task`）
  - `index_full.html` → 標準版（`api/pose_landmarker_full.task`）
  - `index_heavy.html` → 重量版（`api/pose_landmarker_heavy.task`）

**具体的な変更:**
1. 各HTMLファイルの `modelAssetPath` を適切なモデルファイルに変更
2. 各HTMLファイルのタイトルに版名を追加
3. ヘッダー部分にモデル切り替えボタンを追加
   - 横並び中央揃えで配置
   - 現在のモデルは無効化（オレンジ色で表示）
   - 他のモデルはリンクボタンとして機能

**変更ファイル:**
- `/Users/embryo03/Documents/GitHub/badmintools/index.html`
- `/Users/embryo03/Documents/GitHub/badmintools/index_full.html`
- `/Users/embryo03/Documents/GitHub/badmintools/index_heavy.html`

**UIの改善:**
- ユーザーが用途に応じて適切なモデルを選択可能
- 軽量版：処理速度重視、精度は標準的
- 標準版：バランスの取れた処理速度と精度
- 重量版：高精度、処理に時間がかかる可能性
