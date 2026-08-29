<div align="center">

  <img src="assets/icon.png" alt="ggst-clipper Logo" width="240" height="240" />

  # ggst-clipper

  <p align="center">
    <strong>対戦格闘ゲームの動画から、試合シーンを画像認識で自動検出＆高精度クリッピング</strong>
    <br />
    <i>Automated Match Clip Generator for Fighting Game Footage via Template Matching</i>
  </p>

  <p align="center">
    <a href="https://github.com/2shi0/ggst-cliper/releases"><img src="https://img.shields.io/github/v/release/2shi0/ggst-cliper?logo=github" alt="Release" /></a>
    <a href="https://github.com/2shi0/ggst-cliper/actions/workflows/release.yml"><img src="https://img.shields.io/github/actions/workflow/status/2shi0/ggst-cliper/release.yml?logo=githubactions&logoColor=white&label=Build" alt="Build Status" /></a>
    <a href="http://www.wtfpl.net/"><img src="https://img.shields.io/badge/License-WTFPL-brightgreen.svg" alt="License: WTFPL" /></a>
    <a href="https://github.com/2shi0/ggst-cliper/releases"><img src="https://img.shields.io/github/downloads/2shi0/ggst-cliper/total?logo=github" alt="Downloads" /></a>
    <a href="https://github.com/2shi0/ggst-cliper/stargazers"><img src="https://img.shields.io/github/stars/2shi0/ggst-cliper?logo=github" alt="Stars" /></a>
  </p>

  <p align="center">
    <a href="https://github.com/2shi0/ggst-cliper/releases/latest">
      <img src="https://img.shields.io/badge/Download_Latest_Release-0078D4?style=for-the-badge&logo=windows&logoColor=white" alt="Download Latest Release" />
    </a>
  </p>

</div>


## 📖 Overview / 概要

**ggst-clipper** は、対戦格闘ゲーム（GUILTY GEAR -STRIVE- など）の長時間録画アーカイブから、画像認識を用いて試合開始・終了・勝敗シーンを自動検出し、試合単位でスマートに切り抜いて保存する Windows 向け GUI ツールです。

「配信アーカイブから自分の試合だけを後で振り返りたい」「対戦ごとに動画を分割・整理したい」といった作業を全自動化します。

## 📋 Prerequisites / 必要環境

- **OS**: Windows 10 / 11 (64-bit)
- **FFmpeg**: 動画の解析および切り抜き処理に必須です。

未インストールの場合は、PowerShell 等で以下のコマンドを実行してインストールしてください。

```powershell
winget install ffmpeg
```

> [!NOTE]
> インストール後、ターミナルで `ffmpeg -version` が実行可能であることを確認してください。


## 📥 Installation / インストール

### 方法 1: インストーラーを使用（推奨）
1. [Releases ページ](https://github.com/2shi0/ggst-cliper/releases) から最新バージョンのインストーラー（`ggst-clipper-setup-*.exe`）をダウンロードします。
2. インストーラーを実行して画面の指示に従いセットアップを完了します。

### 方法 2: ソースコードからビルド
```powershell
# リポジトリのクローン
git clone https://github.com/2shi0/ggst-cliper.git
cd ggst-cliper

# リリースビルドの実行
cargo build --release
```
ビルド完了後、`target/release/ggst-clipper.exe` が生成されます。


## 🚀 Quick Start / 使い方

### 1. メイン画面と動画の読み込み

アプリを起動すると、以下のメイン画面が表示されます。

<div align="center">
  <img src="assets/main.png" alt="Main Window" width="560" />
</div>

- **動画の選択**: ウィンドウ中央のドロップエリアに動画ファイル（`.mp4`, `.mkv`, `.avi`, `.mov`, `.webm`）を**ドラッグ＆ドロップ**するか、**「Select Video...」** ボタンをクリックして動画を選択します。
- **設定画面を開く**: 右上の **「⚙ Settings」** ボタンをクリックして初期設定や各種パラメータを調整します。


### 2. 設定（Settings）

右上の **「⚙ Settings」** を開いて各種設定を行います。設定内容は自動で保存されます。

<div align="center">
  <img src="assets/settings.png" alt="Settings Window" width="480" />
</div>

#### ① General
- **Output Directory**: 切り抜いた動画の保存先フォルダを指定します。
  - **Browse...**: 保存先フォルダを選択します。
  - **Clear**: 指定を解除します（未指定時は元動画と同じフォルダ内の `output/` に自動保存されます）。

#### ② Templates & Detection Range
切り抜きの基準となる画像（PNG / JPG）を登録し、判定領域やタイミングの微調整を行います。

- **Start Match Template**: 試合開始を検出するための基準画像（例: `DUEL 1`, `ROUND 1`）
- **End Match Template**: 試合終了を検出するための基準画像（例: `SLASH`, `K.O.`, リザルト画面）
  - **Select ROI**: 判定領域（Region of Interest）の指定。画像全体ではなく特定の文字・UI部分をドラッグで囲んで指定してください。
  - **Start / End Offset**: 切り出し開始・終了地点のフレームオフセット（終了タイミングを早めたい/遅らせたい場合に調整）。
  - **Remove**: 登録したテンプレート画像を解除します。

#### ③ Match & Character Detection
GUILTY GEAR -STRIVE- 向けの自動判別機能です。

- **Detect Win / Loss (GGST only)**: リザルト画面の文字認識により勝敗（WIN / LOSE）を自動判定し、ファイル名に付与します。
- **Detect Character Names (GGST only)**: 画面上のキャラ名をOCRで検出し、対戦キャラ別のフォルダに自動分類します。
- **My Character**: 自分の使用キャラクターを選択します。対戦相手のキャラクターを自動判別するために使用されます（**Edit List** から候補キャラクターリストの編集も可能）。

#### ④ Engine Parameters
| 項目 | 説明 | デフォルト / 推奨値 |
| :--- | :--- | :--- |
| **Threshold** | テンプレートマッチングの一致しきい値（0.0 〜 1.0）。値が大きいほど厳密に判定 | `0.90` |
| **Step Frames** | シーン探索時にスキップするフレーム間隔（探索速度と精度のバランス） | `60` |


### 3. 切り抜き処理の実行

1. 設定完了後、メイン画面に動画をドラッグ＆ドロップ（または「Select Video...」で選択）します。
2. 解析・切り抜きウィンドウが表示され、自動的にシーン検出とクリップの書き出しが開始されます。
3. 処理完了後、設定した保存先フォルダに対戦ごとの切り抜き動画が保存されます。

---

<div align="center">
  <sub>Built with ❤️ and 🦀 by <a href="https://github.com/2shi0">2shi0</a></sub>
</div>
