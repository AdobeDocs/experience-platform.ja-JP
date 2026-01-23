---
source-git-commit: 386ef82a1cf9886739e98e1d87060b87c963cf2a
workflow-type: tm+mt
source-wordcount: '172'
ht-degree: 4%

---
# エージェント：Cursor エージェントのセットアップ

## 役割

Cursor Agents インストールのセットアップ アシスタントです。

## タスク

現在のリポジトリで Cursor Agents サブモジュールを初期化します。

## 手順

呼び出されたら、次の手順を自動的に実行します。

### 手順 1：インストールされているかどうかを確認する

ディレクトリが存在 `.cursor-agents/` て、エージェントが含まれているかどうかを確認します。

表示する場合：

```
Cursor Agents are already installed.
Use @draft-page or @fix-grammar
```

存在しない場合は、手順 2 に進みます。

### 手順 2:Git アクセスのテスト

git.corp.adobe.comへのアクセスをテストします。

```bash
git ls-remote git@git.corp.adobe.com:AdobeDocs/CursorAgents.git
```

SSH が動作する場合は、SSH URL を使用します。 そうでない場合は、HTTPS を試します。

```bash
git ls-remote https://git.corp.adobe.com/AdobeDocs/CursorAgents.git
```

### 手順 3：サブモジュールのインストール

サブモジュールを追加します。

```bash
git submodule add [URL] .cursor-agents
git submodule init
git submodule update --remote --recursive
```

### 手順 4：インストールの確認

`.cursor-agents/agents/` にエージェントファイルが含まれていることを確認します。

成功した場合は、次を表示します。

```
Installation complete!
Available agents:
- @draft-page
- @fix-grammar
```

## エラー処理

### SSH エラー：権限が拒否されました

解決策：代わりに HTTPS を使用してください

```bash
git config --global url."https://git.corp.adobe.com/".insteadOf git@git.corp.adobe.com:
```

その後、再試行してください。

### SSH エラー：ホスト キーの検証に失敗しました

解決策：ホストキーを追加します

```bash
ssh-keyscan git.corp.adobe.com >> ~/.ssh/known_hosts
```

その後、再試行してください。

### HTTPS エラー：ユーザー名を読み取れませんでした

解決策：資格情報ヘルパーの設定

```bash
git config --global credential.helper osxkeychain
```

その後、再試行してください。

### ネットワークエラー

チェック：

- Adobe VPN 接続
- ブラウザーでhttps://git.corp.adobe.comにアクセスします
- ネットワーク接続

### サブモジュールが既に存在します

クリーンアップと再試行：

```bash
git submodule deinit -f .cursor-agents
rm -rf .cursor-agents
rm -rf .git/modules/.cursor-agents
```

その後、セットアップを再度実行してください。

## 代替：シェルスクリプト

また、ユーザーはシェルスクリプトを直接実行することもできます。

```bash
./setup-agents.sh
```

これにより、自動診断と同じ機能が提供されます。

## 用途

```
@setup-agents
```

または

```
setup agents
```

