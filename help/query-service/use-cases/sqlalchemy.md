---
title: Python と SQLAlchemy を使用した Platform データの管理
description: SQL の代わりに Python を使用して、SQLAlchemy を使用して Platform データを管理する方法を説明します。
exl-id: 9fba942e-9b3d-4efe-ae94-aed685025dea
source-git-commit: 8644b78c947fd015f6a169c9440b8d1df71e5e17
workflow-type: tm+mt
source-wordcount: '567'
ht-degree: 7%

---

# 次を使用した Platform データの管理 [!DNL Python] および [!DNL SQLAlchemy]

SQLAlchemy を使用して、Adobe Experience Platformデータの管理をより柔軟におこなう方法を説明します。 SQL に詳しくない方には、SQLAlchemy を使用すると、リレーショナルデータベースを使用する際の開発時間を大幅に短縮できます。 このドキュメントでは、接続の手順と例を示します [!DNL SQLAlchemy] をクエリサービスに追加し、Python を使用してデータベースとやり取りを開始します。

[!DNL SQLAlchemy] はオブジェクトリレーショナルマッパー (ORM) および [!DNL Python] SQL データベースに保存されたデータをに転送できるコードライブラリ [!DNL Python] オブジェクト。 その後、 [!DNL Python] コード。 これにより、PSQL のみを使用してデータを管理する必要がなくなりました。

## はじめに

[!DNL SQLAlchemy] を Experience Platform に接続するために必要な資格情報を取得するには、Platform UI のクエリワークスペースにアクセスできる必要があります。現在クエリワークスペースへのアクセス権がない場合は、組織の管理者に問い合わせてください。

## [!DNL Query Service] 資格情報 {#credentials}

資格情報を見つけるには、Platform UI にログインし、左側のナビゲーションから「**[!UICONTROL クエリ]**」を選択し、続いて「**[!UICONTROL 資格情報]**」を選択します。ログイン資格情報の見つけ方の詳細については、 [資格情報ガイド](../ui/credentials.md).

![クエリサービスの資格情報の期限が近づいている「資格情報」タブがハイライト表示されています。](../images/use-cases/credentials.png)

クエリサービスへの接続には、ポート 80 が推奨されるポートですが、ポート 5432 を使用することもできます。

>[!IMPORTANT]
>
>（上の図に示すように）期限切れの資格情報を使用してクエリサービスに接続すると、接続のセッション有効期間は、組織の設定で定義された設定期間が過ぎると期限切れになります。 デフォルトでは、この期間は 24 時間です。 方法については、ドキュメントを参照してください。 [期限切れでない資格情報でクライアントに接続](../ui/credentials.md#non-expiring-credentials)、または方法 [有効期限が切れる資格情報のセッション期間を変更する](../ui/credentials.md#expiring-credentials).

QS 資格情報にアクセスできたら、 [!DNL Python] 編集者を選択します。

### 資格情報をに保存 [!DNL Python] {#store-credentials}

を [!DNL Python] エディタ、インポート `urllib.parse.quote` ライブラリに追加し、各秘密鍵証明書変数をパラメーターとして保存します。 この `urllib.parse` モジュールは、URL 文字列をコンポーネントに分割するための標準的なインターフェイスを提供します。 quote 関数は、URL 文字列内の特殊文字を置き換えて、URL コンポーネントとしてデータを安全に使用できるようにします。 必要なコードの例を以下に示します。

>[!TIP]
>
>用途 [!DNL Python]複数行のパスワード文字列を入力するための三重引用符です。

```python
from urllib.parse import quote

host = "<YOUR_HOST>"

port = "<YOUR_PORT>"

dbname = "<YOUR_DATABASE>"

user = "<YOUR_USERNAME>"

password = quote('''
<YOUR_PASSWORD>
''')
```

>[!NOTE]
>
>接続に指定したパスワード [!DNL SQLAlchemy] をExperience Platformに追加すると、有効期限が切れます。 詳しくは、 [資格情報セクション](#credentials) を参照してください。

### エンジンインスタンスの作成 [#create-engine]

変数を作成したら、 `create_engine` 関数を作成し、SQLAlchemy でクエリサービス資格情報をコンパイルして書式設定する文字列を作成します。 この `create_engine` 関数は、その後、エンジンインスタンスの構築に使用されます。

>[!NOTE]
>
>`create_engine`エンジンのインスタンスを返します。 ただし、接続が必要なクエリが呼び出されるまで、クエリサービスへの接続は開きません。

サードパーティのクライアントを使用して Platform にアクセスする場合は、SSL を有効にする必要があります。 エンジンの一部として、 `connect_args` をクリックして、追加のキーワード引数を入力します。 SSL モードをに設定することをお勧めします。 `require`. 詳しくは、 [SSL モードのドキュメント](../clients/ssl-modes.md) を参照してください。

次の例は、 [!DNL Python] エンジンと接続文字列を初期化するために必要なコード。

```python
from sqlalchemy import create_engine

db_string = "postgresql://{user}:{password}@{host}:{port}/{dbname}".format(
    user=user,
    password=password,
    host=host,
    port = port,
    dbname = dbname
)

engine = create_engine(db_string, connect_args={'sslmode':'require'})
```

>[!NOTE]
>
>接続に指定したパスワード [!DNL SQLAlchemy] をExperience Platformに追加すると、有効期限が切れます。 詳しくは、 [資格情報セクション](#credentials) を参照してください。

これで、次を使用して Platform データに対してクエリを実行する準備が整いました： [!DNL Python]. 次の例は、クエリサービステーブル名の配列を返します。

```python
from sqlalchemy import inspect
insp = inspect(engine)
print(insp.get_table_names())
```
