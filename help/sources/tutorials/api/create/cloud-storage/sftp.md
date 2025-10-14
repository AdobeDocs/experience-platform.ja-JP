---
title: Flow Service API を使用した SFTP ベース接続の作成
description: Flow Service API を使用してAdobe Experience Platformを SFTP （Secure File Transfer Protocol）サーバーに接続する方法について説明します。
exl-id: b965b4bf-0b55-43df-bb79-c89609a9a488
source-git-commit: 4816a6b627dc6551e351bfe3cdc4bc8c8ea8b17e
workflow-type: tm+mt
source-wordcount: '751'
ht-degree: 14%

---

# [!DNL Flow Service] API を使用した SFTP ベース接続の作成

ベース接続は、ソースと Adobe Experience Platform 間の認証済み接続を表します。

このチュートリアルでは、[[!DNL Flow Service] API](https://www.adobe.io/experience-platform-apis/references/flow-service/) を使用して [!DNL SFTP] （セキュアファイル転送プロトコル）のベース接続を作成する手順について説明します。

## はじめに

このガイドでは、Adobe Experience Platform の次のコンポーネントに関する十分な知識が必要です。

* [&#x200B; ソース &#x200B;](../../../../home.md):Experience Platformを使用すると、データを様々なソースから取得しながら、Experience Platform サービスを使用して受信データの構造化、ラベル付け、拡張を行うことができます。
* [&#x200B; サンドボックス &#x200B;](../../../../../sandboxes/home.md): Experience Platformには、1 つのExperience Platform インスタンスを別々の仮想環境に分割し、デジタルエクスペリエンスアプリケーションの開発と発展に役立つ仮想サンドボックスが用意されています。

>[!IMPORTANT]
>
>[!DNL SFTP] ソース接続を持つ JSON オブジェクトを取り込む場合は、改行や改行を避けることをお勧めします。 この制限を回避するには、1 行に 1 つの JSON オブジェクトを使用し、複数の行を使用してファイルを送信します。

次の節では、[!DNL Flow Service] API を使用して [!DNL SFTP] サーバーに正常に接続するために必要な追加情報を示しています。

### 必要な資格情報の収集

認証資格情報の取得方法の手順について詳しくは、[[!DNL SFTP]  認証ガイド &#x200B;](../../../../connectors/cloud-storage/sftp.md#gather-required-credentials) を参照してください。

### Experience Platform API の使用

Experience Platform API を正常に呼び出す方法について詳しくは、[Experience Platform API の概要 &#x200B;](../../../../../landing/api-guide.md) を参照してください。

## ベース接続の作成

>[!TIP]
>
>作成した後は、[!DNL SFTP] ベース接続の認証タイプを変更できません。 認証タイプを変更するには、新しいベース接続を作成する必要があります。

ベース接続は、ソースとExperience Platform間の情報（ソースの認証資格情報、現在の接続状況、一意のベース接続 ID など）を保持します。 ベース接続 ID により、ソース内からファイルを参照および移動し、データタイプやフォーマットに関する情報を含む、取り込みたい特定の項目を識別することができます。

[!DNL SFTP] ソースは、基本認証と SSH 公開鍵を使用した認証の両方をサポートしています。 この手順では、アクセス権を付与するサブフォルダーへのパスを指定することもできます。

ベース接続 ID を作成するには、`/connections` エンドポイントに POST リクエストを実行し、[!DNL SFTP] 認証資格情報をリクエストパラメーターの一部として使用します。

>[!IMPORTANT]
>
>[!DNL SFTP] コネクタは、`ed25519`、`RSA` または `DSA` タイプの OpenSSH キーをサポートしています。 主要なファイルの内容が `"-----BEGIN [RSA/DSA] PRIVATE KEY-----"` で始まり、`"-----END [RSA/DSA] PRIVATE KEY-----"` で終わることを確認してください。 秘密鍵ファイルが PPK 形式のファイルの場合は、PuTTY ツールを使用して、PPK 形式から OpenSSH 形式に変換します。

**API 形式**

```http
POST /connections
```

>[!BEGINTABS]

>[!TAB  基本認証 ]

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
              "folderPath": "acme/business/customers/holidaySales",
              "disableChunking": "true"
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
| `auth.params.port` | SFTP サーバーのポート。 この整数値のデフォルトは 22 です。 |
| `auth.params.username` | SFTP サーバーに関連付けられたユーザー名。 |
| `auth.params.password` | SFTP サーバーに関連付けられたパスワード。 |
| `auth.params.maxConcurrentConnections` | Experience Platformを SFTP に接続する際に指定した同時接続の最大数。 有効にする場合、この値は 1 以上に設定する必要があります。 |
| `auth.params.folderPath` | アクセス権を付与するフォルダーへのパス。 |
| `auth.params.disableChunking` | SFTP サーバーがチャンクをサポートするかどうかを決定するために使用されるブール値です。 |
| `connectionSpec.id` | SFTP サーバー接続仕様 ID:`b7bf2577-4520-42c9-bae9-cad01560f7bc` |

+++

+++応答

リクエストが成功した場合は、新しく作成した接続の一意の ID （`id`）が返されます。 この ID は、次のチュートリアルで SFTP サーバーを探索するために必要になります。

```json
{
    "id": "bf367b0d-3d9b-4060-b67b-0d3d9bd06094",
    "etag": "\"1700cc7b-0000-0200-0000-5e3b3fba0000\""
}
```

+++

>[!TAB SSH 公開鍵認証 ]

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
              "folderPath": "acme/business/customers/holidaySales",
              "disableChunking": "true"
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
| `auth.params.host` | [!DNL SFTP] サーバーのホスト名。 |
| `auth.params.port` | SFTP サーバーのポート。 この整数値のデフォルトは 22 です。 |
| `auth.params.username` | [!DNL SFTP] サーバーに関連付けられたユーザー名。 |
| `auth.params.privateKeyContent` | Base64 にエンコードされた SSH 秘密鍵の内容。 サポートされている OpenSSH キータイプは、`ed25519`、`RSA`、`DSA` です。 |
| `auth.params.passPhrase` | キーファイルまたはキーの内容がパスフレーズによって保護されている場合に秘密鍵を復号化するためのパスフレーズまたはパスワード。 PrivateKeyContent がパスワードで保護されている場合、このパラメーターは、PrivateKeyContent のパスフレーズを値として使用する必要があります。 |
| `auth.params.maxConcurrentConnections` | Experience Platformを SFTP に接続する際に指定した同時接続の最大数。 有効にする場合、この値は 1 以上に設定する必要があります。 |
| `auth.params.folderPath` | アクセス権を付与するフォルダーへのパス。 |
| `auth.params.disableChunking` | SFTP サーバーがチャンクをサポートするかどうかを決定するために使用されるブール値です。 |
| `connectionSpec.id` | [!DNL SFTP] サーバー接続仕様 ID: `b7bf2577-4520-42c9-bae9-cad01560f7bc` |

+++

+++応答

リクエストが成功した場合は、新しく作成した接続の一意の ID （`id`）が返されます。 この ID は、次のチュートリアルで SFTP サーバーを探索するために必要になります。

```json
{
    "id": "bf367b0d-3d9b-4060-b67b-0d3d9bd06094",
    "etag": "\"1700cc7b-0000-0200-0000-5e3b3fba0000\""
}
```

+++

>[!ENDTABS]

## 次の手順

このチュートリアルでは、[!DNL Flow Service] API を使用して [!DNL SFTP] 接続を作成し、接続の一意の ID 値を取得しました。 この接続 ID を使用して [Flow Service API を使用したクラウドストレージの調査 &#x200B;](../../explore/cloud-storage.md) を行うことができます。
