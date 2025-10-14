---
title: Python と SQLAlchemy を使用したExperience Platform データの管理
description: SQL ではなく Python を使用して、SQLAlchemy を使用してExperience Platform データを管理する方法を説明します。
exl-id: 9fba942e-9b3d-4efe-ae94-aed685025dea
source-git-commit: f129c215ebc5dc169b9a7ef9b3faa3463ab413f3
workflow-type: tm+mt
source-wordcount: '578'
ht-degree: 0%

---

# [!DNL Python] と [!DNL SQLAlchemy] を使用したExperience Platform データの管理

SQLAlchemy を使用してAdobe Experience Platform データの管理をより柔軟に行う方法を説明します。 SQL に詳しくないユーザーにとって、SQLAlchemy は、リレーショナル・データベースを使用する際の開発時間を大幅に短縮できます。 このドキュメントでは、[!DNL SQLAlchemy] をクエリサービスに接続し、Python を使用してデータベースとやり取りする手順と例を説明します。

[!DNL SQLAlchemy] は、SQL データベースに格納されたデータを [!DNL Python] のオブジェクトに転送できる ORM （Object Relational Mapper）および [!DNL Python] コード ライブラリです。 その後、コードを使用して、Experience Platform データレイク内に保持されているデータに対して CRUD 操作 [!DNL Python] 実行できます。 これにより、PSQL のみを使用してデータを管理する必要がなくなります。

## はじめに

[!DNL SQLAlchemy] をExperience Platformに接続するために必要な資格情報を取得するには、Experience Platform UI のクエリワークスペースにアクセスできる必要があります。 現在、クエリワークスペースにアクセスできない場合は、組織の管理者にお問い合わせください。

## [!DNL Query Service] 資格情報 {#credentials}

資格情報を見つけるには、Experience Platform UI にログインし、左側のナビゲーションから **[!UICONTROL クエリ]** を選択し、続いて **[!UICONTROL 資格情報]** を選択します。 ログイン資格情報の検索方法の詳細については、[&#x200B; 資格情報ガイド &#x200B;](../ui/credentials.md) を参照してください。

![&#x200B; クエリサービスの資格情報の有効期限が切れる「資格情報」タブがハイライト表示されます。](../images/use-cases/credentials.png)

クエリサービスへの接続にはポート 80 をお勧めしますが、ポート 5432 を使用することもできます。

>[!IMPORTANT]
>
>有効期限が切れる資格情報（上図を参照）を使用してクエリサービスに接続する場合、接続のセッション有効期間は、組織の設定で定義された期間の後に期限切れになります。 デフォルトでは、この期間は 24 時間です。 [&#x200B; 有効期限のない資格情報を使用してクライアントを接続 &#x200B;](../ui/credentials.md#non-expiring-credentials) する方法や、有効期限のある資格情報のセッション期間を変更 [&#x200B; する方法については、ドキュメントを参照してください &#x200B;](../ui/credentials.md#expiring-credentials)。

QS 認証情報にアクセスできるようになったら、任意の [!DNL Python] エディターを開きます。

### [!DNL Python] に資格情報を保存 {#store-credentials}

[!DNL Python] エディターで、`urllib.parse.quote` ライブラリを読み込み、各資格情報変数をパラメーターとして保存します。 `urllib.parse` モジュールは、URL 文字列をコンポーネントに分割する標準的なインターフェイスを提供します。 quote 関数は、URL 文字列内の特殊文字を置き換えて、データを URL コンポーネントとして安全に使用できるようにします。 必要なコードの例を以下に示します。

>[!TIP]
>
>[!DNL Python] の 3 重引用符を使用して、複数行のパスワード文字列を入力します。

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
>有効期限が切れる資格情報を使用する場合、[!DNL SQLAlchemy] をExperience Platformに接続するために指定したパスワードの有効期限が切れます。 詳しくは、[&#x200B; 資格情報の節 &#x200B;](#credentials) を参照してください。

### エンジンインスタンスの作成 [#create-engine]

変数を作成した後、`create_engine` 関数をインポートし、SQLAlchemy でクエリサービスの認証情報をコンパイルしてフォーマットするための文字列を作成します。 次に、`create_engine` 関数を使用してエンジンインスタンスが作成されます。

>[!NOTE]
>
>`create_engine` エンジンのインスタンスを返します。 ただし、接続を必要とするクエリが呼び出されるまで、クエリサービスへの接続は開きません。

サードパーティのクライアントを使用してExperience Platformにアクセスする場合は、SSL を有効にする必要があります。 エンジンの一部として、`connect_args` を使用して追加のキーワード引数を入力します。 SSL モードを `require` に設定することをお勧めします。 使用可能な値について詳しくは、[SSL モードのドキュメント &#x200B;](../clients/ssl-modes.md) を参照してください。

次の例は、エンジンと接続文字列を初期化するために必要な [!DNL Python] コードを示しています。

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
>有効期限が切れる資格情報を使用する場合、[!DNL SQLAlchemy] をExperience Platformに接続するために指定したパスワードの有効期限が切れます。 詳しくは、[&#x200B; 資格情報の節 &#x200B;](#credentials) を参照してください。

これで、[!DNL Python] を使用してExperience Platform データをクエリする準備が整いました。 次の例では、クエリサービスのテーブル名の配列を返します。

```python
from sqlalchemy import inspect
insp = inspect(engine)
print(insp.get_table_names())
```
