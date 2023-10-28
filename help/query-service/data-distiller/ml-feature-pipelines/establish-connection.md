---
title: Jupyter ノートブックから Data Distillerに接続する
description: Jupyter ノートブックから Data Distillerに接続する方法を説明します。
source-git-commit: 60c5a624bfbe88329ab3e12962f129f03966ce77
workflow-type: tm+mt
source-wordcount: '693'
ht-degree: 1%

---

# Jupyter ノートブックから Data Distillerに接続する

価値の高い顧客体験データを使用して機械学習パイプラインを強化するには、まず、 [!DNL Jupyter Notebooks]. このドキュメントでは、 [!DNL Python] ノートブックを機械学習環境に追加します。

## はじめに

このガイドは、読者がインタラクティブ機能に精通していることを前提としています。 [!DNL Python] ノートブックを使用し、ノートブック環境にアクセスできる。 ノートブックは、クラウドベースの機械学習環境内でホストすることも、 [[!DNL Jupyter Notebook]](https://jupyter.org/).

### 接続資格情報の取得 {#obtain-credentials}

Data Distillerおよびその他のAdobe Experience Platformサービスに接続するには、Experience PlatformAPI の資格情報が必要です。 API 資格情報は、  [Adobe Developer Console](https://developer.adobe.com/console/home) Experience Platformへの開発者アクセス権を持つユーザーによって データサイエンスワークフロー専用の Oauth2 API 資格情報を作成し、組織のAdobeシステム管理者に、適切な権限を持つ役割に資格情報を割り当ててもらうことをお勧めします。

詳しくは、 [Experience PlatformAPI の認証とアクセス](../../../landing/api-authentication.md) API 資格情報の作成と必要な権限の取得に関する詳しい手順については、を参照してください。

データサイエンスに推奨される権限は次のとおりです。

- データサイエンス ( 通常、 `prod`)
- データモデリング： [!UICONTROL スキーマを管理]
- データ管理： [!UICONTROL データセットの管理]
- データ取り込み： [!UICONTROL ソースを表示]
- 宛先： [!UICONTROL データセットの宛先の管理とアクティブ化]
- クエリサービス： [!UICONTROL クエリの管理]

デフォルトでは、役割（およびその役割に割り当てられた API 資格情報）は、ラベル付きのデータへのアクセスをブロックされます。 組織のデータガバナンスポリシーに従い、システム管理者は、データサイエンスの使用に適したと見なされる特定のラベル付きデータに対して、役割のアクセス権を付与できます。 Platform のお客様は、関連する規制や組織のポリシーに準拠するために、ラベルのアクセスとポリシーを適切に管理する必要があります。

### 認証情報を別個の設定ファイルに保存 {#store-credentials}

秘密鍵証明書の安全性を維持するには、秘密鍵証明書情報をコードに直接書き込まないことをお勧めします。 代わりに、秘密鍵証明書情報を別の設定ファイルに保持し、Experience Platformと Data Distillerへの接続に必要な値を読み取ります。

例えば、 `config.ini` とには、次の情報（と共に、セッション間で保存するのに役立つデータセット ID などのその他の情報）を含めます。

```ini
[Credential]
ims_org_id=<YOUR_IMS_ORG_ID>
sandbox_name=<YOUR_SANDBOX_NAME>
client_id=<YOUR_CLIENT_ID>
client_secret=<YOUR_CLIENT_SECRET>
scopes=openid, AdobeID, read_organizations, additional_info.projectedProductContext, session
tech_acct_id=<YOUR_TECHNICAL_ACCOUNT_ID>
```

ノートブックで、次に、 `configParser` 標準のパッケージ [!DNL Python] ライブラリ：

```python
from configparser import ConfigParser

# Create a ConfigParser object to read and store information from config.ini
config = ConfigParser()
config_path = '<PATH_TO_YOUR_CONFIG.INI_FILE>'
config.read(config_path)
```

その後、次のようにコード内で秘密鍵証明書の値を参照できます。

```python
org_id = config.get('Credential', 'ims_org_id')
```

## aepp Python ライブラリのインストール {#install-python-library}

[aepp](https://github.com/adobe/aepp/tree/main) はAdobeが管理するオープンソースです。 [!DNL Python] 他のExperience Platformサービスにリクエストを送信する際に、Data Distillerに接続し、クエリを送信する機能を提供するライブラリ。 The `aepp` ライブラリは、PostgreSQL データベースアダプタパッケージに依存しています  `psycopg2` インタラクティブな Data Distillerクエリ用。 Data Distillerに接続し、 `psycopg2` ただ一人で `aepp` は、すべてのExperience PlatformAPI サービスにリクエストを送信するための、より便利な追加機能を提供します。

インストールまたはアップグレードするには `aepp` および `psycopg2` お使いの環境では、 `%pip` ノートブック内の magic コマンド：

```python
%pip install --upgrade aepp
%pip install --upgrade psycopg2-binary
```

次に、 `aepp` 次のコードを使用して、資格情報を使用したライブラリ：

```python
from configparser import ConfigParser

# Create a ConfigParser object to read and store information from config.ini
config = ConfigParser()
config_path = '<PATH_TO_YOUR_CONFIG.INI_FILE>'
config.read(config_path)

# Configure aepp with your credentials
import aepp

aepp.configure(
  org_id=config.get('Credential', 'ims_org_id'),
  sandbox=config.get('Credential', 'sandbox_name'),
  client_id=config.get('Credential', 'client_id'), 
  secret=config.get('Credential', 'client_secret'),
  scopes=config.get('Credential', 'scopes'),
  tech_id=config.get('Credential', 'tech_acct_id')
)
```

## Data Distillerへの接続の作成 {#create-connection}

1 回 `aepp` が資格情報で設定されている場合は、次のコードを使用して Data Distillerへの接続を作成し、次のようにインタラクティブセッションを開始できます。

```python
from aepp import queryservice

dd_conn = queryservice.QueryService().connection()
dd_cursor = queryservice.InteractiveQuery2(dd_conn)
```

その後、Experience Platformサンドボックス内のデータセットに対してクエリを実行できます。 クエリを実行するデータセットの ID を指定したら、対応するテーブル名をカタログサービスから取得して、テーブルに対してクエリを実行できます。

```python
table_name = 'ecommerce_events'
simple_query = f'''SELECT * FROM {table_name} LIMIT 5'''
dd_cursor.query(simple_query)
```

### 単一のデータセットに接続してクエリのパフォーマンスを向上 {#connect-to-single-dataset}

デフォルトでは、Data Distiller接続は、サンドボックス内のすべてのデータセットに接続します。 クエリを高速化し、リソースの使用量を削減するために、代わりに特定の目的のデータセットに接続できます。 これをおこなうには、 `dbname` Data Distiller接続オブジェクトの `{sandbox}:{table_name}`:

```python
from aepp import queryservice

sandbox = config.get('Credential', 'sandbox_name')
table_name = 'ecommerce_events'

dd_conn = queryservice.QueryService().connection()
dd_conn['dbname'] = f'{sandbox}:{table_name}'
dd_cursor = queryservice.InteractiveQuery2(dd_conn)
```

## 次の手順

このドキュメントでは、 [!DNL Python] ノートブックを機械学習環境に追加します。 機械学習環境でExperience Platformからカスタムモデルをフィードするための機能パイプラインを作成する次の手順は、次のとおりです。 [データセットの調査と分析](./exploratory-analysis.md).
