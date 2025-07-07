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

### 2. 軽量版（index.html）の最適化作業

**実施内容:**
軽量版のさらなる軽量化を実施。処理速度とレスポンス性能を向上させるため、以下の改良を行った。

**具体的な変更:**

1. **外部ライブラリの見直し**
   - Chart.js CDNを削除（未使用のため）
   - Tailwind CSS CDNは維持

2. **CSSの大幅な簡素化**
   - 複雑なガラスモーフィズム効果を削除
   - 複数テーマ（ダーク/ブルー/グリーン）を削除、ライトテーマのみ
   - アニメーション効果を削除（fadeIn, slideIn, hover effects等）
   - 装飾的なグラデーション、backdrop-filter、複雑な影効果を削除
   - シンプルな.simple-cardと.simple-buttonスタイルに統一

3. **DOM構造の簡素化**
   - テーマ選択ボタンを削除
   - ヘルプモーダルを完全削除
   - プログレスバーを削除
   - 不要なUI要素を削除

4. **JavaScript機能の削除**
   - テーマ切り替え機能を削除
   - ヘルプモーダル関連のコードを削除
   - キーボードショートカット機能を削除
   - アニメーション制御のコードを削除

5. **デザインの簡素化**
   - システムフォント（system-ui, -apple-system）を使用
   - 単純な白背景とグレー系カラーに統一
   - 影やエフェクトを最小限に抑制
   - フォントサイズを調整（text-4xl → text-3xl等）

**削除されたファイル容量:**
- CSS: 約300行 → 約50行（約80%削減）
- JavaScript: 約100行削除
- HTML: 約60行削除（ヘルプモーダル等）

**パフォーマンス改善:**
- 初期ロード時間の短縮
- CSS描画処理の軽量化
- DOM操作の削減
- メモリ使用量の削減

**機能保持:**
- 姿勢推定機能は完全に保持
- 動画アップロード・録画機能は保持
- 軌跡・ヒートマップ・ショット分析機能は保持
- データエクスポート機能は保持
- コート補正機能は保持

**変更ファイル:**
- `/Users/embryo03/Documents/GitHub/badmintools/index.html`


さらに、標準版で動作するモデルをfull.task → lite.taskに。
重量版で動作するモデルをheavy.task → full.taskに変更
ブラウザで開かれる標準ページを標準版に変更
それぞれのモデルへのリンクを変更

### 3. コート補正（トリミング）機能のスマホ・タブレット対応

**実施内容:**
- コート四隅トリミング（cropOverlayCanvas）の操作について、スマートフォンやタブレットでもPCと同様に直感的なドラッグ操作ができるよう、タッチイベント（touchstart, touchmove, touchend, touchcancel）に対応。

**具体的な変更:**
- cropOverlayCanvasに以下のイベントリスナーを追加：
  - touchstart：タッチ開始時にハンドルの当たり判定とドラッグ開始判定
  - touchmove：ドラッグ中のハンドル移動
  - touchend/touchcancel：ドラッグ終了
- タッチ座標取得用のgetTouchCoords関数を追加し、clientX/clientYの代わりにtouches[0].clientX等を使用
- e.preventDefault()と{ passive: false }で画面スクロール等の誤動作を防止
- 既存のCROP_HANDLE_RADIUS（45）を活かし、タッチでもハンドルを掴みやすい設計

**効果:**
- スマホ・タブレットでもPCと同じ感覚でコート四隅の調整が可能
- 小さな画面でも誤タッチを防ぎ、直感的な操作性を実現

**変更ファイル:**
- `/Users/embryo03/Documents/GitHub/badmintools/index.html`
