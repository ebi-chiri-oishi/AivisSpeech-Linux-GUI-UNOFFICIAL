# AivisSpeech-linux-gui-unofficial
日本語:これはAivisSpeechの**「非公式」**LinuxGUI版です。AivisSpeech様とVOICEVOX様に最大の敬意を表します。このプロジェクトが原因の不具合があればこちらに報告し、AivisSpeech様にご報告されるのはおやめください。AivisSpeechが原因の不具合であればAvisSpeech様にご報告ください。
English:This is the ""unofficial"" Linux GUI version of AivisSpeech. I would like to express my utmost respect to AivisSpeech and VOICEVOX. If you have any problems caused by this project, please report them here and do not report them to AivisSpeech. If the problem is caused by AivisSpeech, please report it to AvisSpeech.
# AivisSpeech

[![Releases](https://img.shields.io/github/v/release/Aivis-Project/AivisSpeech?label=Release)](https://github.com/Aivis-Project/AivisSpeech/releases)
[![License: LGPL-3.0](https://img.shields.io/badge/License-LGPL3-blue.svg)](LICENSE)
[![CI: Build](https://github.com/Aivis-Project/AivisSpeech/actions/workflows/build.yml/badge.svg)](https://github.com/Aivis-Project/AivisSpeech/actions/workflows/build.yml)
<!-- [![CI: Test](https://github.com/Aivis-Project/AivisSpeech/actions/workflows/test.yml/badge.svg)](https://github.com/Aivis-Project/AivisSpeech/actions/workflows/test.yml) -->

💠 **AivisSpeech:** **AI** **V**oice **I**mitation **S**ystem - Text to **Speech** Software

-----

**AivisSpeech は、[VOICEVOX](https://github.com/VOICEVOX/voicevox) のエディター UI をベースにした、日本語音声合成ソフトウェアです。**  
日本語音声合成エンジンの [AivisSpeech Engine](https://github.com/Aivis-Project/AivisSpeech-Engine) を組み込んでおり、かんたんに感情豊かな音声を生成できます。

#### [💠 AivisSpeech をダウンロード](https://aivis-project.com/speech/) ／ [💠 AivisSpeech Engine をダウンロード](https://github.com/Aivis-Project/AivisSpeech-Engine/releases)

-----

- [ユーザーの方へ](#ユーザーの方へ)
- [動作環境](#動作環境)
- [サポートされている音声合成モデル](#サポートされている音声合成モデル)
  - [対応モデルアーキテクチャ](#対応モデルアーキテクチャ)
  - [モデルファイルの配置場所](#モデルファイルの配置場所)
- [開発方針](#開発方針)
- [開発環境の構築](#開発環境の構築)
- [開発](#開発)
- [ライセンス](#ライセンス)

## ユーザーの方へ

**AivisSpeech の使い方をお探しの方は、[AivisSpeech 公式サイト](https://aivis-project.com/speech/) をご覧ください。**

このページでは、主に開発者向けの情報を掲載しています。  
以下はユーザーの方向けのドキュメントです。

- **[使い方](public/howtouse.md)**
- **[よくある質問](public/qAndA.md)**
- **[お問い合わせ](public/contact.md)**

## 動作環境

Windows・macOS 搭載の PC に対応しています。

- **Windows:** Windows 10・Windows 11
- **macOS:** macOS 13 Ventura 以降

> [!NOTE]
> Intel CPU 搭載 Mac での動作は積極的に検証していません。  
> Intel CPU 搭載 Mac はすでに製造が終了しており、検証環境やビルド環境の用意自体が難しくなってきています。Apple Silicon 搭載 Mac での利用をおすすめいたします。

## サポートされている音声合成モデル

**AivisSpeech に組み込まれている AivisSpeech Engine は、[AIVMX (**Ai**vis **V**oice **M**odel for ONN**X**)](https://github.com/Aivis-Project/aivmlib#aivmx-file-format-specification) (拡張子 `.aivmx`) フォーマットの音声合成モデルファイルをサポートしています。**

**AIVM** (**Ai**vis **V**oice **M**odel) / **AIVMX** (**Ai**vis **V**oice **M**odel for ONN**X**) は、**学習済みモデル・ハイパーパラメータ・スタイルベクトル・話者メタデータ（名前・概要・ライセンス・アイコン・ボイスサンプル など）を 1 つのファイルにギュッとまとめた、AI 音声合成モデル用オープンファイルフォーマット**です。  

AIVM 仕様や AIVM / AIVMX ファイルについての詳細は、Aivis Project にて策定した **[AIVM 仕様](https://github.com/Aivis-Project/aivmlib#aivm-specification)** をご参照ください。

> [!NOTE]  
> **「AIVM」は、AIVM / AIVMX 両方のフォーマット仕様・メタデータ仕様の総称でもあります。**  
> 具体的には、AIVM ファイルは「AIVM メタデータを追加した Safetensors 形式」、AIVMX ファイルは「AIVM メタデータを追加した ONNX 形式」のモデルファイルです。  
> 「AIVM メタデータ」とは、AIVM 仕様に定義されている、学習済みモデルに紐づく各種メタデータのことをいいます。

> [!IMPORTANT]  
> **AivisSpeech Engine は AIVM 仕様のリファレンス実装でもありますが、敢えて AIVMX ファイルのみをサポートする設計としています。**  
> これにより、PyTorch への依存を排除してインストールサイズを削減し、ONNX Runtime による高速な CPU 推論を実現しています。

> [!TIP]  
> **[AIVM Generator](https://aivm-generator.aivis-project.com/) を使うと、既存の音声合成モデルから AIVM / AIVMX ファイルを生成したり、既存の AIVM / AIVMX ファイルのメタデータを編集したりできます！**

### 対応モデルアーキテクチャ

以下のモデルアーキテクチャの AIVMX ファイルを利用できます。

- `Style-Bert-VITS2`
- `Style-Bert-VITS2 (JP-Extra)`

> [!NOTE]
> AIVM の仕様上は多言語対応の話者を定義できますが、AivisSpeech Engine は VOICEVOX ENGINE と同様に、日本語音声合成のみに対応しています。  
> そのため、英語や中国語に対応した音声合成モデルであっても、日本語以外の音声合成はできません。

### モデルファイルの配置場所

AIVMX ファイルは、OS ごとに以下のフォルダに配置してください。  

- **Windows:** `C:\Users\(ユーザー名)\AppData\Roaming\AivisSpeech-Engine\Models`
- **macOS:** `~/Library/Application Support/AivisSpeech-Engine/Models`
- **Linux:** `~/.local/share/AivisSpeech-Engine/Models`

実際のフォルダパスは、AivisSpeech Engine の起動直後のログに `Models directory:` として表示されます。

> [!TIP]  
> **AivisSpeech 利用時は、AivisSpeech の UI 画面から簡単に音声合成モデルを追加できます！**  
> エンドユーザーの方は、基本的にこちらの方法で音声合成モデルを追加することをおすすめします。

> [!IMPORTANT]
> 開発版 (PyInstaller でビルドされていない状態で AivisSpeech Engine を実行している場合) の配置フォルダは、`AivisSpeech-Engine` 以下ではなく `AivisSpeech-Engine-Dev` 以下となります。

## 開発方針

[VOICEVOX](https://github.com/VOICEVOX) は非常に巨大なソフトウェアであり、現在も活発に開発が続けられています。  
そのため、AivisSpeech では VOICEVOX の最新版をベースに、以下の方針で開発を行っています。

- VOICEVOX 最新版への追従を容易にするため、できるだけ改変を必要最小限に留める
  - VOICEVOX から AivisSpeech へのリブランディングは必要な箇所のみ行う
- リファクタリングを行わない
  - VOICEVOX とのコンフリクトが発生することが容易に予想される上、コード全体に精通しているわけではないため
- AivisSpeech で利用しない機能 (歌声合成機能など) であっても、コードの削除は行わない
  - これもコンフリクトを回避するため
  - 利用しないコードの無効化は削除ではなく、コメントアウトで行う
- 保守や追従が困難なため、ドキュメントの更新は行わない
  - このため各ドキュメントは一切更新されておらず、AivisSpeech での変更を反映していない
- AivisSpeech 向けの改変にともないテストコードの維持が困難なため、テストコードの更新は行わない
  - 特に E2E テストは UI が大きく改変されているためまともに動作しない
  - テストコードの一部は AivisSpeech に合わせて修正されているが、動作検証は行っておらず放置されている

## 開発環境の構築

手順はオリジナルの VOICEVOX と異なります。  
事前に Node.js 20.12.2 がインストールされている必要があります。

```bash
# 依存関係をすべてインストール
npm ci

# .env.development を .env にコピー
## コピーした .env を編集する必要はない
cp .env.development .env

# macOS のみ、.env.production を編集
nano .env.production
--------------------
# executionFilePath を "AivisSpeech-Engine/run.exe" から "../Resources/AivisSpeech-Engine/run" に書き換える
## executionFilePath は、npm run electron:build でビルドした製品ビルドの AivisSpeech の起動時に使用される
...
VITE_DEFAULT_ENGINE_INFOS=`[
    {
        "uuid": "1b4a5014-d9fd-11ee-b97d-83c170a68ed3",
        "name": "AivisSpeech Engine",
        "executionEnabled": true,
        "executionFilePath": "../Resources/AivisSpeech-Engine/run",
        "executionArgs": [],
        "host": "http://127.0.0.1:10101"
    }
]`
...
--------------------

# 事前に別のターミナルで AivisSpeech Engine を起動しておく
## AivisSpeech Engine の開発環境は別途構築する必要がある
cd ../AivisSpeech-Engine
poetry run task serve
```

## 開発

手順は一部オリジナルの VOICEVOX と異なります。

```bash
# 開発環境で Electron 版 AivisSpeech を起動
npm run electron:serve

# 開発環境でブラウザ版 AivisSpeech を起動
npm run browser:serve

# Electron 版 AivisSpeech をビルド
npm run electron:build

# ブラウザ版 AivisSpeech (WIP) をビルド
npm run browser:build

# コードフォーマットを自動修正
npm run format

# コードフォーマットをチェック
npm run lint

# OpenAPI Generator による自動生成コードを更新
npm run openapi:generate

# 依存ライブラリのライセンス情報を生成
## VOICEVOX と異なり、音声合成エンジンとのライセンス情報との統合は行わない
## エディタ側で別途エンジンマニフェストから取得したライセンス情報を表示できるようにしているため不要
npm run license:generate
```

## ライセンス

ベースである VOICEVOX / VOICEVOX ENGINE のデュアルライセンスのうち、[LGPL-3.0](LICENSE) のみを単独で継承します。

下記ならびに [docs/](docs/) 以下のドキュメントは、[VOICEVOX](https://github.com/VOICEVOX/voicevox) 本家のドキュメントを改変なしでそのまま引き継いでいます。これらのドキュメントの内容が AivisSpeech にも通用するかは保証されません。

-----

# VOICEVOX

[![releases](https://img.shields.io/github/v/release/VOICEVOX/voicevox?label=Release)](https://github.com/VOICEVOX/voicevox/releases)
[![build](https://github.com/VOICEVOX/voicevox/actions/workflows/build.yml/badge.svg)](https://github.com/VOICEVOX/voicevox/actions/workflows/build.yml)
[![test](https://github.com/VOICEVOX/voicevox/actions/workflows/test.yml/badge.svg)](https://github.com/VOICEVOX/voicevox/actions/workflows/test.yml)
[![Discord](https://img.shields.io/discord/879570910208733277?color=5865f2&label=&logo=discord&logoColor=ffffff)](https://discord.gg/WMwWetrzuh)

[VOICEVOX](https://voicevox.hiroshiba.jp/) のエディターです。

（エンジンは [VOICEVOX ENGINE](https://github.com/VOICEVOX/voicevox_engine/) 、
コアは [VOICEVOX CORE](https://github.com/VOICEVOX/voicevox_core/) 、
全体構成は [こちら](./docs/全体構成.md) に詳細があります。）

## ユーザーの方へ

こちらは開発用のページになります。利用方法に関しては[VOICEVOX 公式サイト](https://voicevox.hiroshiba.jp/) をご覧ください。

## プロジェクトに貢献したいと考えている方へ

VOICEVOXプロジェクトは興味ある方の参画を歓迎しています。
[貢献手順について説明したガイド](./CONTRIBUTING.md)をご用意しております。

貢献というとプログラム作成と思われがちですが、ドキュメント執筆、テスト生成、改善提案への議論参加など様々な参加方法があります。
初心者歓迎タスクもありますので、皆様のご参加をお待ちしております。

VOICEVOX のエディタは Electron・TypeScript・Vue・Vuex などが活用されており、全体構成がわかりにくくなっています。  
[コードの歩き方](./docs/コードの歩き方.md)で構成を紹介しているので、開発の一助になれば幸いです。

Issue を解決するプルリクエストを作成される際は、別の方と同じ Issue に取り組むことを避けるため、
Issue 側で取り組み始めたことを伝えるか、最初に Draft プルリクエストを作成してください。

[VOICEVOX 非公式 Discord サーバー](https://discord.gg/WMwWetrzuh)にて、開発の議論や雑談を行っています。気軽にご参加ください。

### デザインガイドライン

[UX・UI デザインの方針](./docs/UX・UIデザインの方針.md)をご参照ください。

## 環境構築

[.node-version](.node-version) に記載されているバージョンの Node.js をインストールしてください。  
Node.js の管理ツール（[nvs](https://github.com/jasongin/nvs)や[Volta](https://volta.sh)など）を利用すると簡単にインストールでき、Node.js の自動切り替えもできます。

Node.js をインストール後、[このリポジトリ](https://github.com/VOICEVOX/voicevox.git) を
Fork して `git clone` し、次のコマンドを実行してください。

```bash
npm ci
```

## 実行

`.env.production`をコピーして`.env`を作成し、`VITE_DEFAULT_ENGINE_INFOS`内の`executionFilePath`に`voicevox_engine`のパスを指定します。

[製品版 VOICEVOX](https://voicevox.hiroshiba.jp/) のディレクトリのパスを指定すれば動きます。

Windows の場合でもパスの区切り文字は`\`ではなく`/`なのでご注意ください。

また、macOS 向けの`VOICEVOX.app`を利用している場合は`/path/to/VOICEVOX.app/Contents/MacOS/vv-engine/run`を指定してください。

Linux の場合は、[Releases](https://github.com/VOICEVOX/voicevox/releases/)から入手できる tar.gz 版に含まれる`run`コマンドを指定してください。
AppImage 版の場合は`$ /path/to/VOICEVOX.AppImage --appimage-mount`でファイルシステムをマウントできます。

VOICEVOX エディタの実行とは別にエンジン API のサーバを立てている場合は`executionFilePath`を指定する必要はありません。
これは製品版 VOICEVOX を起動している場合もあてはまります。

また、エンジン API の宛先エンドポイントを変更する場合は`VITE_DEFAULT_ENGINE_INFOS`内の`host`を変更してください。

```bash
# 開発しやすい環境で実行
npm run electron:serve

# ビルド時に近い環境で実行
npm run electron:serve -- --mode production
```

音声合成エンジンのリポジトリはこちらです <https://github.com/VOICEVOX/voicevox_engine>

### Storybook の実行

Storybook を使ってコンポーネントを開発することができます。

```bash
npm run storybook
```

### ブラウザ版の実行（開発中）

別途音声合成エンジンを起動し、以下を実行して表示された localhost へアクセスします。

```bash
npm run browser:serve
```

また、main ブランチのビルド結果がこちらにデプロイされています <https://voicevox-browser-dev.netlify.app/>  
今はローカル PC 上で音声合成エンジンを起動する必要があります。

## ビルド

```bash
npm run electron:build
```

### Github Actions でビルド

fork したリポジトリで Actions を ON にし、workflow_dispatch で`build.yml`を起動すればビルドできます。
成果物は Release にアップロードされます。

## テスト

### 単体テスト

```bash
npm run test:unit
npm run test-watch:unit # 監視モード
npm run test:unit -- --update # スナップショットの更新
```

### コンポーネントのテスト

Storybook を使ってコンポーネントのテストを行います。

```bash
npm run storybook # 先に Storybook を起動
npm run test:storybook
npm run test-watch:storybook # 監視モード
```

### ブラウザ End to End テスト

Electron の機能が不要な、UI や音声合成などの End to End テストを実行します。

> **Note**
> 一部のエンジンの設定を書き換えるテストは、CI(Github Actions)上でのみ実行されるようになっています。

```bash
npm run test:browser-e2e
npm run test-watch:browser-e2e # 監視モード
npm run test-watch:browser-e2e -- --headed # テスト中の UI を表示
```

Playwright を使用しているためテストパターンを生成することもできます。
**ブラウザ版を起動している状態で**以下のコマンドを実行してください。

```bash
npx playwright codegen http://localhost:5173/  --viewport-size=1024,630
```

詳細は [Playwright ドキュメントの Test generator](https://playwright.dev/docs/codegen-intro) を参照してください。

#### スクリーンショットの更新

ブラウザ End to End テストでは Visual Regression Testing を行っています。
現在 VRT テストは Windows のみで行っています。
以下の手順でスクリーンショットを更新できます：

##### Github Actions で更新する場合

1. フォークしたリポジトリの設定で GitHub Actions を有効にします。
2. リポジトリの設定の Actions > General > Workflow permissions で Read and write permissions を選択します。
3. `[update snapshots]` という文字列をコミットメッセージに含めてコミットします。

   ```bash
   git commit -m "UIを変更 [update snapshots]"
   ```

4. Github Workflow が完了すると、更新されたスクリーンショットがコミットされます。

##### ローカルで更新する場合

ローカル PC の OS に対応したもののみが更新されます。

```bash
npm run test:browser-e2e -- --update-snapshots
```

### Electron End to End テスト

Electron の機能が必要な、エンジン起動・終了などを含めた End to End テストを実行します。

```bash
npm run test:electron-e2e
npm run test-watch:electron-e2e # 監視モード
```

## 依存ライブラリのライセンス情報の生成

依存ライブラリのライセンス情報は Github Workflow でのビルド時に自動生成されます。以下のコマンドで生成できます。

```bash
# get licenses.json from voicevox_engine as engine_licenses.json

npm run license:generate -- -o voicevox_licenses.json
npm run license:merge -- -o public/licenses.json -i engine_licenses.json -i voicevox_licenses.json
```

## コードフォーマット

コードのフォーマットを整えます。プルリクエストを送る前に実行してください。

```bash
npm run fmt
```

## リント（静的解析）

コードの静的解析を行い、バグを未然に防ぎます。プルリクエストを送る前に実行してください。

```bash
npm run lint
```

## タイポチェック

[typos](https://github.com/crate-ci/typos) を使ってタイポのチェックを行っています。
[typos をインストール](https://github.com/crate-ci/typos#install) した後

```bash
typos
```

でタイポチェックを行えます。
もし誤判定やチェックから除外すべきファイルがあれば
[設定ファイルの説明](https://github.com/crate-ci/typos#false-positives) に従って`_typos.toml`を編集してください。

## 型チェック

TypeScript の型チェックを行います。

```bash
npm run typecheck
```

## Markdownlint

Markdown の文法チェックを行います。

```bash
npm run markdownlint
```

## Shellcheck

ShellScript の文法チェックを行います。
インストール方法は [こちら](https://github.com/koalaman/shellcheck#installing) を参照してください。

```bash
shellcheck ./build/*.sh
```

## OpenAPI generator

音声合成エンジンが起動している状態で以下のコマンドを実行してください。

```bash
curl http://127.0.0.1:50021/openapi.json >openapi.json

npx openapi-generator-cli generate \
    -i openapi.json \
    -g typescript-fetch \
    -o src/openapi/ \
    --additional-properties "modelPropertyNaming=camelCase,supportsES6=true,withInterfaces=true,typescriptThreePlus=true"

npm run fmt
```

### OpanAPI generator のバージョンアップ

新しいバージョンの確認・インストールは次のコマンドで行えます。

```bash
npx openapi-generator-cli version-manager list
```

## VS Code でのデバッグ実行

npm scripts の `serve` や `electron:serve` などの開発ビルド下では、ビルドに使用している vite で sourcemap を出力するため、ソースコードと出力されたコードの対応付けが行われます。

`.vscode/launch.template.json` をコピーして `.vscode/launch.json` を作成することで、開発ビルドを VS Code から実行し、デバッグを可能にするタスクが有効になります。

## ライセンス

LGPL v3 と、ソースコードの公開が不要な別ライセンスのデュアルライセンスです。
別ライセンスを取得したい場合は、ヒホに求めてください。  
X アカウント: [@hiho_karuta](https://x.com/hiho_karuta)
