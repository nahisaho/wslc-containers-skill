# wslc-containers-skill

[English README](README.md) | [変更履歴](CHANGELOG.md) | [ライセンス: MIT](LICENSE)

**WSL 上の GitHub Copilot** から、WSL 組み込みのコンテナ CLI
(**`wslc.exe`**) を使って Linux コンテナを操作するための Agent skill です。
Docker Desktop は不要です。

## 特長

- `wslc` によるコンテナのビルド・実行・一覧・確認・停止・削除。
- イメージ、ネットワーク、ボリュームの管理。
- ラッパースクリプトが Windows 側の `wslc.exe` のパスを自動解決
  (`/mnt/c/Program Files/WSL/wslc.exe` のようなスペースを含むパスにも対応)。
- Copilot 向けの安全ガイドライン (不明なイメージの実行前に確認、破壊的操作の前に状態確認)。

## 要件

- WSL **2.9.3 以降**の Windows (`wsl.exe --version` で確認、`wsl --update` で更新)。
- WSL コンテナ機能 (`wslc.exe` は WSL に同梱)。
- Windows 相互運用 (`/mnt/c`) が有効な WSL ディストリビューション上で GitHub Copilot CLI を実行。

## リポジトリ構成

```
.github/skills/wslc-containers/
├── SKILL.md          # Copilot 向けのスキル定義と指示
└── scripts/wslc.sh   # wslc.exe を探索して実行するラッパー
```

## インストール

リポジトリをクローンするか、スキルのディレクトリをプロジェクトにコピーします。

```bash
git clone https://github.com/nahisaho/wslc-containers-skill.git
# 既存プロジェクトへコピーする場合
cp -r wslc-containers-skill/.github/skills/wslc-containers <your-project>/.github/skills/
```

Copilot は `.github/skills/` 配下のプロジェクトスキルを検出します。
全プロジェクトで個人的に使う場合は `~/.copilot/skills/` に配置してください。

## 使い方

Copilot に自然言語で依頼します。例:

- 「wslc で alpine を pull して `uname -a` を実行して」
- 「このディレクトリの Dockerfile をビルドして 8080 番ポートで起動して」
- 「実行中のコンテナを一覧し、`web` のログを表示して」

ラッパーを直接呼び出すこともできます。

```bash
bash .github/skills/wslc-containers/scripts/wslc.sh version
bash .github/skills/wslc-containers/scripts/wslc.sh image ls
bash .github/skills/wslc-containers/scripts/wslc.sh run --rm docker.io/library/alpine:latest echo hello
```

### 主なコマンド

| 目的 | コマンド |
|---|---|
| バージョン / 情報 | `wslc version`, `wslc system info` |
| イメージ取得 / 一覧 | `wslc pull <image>`, `wslc image ls` |
| ビルド | `wslc build -t <tag> <path>` |
| 実行 | `wslc run --rm -it <image> <cmd>` / `wslc run -d --name <n> -p <h>:<c> <image>` |
| コンテナ一覧 | `wslc container ps` |
| exec / ログ | `wslc exec <c> <cmd>`, `wslc logs <c>` |
| 停止 / 再起動 / 削除 | `wslc stop <c>`, `wslc restart <c>`, `wslc remove <c>` |
| ネットワーク / ボリューム | `wslc network ls`, `wslc volume ls` |

(スキル内の `wslc` はラッパースクリプト `scripts/wslc.sh` を指します。)

## セキュリティ上の注意

`wslc run` / `wslc exec` はコンテナ内で任意のコードを実行します。見慣れない
イメージや信頼できないレジストリを使う前に確認し、名前・ポート・パスは使用前に検証してください。

## トラブルシューティング

`wslc.exe` が見つからない場合、ラッパーは非ゼロで終了し `wsl --update` を案内します。
WSL が 2.9.3 以降で、Windows 相互運用が有効であることを確認してください。

## ライセンス

[MIT](LICENSE) © 2026 nahisaho
