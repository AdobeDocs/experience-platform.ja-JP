---
title: Jupyter Notebook からの Data Distillerへの接続
description: Jupyter Notebook から Data Distillerに接続する方法を説明します。
exl-id: e6238b00-aaeb-40c0-a90f-9aebb1a1c421
source-git-commit: 308d07cf0c3b4096ca934a9008a13bf425dc30b6
workflow-type: tm+mt
source-wordcount: '684'
ht-degree: 0%

---

# Jupyter Notebook からの Data Distillerへの接続

価値の高いカスタマーエクスペリエンスデータで機械学習パイプラインを強化するには、まず [!DNL Jupyter Notebooks] から Data Distillerに接続する必要があります。 このドキュメントでは、機械学習環境の [!DNL Python] ノートブックから Data Distillerに接続する手順を説明します。

## はじめに

このガイドは、インタラクティブ [!DNL Python] ノートブックに精通し、ノートブック環境にアクセスできることを前提としています。 ノートブックは、クラウドベースの機械学習環境内でホストすることも、[[!DNL Jupyter Notebook]](https://jupyter.org/) でローカルにホストすることもできます。

### 接続資格情報の取得 {#obtain-credentials}

Data Distillerおよびその他のAdobe Experience Platform サービスに接続するには、Experience PlatformAPI 資格情報が必要です。 API 資格情報は、Experience Platformに対する開発者アクセス権を持つユーザーが ](https://developer.adobe.com/console/home)0}Adobe Developer Console} で作成できます。 [データサイエンスワークフロー専用の Oauth2 API 認証情報を作成し、組織のAdobeシステム管理者に、適切な権限を持つロールに認証情報を割り当てることをお勧めします。

API 認証情報の作成および必要な権限の取得に関する詳細な手順については、[ 認証およびアクセスExperience PlatformAPI](../../../landing/api-authentication.md) を参照してください。

データサイエンスに推奨される権限は次のとおりです。

- データサイエンスに使用されるサンドボックス （通常は `prod`）
- データモデリング：[!UICONTROL  スキーマの管理 ]
- データ管理：[!UICONTROL  データセットの管理 ]
- データ取り込み：[!UICONTROL  ソースの表示 ]
- 宛先：[!UICONTROL  データセット宛先の管理とアクティブ化 ]
- クエリサービス：[!UICONTROL  クエリの管理 ]

デフォルトでは、役割（およびその役割に割り当てられた API 資格情報）は、ラベル付きデータへのアクセスからブロックされます。 組織のデータガバナンスポリシーに従って、システム管理者は、データサイエンスの使用に適していると思われる特定のラベル付きデータへのアクセス権を役割に付与できる場合があります。 Platform のお客様は、関連する規制や組織のポリシーに準拠するために、ラベルアクセスとポリシーを適切に管理する責任を負います。

### 資格情報を別の設定ファイルに保存 {#store-credentials}

認証情報の安全性を維持するために、コードに認証情報を直接書き込まないことをお勧めします。 代わりに、秘密鍵証明書に関する情報を別の設定ファイルに保存し、Experience Platformと Data Distillerに接続するために必要な値を読み取ります。

例えば、`config.ini` というファイルを作成して、次の情報を（セッション間で保存するのに役立つデータセット ID などのその他の情報と共に）含めることができます。

```ini
[Credential]
ims_org_id=<YOUR_IMS_ORG_ID>
sandbox_name=<YOUR_SANDBOX_NAME>
client_id=<YOUR_CLIENT_ID>
client_secret=<YOUR_CLIENT_SECRET>
scopes=openid, AdobeID, read_organizations, additional_info.projectedProductContext, session
tech_acct_id=<YOUR_TECHNICAL_ACCOUNT_ID>
```

ノートブックでは、標準 [!DNL Python] ライブラリの `configParser` パッケージを使用して、秘密鍵証明書情報をメモリに読み込むことができます。

```python
from configparser import ConfigParser

# Create a ConfigParser object to read and store information from config.ini
config = ConfigParser()
config_path = '<PATH_TO_YOUR_CONFIG.INI_FILE>'
config.read(config_path)
```

その後、次のように、コード内で秘密鍵証明書の値を参照できます。

```python
org_id = config.get('Credential', 'ims_org_id')
```

## aepp Python ライブラリのインストール {#install-python-library}

[aepp](https://github.com/adobe/aepp/tree/main) は、Adobeが管理するオープンソース [!DNL Python] ライブラリで、他のExperience Platformサービスにリクエストを行う場合と同様に、データDistillerに接続しクエリを送信する機能を提供します。 `aepp` ライブラリは、インタラクティブな Data Distiller クエリ用に PostgreSQL データベースアダプターパッケージの `psycopg2` を利用します。 単独で Data Distillerに接続し、Experience Platformデータセットをクエリすることもできますが、`psycopg2` の方が、より高い利便性と、すべてのExperience PlatformAPI サービスに対するリクエストを行う `aepp` めの追加機能を提供します。

お使いの環境で `aepp` と `psycopg2` をインストールまたはアップグレードするには、ノートブックで `%pip` magic コマンドを使用します。

```python
%pip install --upgrade aepp
%pip install --upgrade psycopg2-binary
```

その後、次のコードを使用して、資格情報を含んだ `aepp` ライブラリを設定できます。

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

認証情報 `aepp` 設定されたら、次のコードを使用して Data Distillerへの接続を作成し、次のようにインタラクティブセッションを開始できます。

```python
from aepp import queryservice

dd_conn = queryservice.QueryService().connection()
dd_cursor = queryservice.InteractiveQuery2(dd_conn)
```

その後、Experience Platformサンドボックスでデータセットに対してクエリを実行できます。 クエリ対象のデータセットの ID を指定すると、カタログサービスから対応するテーブル名を取得し、そのテーブルに対してクエリを実行できます。

```python
table_name = 'ecommerce_events'
simple_query = f'''SELECT * FROM {table_name} LIMIT 5'''
dd_cursor.query(simple_query)
```

### 単一のデータセットに接続して、クエリのパフォーマンスを向上させます {#connect-to-single-dataset}

デフォルトでは、Data Distiller接続は、サンドボックス内のすべてのデータセットに接続します。 クエリを高速化しリソース使用量を削減するには、代わりに、関心のある特定のデータセットに接続できます。 これを行うには、Data Distiller接続オブジェクトの `dbname` を `{sandbox}:{table_name}` に変更します。

```python
from aepp import queryservice

sandbox = config.get('Credential', 'sandbox_name')
table_name = 'ecommerce_events'

dd_conn = queryservice.QueryService().connection()
dd_conn['dbname'] = f'{sandbox}:{table_name}'
dd_cursor = queryservice.InteractiveQuery2(dd_conn)
```

## 次の手順

このドキュメントでは、機械学習の [!DNL Python] ノートブックから Data Distillerに接続する方法について説明しました。 Experience Platformから機能パイプラインを作成して、機械学習環境でカスタムモデルにフィードする次の手順は、[ データセットを調査および分析 ](./exploratory-analysis.md) することです。
