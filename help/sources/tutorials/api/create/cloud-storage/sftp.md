---
title: フローサービス API を使用した SFTP ベース接続の作成
description: フローサービス API を使用して、Adobe Experience Platformを SFTP(Secure File Transfer Protocol) サーバーに接続する方法について説明します。
exl-id: b965b4bf-0b55-43df-bb79-c89609a9a488
source-git-commit: a826bda356a7205f3d4c0e0836881530dbaaf54e
workflow-type: tm+mt
source-wordcount: '938'
ht-degree: 28%

---

# を使用した SFTP ベース接続の作成 [!DNL Flow Service] API

ベース接続は、ソースと Adobe Experience Platform 間の認証済み接続を表します。

このチュートリアルでは、のベース接続を作成する手順を説明します。 [!DNL SFTP] （セキュアファイル転送プロトコル） [[!DNL Flow Service] API](https://www.adobe.io/experience-platform-apis/references/flow-service/).

## はじめに

このガイドでは、Adobe Experience Platform の次のコンポーネントに関する十分な知識が必要です。

* [ソース](../../../../home.md)：Experience Platform を使用すると、様々なソースからデータを取得しながら、Platform サービスを使用して受信データの構造化、ラベル付け、拡張を行うことができます。
* [サンドボックス](../../../../../sandboxes/home.md)：Experience Platform には、単一の Platform インスタンスを別々の仮想環境に分割し、デジタルエクスペリエンスアプリケーションの開発と発展に役立つ仮想サンドボックスが用意されています。

>[!IMPORTANT]
>
>JSON オブジェクトを [!DNL SFTP] ソース接続。 この制限を回避するには、1 行に 1 つの JSON オブジェクトを使用し、その後のファイルに複数行を使用します。

以下の節では、 [!DNL SFTP] サーバーの [!DNL Flow Service] API.

### 必要な資格情報の収集

[!DNL Flow Service] を [!DNL SFTP] に接続するには、次の接続プロパティの値を指定する必要があります。

| 資格情報 | 説明 |
| ---------- | ----------- |
| `host` | に関連付けられている名前または IP アドレス [!DNL SFTP] サーバー。 |
| `port` | 接続先の SFTP サーバーポート。 指定しない場合、値はデフォルトでになります。 `22`. |
| `username` | へのアクセス権を持つユーザー名 [!DNL SFTP] サーバー。 |
| `password` | ユーザーのパスワード [!DNL SFTP] サーバー。 |
| `privateKeyContent` | Base64 でエンコードされた SSH 秘密鍵コンテンツ。 OpenSSH キーのタイプは、RSA または DSA に分類する必要があります。 |
| `passPhrase` | キーファイルまたはキーコンテンツがパスフレーズで保護されている場合に秘密鍵を復号化するためのパスフレーズまたはパスワード。 次の場合、 `privateKeyContent` はパスワードで保護されているので、このパラメーターは秘密鍵コンテンツのパスフレーズを値として使用する必要があります。 |
| `maxConcurrentConnections` | このパラメーターを使用すると、SFTP サーバーへの接続時に Platform が作成する同時接続数の上限を指定できます。 この値は、SFTP が設定した制限を下回るように設定する必要があります。 **注意**：この設定を既存の SFTP アカウントに対して有効にすると、今後のデータフローにのみ影響し、既存のデータフローには影響しません。 |
| `folderPath` | アクセス権を付与するフォルダーのパスです。 [!DNL SFTP] ソースの場合は、選択したサブフォルダーへのユーザーアクセスを指定するためのフォルダーパスを指定できます。 |
| `connectionSpec.id` | 接続仕様は、ベース接続とソース接続の作成に関連する認証仕様などの、ソースのコネクタプロパティを返します。[!DNL SFTP] の接続仕様 ID は `b7bf2577-4520-42c9-bae9-cad01560f7bc` です。 |

### Platform API の使用

Platform API への呼び出しを正常に実行する方法について詳しくは、[Platform API の概要](../../../../../landing/api-guide.md)を参照してください。

## ベース接続の作成

>[!TIP]
>
>作成後は、 [!DNL Dynamics] ベース接続。 認証タイプを変更するには、新しいベース接続を作成する必要があります。

ベース接続は、ソースと Platform 間の情報（ソースの認証資格情報、現在の接続状態、固有のベース接続 ID など）を保持します。ベース接続 ID により、ソース内からファイルを参照および移動し、データタイプやフォーマットに関する情報を含む、取り込みたい特定の項目を識別することができます。

The [!DNL SFTP] ソースは、SSH 公開鍵を介した基本認証と認証の両方をサポートします。 この手順の間に、アクセスを許可するサブフォルダーのパスを指定することもできます。

ベース接続 ID を作成するには、`/connections` エンドポイントに POST リクエストを実行し、[!DNL SFTP] 認証資格情報をリクエストパラメーターの一部として使用します。

>[!IMPORTANT]
>
>The [!DNL SFTP] コネクタは、RSA または DSA タイプの OpenSSH キーをサポートします。 鍵となるファイルコンテンツが `"-----BEGIN [RSA/DSA] PRIVATE KEY-----"` およびで終わる `"-----END [RSA/DSA] PRIVATE KEY-----"`. 秘密鍵ファイルが PPK 形式のファイルの場合は、PuTTY ツールを使用して PPK から OpenSSH 形式に変換します。

**API 形式**

```http
POST /connections
```

>[!BEGINTABS]

>[!TAB 基本認証]

+++リクエスト

```shell
curl -X POST \
  'https://platform.adobe.io/data/foundation/flowservice/connections' \
  -H 'Authorization: Bearer {ACCESS_TOKEN}' \
  -H 'x-api-key: {API_KEY}' \
  -H 'x-gw-ims-org-id: {ORG_ID}' \
  -H 'x-sandbox-name: {SANDBOX_NAME}' \
  -H 'Content-Type: application/json' \
  -d  '{
      "name": "SFTP connector with password",
      "description": "SFTP connector password",
      "auth": {
          "specName": "Basic Authentication for sftp",
          "params": {
              "host": "{HOST}",
              "port": 22,
              "userName": "{USERNAME}",
              "password": "{PASSWORD}",
              "maxConcurrentConnections": 5,
              "folderPath": "acme/business/customers/holidaySales"
          }
      },
      "connectionSpec": {
          "id": "b7bf2577-4520-42c9-bae9-cad01560f7bc",
          "version": "1.0"
      }
  }'
```

| プロパティ | 説明 |
| -------- | ----------- |
| `auth.params.host` | SFTP サーバーのホスト名。 |
| `auth.params.port` | SFTP サーバーのポート。 この整数値のデフォルト値は 22 です。 |
| `auth.params.username` | SFTP サーバーに関連付けられたユーザー名。 |
| `auth.params.password` | SFTP サーバーに関連付けられたパスワード。 |
| `auth.params.maxConcurrentConnections` | Platform を SFTP に接続する際に指定された同時接続の最大数です。 有効にした場合、この値は少なくとも 1 に設定する必要があります。 |
| `auth.params.folderPath` | アクセス権を付与するフォルダーのパスです。 |
| `connectionSpec.id` | SFTP サーバー接続仕様 ID: `b7bf2577-4520-42c9-bae9-cad01560f7bc` |

+++

+++応答

正常な応答は、一意の識別子 (`id`) に含まれます。 この ID は、次のチュートリアルで SFTP サーバーを調べるために必要です。

```json
{
    "id": "bf367b0d-3d9b-4060-b67b-0d3d9bd06094",
    "etag": "\"1700cc7b-0000-0200-0000-5e3b3fba0000\""
}
```

+++

>[!TAB SSH 公開鍵認証]

+++リクエスト

```shell
curl -X POST \
  'https://platform.adobe.io/data/foundation/flowservice/connections' \
  -H 'Authorization: Bearer {ACCESS_TOKEN}' \
  -H 'x-api-key: {API_KEY}' \
  -H 'x-gw-ims-org-id: {ORG_ID}' \
  -H 'x-sandbox-name: {SANDBOX_NAME}' \
  -H 'Content-Type: application/json' \
  -d '{
      "name": "SFTP connector with SSH authentication",
      "description": "SFTP connector with SSH authentication",
      "auth": {
          "specName": "SSH PublicKey Authentication for sftp",
          "params": {
              "host": "{HOST}",
              "port": 22,
              "userName": "{USERNAME}",
              "privateKeyContent": "{PRIVATE_KEY_CONTENT}",
              "passPhrase": "{PASSPHRASE}",
              "maxConcurrentConnections": 5,
              "folderPath": "acme/business/customers/holidaySales"
          }
      },
      "connectionSpec": {
          "id": "b7bf2577-4520-42c9-bae9-cad01560f7bc",
          "version": "1.0"
      }
  }'
```

| プロパティ | 説明 |
| -------- | ----------- |
| `auth.params.host` | のホスト名 [!DNL SFTP] サーバー。 |
| `auth.params.port` | SFTP サーバーのポート。 この整数値のデフォルト値は 22 です。 |
| `auth.params.username` | に関連付けられたユーザー名 [!DNL SFTP] サーバー。 |
| `auth.params.privateKeyContent` | Base64 でエンコードされた SSH 秘密鍵コンテンツ。 OpenSSH キーのタイプは、RSA または DSA に分類する必要があります。 |
| `auth.params.passPhrase` | キーファイルまたはキーコンテンツがパスフレーズで保護されている場合に秘密鍵を復号化するためのパスフレーズまたはパスワード。 PrivateKeyContent がパスワードで保護されている場合、このパラメーターを PrivateKeyContent のパスフレーズと共に値として使用する必要があります。 |
| `auth.params.maxConcurrentConnections` | Platform を SFTP に接続する際に指定された同時接続の最大数です。 有効にした場合、この値は少なくとも 1 に設定する必要があります。 |
| `auth.params.folderPath` | アクセス権を付与するフォルダーのパスです。 |
| `connectionSpec.id` | The [!DNL SFTP] サーバ接続仕様 ID: `b7bf2577-4520-42c9-bae9-cad01560f7bc` |

+++

+++応答

正常な応答は、一意の識別子 (`id`) に含まれます。 この ID は、次のチュートリアルで SFTP サーバーを調べるために必要です。

```json
{
    "id": "bf367b0d-3d9b-4060-b67b-0d3d9bd06094",
    "etag": "\"1700cc7b-0000-0200-0000-5e3b3fba0000\""
}
```

+++

>[!ENDTABS]

## 次の手順

このチュートリアルに従って、 [!DNL SFTP] を使用した接続 [!DNL Flow Service] API で、接続の一意の ID 値を取得している。 この接続 ID を [フローサービス API を使用したクラウドストレージの調査](../../explore/cloud-storage.md).
