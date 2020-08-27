---
keywords: Experience Platform;home;popular topics;data access;python sdk;data access api
solution: Experience Platform
title: セキュアPythonデータアクセスSDK
topic: tutorial
description: Secure Python Data Access SDKは、Adobe Experience Platformからのデータセットの読み取りと書き込みを可能にするソフトウェア開発キットです。
translation-type: tm+mt
source-git-commit: cddc559dfb65ada888bb367d6265863091a9b2a1
workflow-type: tm+mt
source-wordcount: '500'
ht-degree: 1%

---


# セキュア [!DNL Python][!DNL Data Access] SDK

Secure [!DNL Python][!DNL Data Access] SDKは、Adobe Experience Platformからのデータセットの読み取りと書き込みを可能にするソフトウェア開発キットです。

## はじめに

Secure [SDKを呼び出すための値にアクセスするには、次の](../../tutorials/authentication.md) 認証 [!DNL Python][!DNL Data Access] チュートリアルを完了する必要があります。

- `{ACCESS_TOKEN}`
- `{API_KEY}`
- `{IMS_ORG}`

All resources in [!DNL Experience Platform] are isolated to specific virtual sandboxes. SDKを使用するには、操作を実行するサンドボックスの名前とIDが必要です。 [!DNL Python]

- `{SANDBOX_NAME}`
- `{SANDBOX_ID}`

For more information on sandboxes in [!DNL Platform], see the [sandbox overview documentation](../../sandboxes/home.md).

## 環境設定

デフォルトでは、サービスエンドポイントは統合環境エンドポイントに設定されます。 その結果、実稼働環境を指すようにするには、次の環境変数を次の値に設定します。

| Variable | エンドポイント |
| -------- | -------- |
| `ENV_CATALOG_URL` | `https://platform.adobe.io/data/foundation/catalog/` |
| `ENV_QUERY_SERVICE_URL` | `https://platform.adobe.io/data/foundation/query` |
| `ENV_BULK_INGEST_URL` | `https://platform.adobe.io/data/foundation/import/` |
| `ENV_REGISTRY_URL` | `https://platform.adobe.io/data/foundation/schemaregistry/tenant/schemas` |

また、資格情報を環境変数として追加できます。

| Variable | 値 |
| -------- | ----- |
| `ORG_ID` | ID。 `{IMS_ORG}` |
| `SERVICE_API_KEY` | あなたの `{API_KEY}` 価値。 |
| `USER_TOKEN` | あなたの `{ACCESS_TOKEN}` 価値。 |
| `SERVICE_TOKEN` | お客様 `{SERVICE_TOKEN}`。サービス間のバックチャネル要求を承認する必要がある場合があります。 |
| `SANDBOX_ID` | サンドボックスの `{SANDBOX_ID}` 値。 |
| `SANDBOX_NAME` | サンドボックスの `{SANDBOX_NAME}` 値。 |

## インストール

すべてのパッケージは、構築 `./dist` 後に出力されます。

### ホイール

```python
python3 setup.py bdist_wheel --universal
```

プロジェクトディレクトリから、ホイールを [!DNL Python] 3環境にロードします。

```python
pip3 install ./dist/<name_of_wheel_file>.whl
```

### エッグファイル

```python
python3 setup.py bdist_egg
```

## データセットの読み取り

環境変数を設定し、インストールを完了した後、データセットをパンダのデータフレームに読み込めるようになりました。

```python
from platform_sdk.client_context import ClientContext
from platform_sdk.dataset_reader import DatasetReader

client_context = ClientContext(api_key={API_KEY},
                               org_id={IMS_ORG_ID},
                               service_token={SERVICE_TOKEN},
                               user_token={USER_TOKEN},
                               sandbox_id={SANDBOX_ID},
                               sandbox_name={SANDBOX_NAME})

dataset_reader = DatasetReader(client_context, {DATASET_ID})
df = dataset_reader.read()
```

### データセットからSELECT列を

```python
df = dataset_reader.select(['column-a','column-b']).read()
```

### パーティション情報の取得：

```python
client_context = ClientContext(api_key={API_KEY},
                               org_id={IMS_ORG_ID},
                               service_token={SERVICE_TOKEN},
                               user_token={USER_TOKEN},
                               sandbox_id={SANDBOX_ID},
                               sandbox_name={SANDBOX_NAME})

dataset = Dataset(client_context).get_by_id({DATASET_ID})
partitions = dataset.get_partitions_info()
```

### DISTINCT句

DISTINCT句を使用すると、行/列レベルですべての個別の値を取得し、応答からすべての重複値を削除できます。

この `distinct()` 関数の使用例を次に示します。

```python
df = dataset_reader.select(['column-a']).distinct().read()
```

### WHERE句

SDKは、データセットのフィルタリングに役立つ特定の演算子をサポートしています。 [!DNL Python]

>[!NOTE]
>
>フィルタリングに使用される関数は、大文字と小文字が区別されます。

```python
eq() = '='
gt() = '>'
ge() = '>='
lt() = '<'
le() = '<='
And = and operator
Or = or operator
```

これらのフィルター機能の使用例を次に示します。

```python
df = dataset_reader.where(experience_ds['timestamp'].gt(87879779797).And(experience_ds['timestamp'].lt(87879779797)).Or(experience_ds['a'].eq(123)))
```

### ORDER BY句

ORDER BY句を使用すると、受け取った結果を、指定した列で特定の順序（昇順または降順）で並べ替えることができます。 SDKでは、これは [!DNL Python] 関数を使用して行われ `sort()` ます。

この `sort()` 関数の使用例を次に示します。

```python
df = dataset_reader.sort([('column_1', 'asc'), ('column_2', 'desc')])
```

### LIMIT句

LIMIT句を使用すると、データセットから受け取るレコードの数を制限できます。

この `limit()` 関数の使用例を次に示します。

```python
df = dataset_reader.limit(100).read()
```

### OFFSET句

OFFSET句を使用すると、行を最初からスキップし、後から行を返す開始にスキップできます。 LIMITと組み合わせて使用すると、ブロック内の行を反復することができます。

この `offset()` 関数の使用例を次に示します。

```python
df = dataset_reader.offset(100).read()
```

## データセットの書き込み

SDKは、データセットの書き込みをサポートしています。 [!DNL Python] ユーザーは、データセットに書き込む必要があるパンダのデータフレームを提供する必要があります。

### パンダのデータフレームの書き込み

```python
client_context = ClientContext(api_key={API_KEY},
                               org_id={IMS_ORG_ID},
                               service_token={SERVICE_TOKEN},
                               user_token={USER_TOKEN},
                               sandbox_id={SANDBOX_ID},
                               sandbox_name={SANDBOX_NAME})

# To fetch existing dataset
dataset = Dataset(client_context).get_by_id({DATASET_ID})

dataset_writer = DatasetWriter(client_context, dataset)

write_tracker = dataset_writer.write(<dataFrame>, file_format='json')
```

## Userspaceディレクトリ（チェックポイント）

より長い時間実行するジョブの場合、ユーザーは中間ステップの保存が必要になる場合があります。 このようなインスタンスでは、 [!DNL Python] SDKはユーザーに対してユーザースペースの読み取りと書き込みを可能にします。

>!![NOTE] データへのパスは、SDKによって **保存されません** 。 ユーザーは、対応するデータのパスを保存する必要があります。

### ユーザースペースへの書き込み

```python
client_context = ClientContext(api_key={API_KEY},
                               org_id={IMS_ORG_ID},
                               service_token={SERVICE_TOKEN},
                               user_token={USER_TOKEN},
                               sandbox_id={SANDBOX_ID},
                               sandbox_name={SANDBOX_NAME})
                               
user_helper = UserSpaceHelper(client_context)
user_helper.write(data_frame=<data_frame>, path=<path_to_directory>, ref_dataset_id=<ref_dataset_id>)
```

### ユーザ空間から読み取る

```python
client_context = ClientContext(api_key={API_KEY},
                               org_id={IMS_ORG_ID},
                               service_token={SERVICE_TOKEN},
                               user_token={USER_TOKEN},
                               sandbox_id={SANDBOX_ID},
                               sandbox_name={SANDBOX_NAME})
                               
user_helper = UserSpaceHelper(client_context)
my_df = user_helper.read(path=<path_to_directory>, ref_dataset_id=<ref_dataset_id>)
```
