---
solution: Experience Platform
title: クラウドストレージ宛先の API 移行ガイド
description: 追加機能を備えた新しいクラウドストレージの宛先カードへの移行の一環として、クラウドストレージの宛先をアクティブ化するワークフローの変更について説明します。
type: Tutorial
exl-id: 4acaf718-794e-43a3-b8f0-9b19177a2bc0
source-git-commit: 4b9e7c22282a5531f2f25f3d225249e4eb0e178e
workflow-type: tm+mt
source-wordcount: '1334'
ht-degree: 2%

---

# クラウドストレージ宛先の API 移行ガイド

>[!IMPORTANT]
>
>* このページで説明される機能は、Real-Time CDP Prime および Ultimate パッケージを購入したお客様が利用できます。 詳しくは、アドビ担当者にお問い合わせください。

## 移行コンテキスト {#migration-context}

[2022 年 10 月 &#x200B;](/help/release-notes/2022/october-2022.md#new-or-updated-destinations) 以降は、新しいファイル書き出し機能を使用して、Experience Platformからファイルを書き出す際に拡張カスタマイズ機能にアクセスできます。

* 追加の[ファイル命名オプション](/help/destinations/ui/activate-batch-profile-destinations.md#file-names)。
* [&#x200B; 新しいマッピングステップ &#x200B;](/help/destinations/ui/activate-batch-profile-destinations.md#mapping) を使用して、書き出すファイルにカスタムファイルヘッダーを設定する機能。
* 書き出されたファイルの [&#x200B; ファイルタイプ &#x200B;](/help/destinations/ui/connect-destination.md#file-formatting-and-compression-options) を選択する機能。
* [&#x200B; 書き出された CSV データファイルの形式をカスタマイズ &#x200B;](/help/destinations/ui/batch-destinations-file-formatting-options.md) する機能。

この機能は、以下に示すベータ版のクラウドストレージカードでサポートされています。

* [[!DNL (Beta) Amazon S3]](../../destinations/catalog/cloud-storage/amazon-s3.md#changelog)
* [[!DNL (Beta) Azure Blob]](../../destinations/catalog/cloud-storage/azure-blob.md#changelog)
* [[!DNL (Beta) SFTP]](../../destinations/catalog/cloud-storage/sftp.md#changelog)

<!--

Commenting out the three net new cloud storage destinations

* [[!DNL (Beta) Azure Data Lake Storage Gen2]](../../destinations/catalog/cloud-storage/adls-gen2.md)
* [[!DNL (Beta) Data Landing Zone]](../../destinations/catalog/cloud-storage/data-landing-zone.md)
* [[!DNL (Beta) Google Cloud Storage]](../../destinations/catalog/cloud-storage/google-cloud-storage.md)

-->

現在、Experience PlatformUI では、3 つの宛先のうち 2 つを並べて表示できます。 次に、[!DNL Amazon S3] の従来の宛先と新しい宛先を示します。 いずれの場合も、**Beta** が付いているカードが新しい出力先カードになります。

![2 つの Amazon S3 の宛先カードを並べて表示した画像。](../assets/catalog/cloud-storage/amazon-s3/two-amazons3-destination-cards.png)

機能が強化されたこれらの宛先は、最初はベータ版として提供されていましたが、*Adobeでは、現在、すべてのReal-Time CDPのお客様を新しいクラウドストレージの宛先に移行してい* す。 既に [!DNL Amazon S3]、[!DNL Azure Blob] または SFTP を使用していた顧客の場合、これは既存のデータフローが新しいカードに移行されることを意味します。 移行の一環としての具体的な変更点について詳しくは、こちらを参照してください。

## このページの適用先 {#who-this-applies-to}

既に [Flow Service API](https://developer.adobe.com/experience-platform-apis/references/destinations/) を使用してプロファイルをAmazon S3、Azure Blob または SFTP クラウドストレージの宛先に書き出している場合は、この API 移行ガイドが適用されます。

Experience Platformから書き出したファイルの上部で、[!DNL Amazon S3]、[!DNL Azure Blob]、SFTP クラウドストレージの場所でスクリプトが実行されている場合、新しいカードの接続とフローの仕様、およびマッピングステップに関して、一部のパラメーターが変化していることに注意してください。

例えば、[!DNL Amazon S3] 宛先への宛先データフローをフィルタリングするスクリプトを使用している場合、[!DNL Amazon S3] 宛先の接続仕様に基づいて、接続仕様が変更されるので、フィルターを更新する必要があります。

## 関連ドキュメントのリンク {#relevant-documentation-links}

この節では、関連する API チュートリアルと、クラウドストレージの宛先にデータを書き出すための拡張機能のリファレンスドキュメントを示します。

<!--

TBD if we keep this link but will likely remove it

[Legacy API tutorial to export data to cloud storage destinations](/help/destinations/api/connect-activate-batch-destinations.md) (outdated, do not use anymore)

-->
* [オーディエンスをクラウドストレージの宛先に書き出す API チュートリアル](/help/destinations/api/activate-segments-file-based-destinations.md)
* [Destinations Flow Service API リファレンスドキュメント &#x200B;](https://developer.adobe.com/experience-platform-apis/references/destinations/)

## 後方互換性のない変更の概要 {#summary-backwards-incompatible-changes}

新しい宛先への移行に伴い、[!DNL Amazon S3]、[!DNL Azure Blob]、SFTP の各宛先への既存のデータフローはすべて、新しいターゲット接続とベース接続に割り当てられます。 プロファイルマッピングの手順も変更されます。 後方互換性のない変更については、各宛先に関して以下の節で要約します。 以下の図の用語について詳しくは、[&#x200B; 宛先の用語集 &#x200B;](https://developer.adobe.com/experience-platform-apis/references/destinations/#tag/Glossary) も参照してください。

![&#x200B; 移行ガイドの概要画像 &#x200B;](/help/destinations/assets/api/api-migration-guide/migration-guide-diagram.png)

### [!DNL Amazon S3] の宛先に対する後方互換性のない変更 {#changes-amazon-s3-destination}

次の表に示すように、API ユーザーに対する後方互換性のない変更は更新された `connection spec ID` であり、`flow spec ID` です。

| [!DNL Amazon S3] | レガシー | 新規 |
|---------|----------|---------|
| フロー仕様 | 71471eba-b620-49e4-90fd-23f1fa0174d8 | 1a0514a6-33d4-4c7f-aff8-594799c47549 |
| 接続仕様 | 4890fc95-5a1f-4983-94bb-e060c08e3f81 | 4fce964d-3f37-408f-9778-e597338a21ee |

以下のタブに、[!DNL Amazon S3] のレガシー接続と新規ベース接続の完全な例とターゲット接続の例が表示されます。 [!DNL Amazon S3] の宛先のベース接続を作成するために必要なパラメーターは変更されません。

同様に、ターゲット接続の作成に必要なパラメーターには、後方互換性のない変更はありません。

>[!BEGINTABS]

>[!TAB  レガシーベース接続とターゲット接続 ]

+++[!DNL Amazon S3] のレガシ [!DNL base connection] を表示

```json {line-numbers="true" start-line="1" highlight="5"}
{
  ...
  "name": "amazon-s3",
  "connectionSpec": {
    "id": "4890fc95-5a1f-4983-94bb-e060c08e3f81",
    "version": "1.0"
  },
  "state": "enabled",
  "auth": {
    "specName": "Access Key",
    "params": {
      "authorizedDate": "2022-10-26",
      "s3SecretKey": "<your-secret-key>",
      "s3AccessKey": "<your-access-key>"
    }
  },
  "encryption": {
    "specName": "File Encryption",
    "params": {
      "encryptionAlgo": "PGP/GPG",
      "publicKey": "<publicKey>"
    }
  },
  "version": "\"640418e2-0000-0200-0000-6359b9ef0000\"",
  "etag": "\"640418e2-0000-0200-0000-6359b9ef0000\""
}
```

+++

+++[!DNL Amazon S3] のレガシ [!DNL target connection] を表示

```json {line-numbers="true" start-line="1" highlight="12"}
{
  ...
  "name": "test 121",
  "baseConnectionId": "ee86d122-10d3-434b-81c7-7252e4d747a7",
  "state": "enabled",
  "data": {
    "format": "CSV",
    "schema": null,
    "properties": null
  },
  "connectionSpec": {
    "id": "4890fc95-5a1f-4983-94bb-e060c08e3f81",
    "version": "1.0"
  },
  "params": {
    "mode": "S3",
    "path": "testpath",
    "bucketName": "test"
  },
  "version": "\"1609cd86-0000-0200-0000-63892cbb0000\"",
  "etag": "\"1609cd86-0000-0200-0000-63892cbb0000\"",
  "inheritedAttributes": {
    "baseConnection": {
      "id": "ee86d122-10d3-434b-81c7-7252e4d747a7",
      "connectionSpec": {
        "id": "4890fc95-5a1f-4983-94bb-e060c08e3f81",
        "version": "1.0"
      }
    }
  }
}
```

+++

>[!TAB  新しいベース接続とターゲット接続 ]

+++[!DNL Amazon S3] の新しい [!DNL base connection] を表示する

```json {line-numbers="true" start-line="1" highlight="5"}
{
  ...
  "name": "Amazon S3",
  "connectionSpec": {
    "id": "4fce964d-3f37-408f-9778-e597338a21ee",
    "version": "1.0"
  },
  "state": "enabled",
  "auth": {
    "specName": "Access Key",
    "params": {
      "authorizedDate": "2022-10-26",
      "s3SecretKey": "<your-secret-key>",
      "s3AccessKey": "<your-access-key>"
    }
  },
  "encryption": {
    "specName": "File Encryption",
    "params": {
      "encryptionAlgo": "PGP/GPG",
      "publicKey": "<publicKey>"
    }
  },
  "version": "\"3708da21-0000-0200-0000-638940b10000\"",
  "etag": "\"3708da21-0000-0200-0000-638940b10000\""
}
```

+++

+++[!DNL Amazon S3] の新しい [!DNL target connection] を表示する

```json {line-numbers="true" start-line="1" highlight="12, 16-27"}
{
  ...
  "name": "test 121",
  "baseConnectionId": "d114c86f-fd47-4bb6-846c-cb1d15a00fe9",
  "state": "enabled",
  "data": {
    "format": "CSV",
    "schema": null,
    "properties": null
  },
  "connectionSpec": {
    "id": "4fce964d-3f37-408f-9778-e597338a21ee",
    "version": "1.0"
  },
  "params": {
    "csvOptions": {
      "nullValue": "null",
      "emptyValue": "",
      "escape": "\\",
      "quote": "",
      "delimiter": ","
    },
    "compression": "NONE",
    "fileType": "CSV",
    "mode": "Server-to-server",
    "path": "testpath",
    "bucketName": "test"
  },
  "version": "\"1b0985c6-0000-0200-0000-638940b10000\"",
  "etag": "\"1b0985c6-0000-0200-0000-638940b10000\"",
  "inheritedAttributes": {
    "baseConnection": {
      "id": "d114c86f-fd47-4bb6-846c-cb1d15a00fe9",
      "connectionSpec": {
        "id": "4fce964d-3f37-408f-9778-e597338a21ee",
        "version": "1.0"
      }
    }
  }
}
```

+++

>[!ENDTABS]

### [!DNL Azure Blob] の宛先に対する後方互換性のない変更 {#changes-azure-blob-destination}

次の表に示すように、API ユーザーに対する後方互換性のない変更は更新された `connection spec ID` であり、`flow spec ID` です。

| [!DNL Azure Blob] | レガシー | 新規 |
|---------|----------|---------|
| フロー仕様 | 71471eba-b620-49e4-90fd-23f1fa0174d8 | 752d422f-b16f-4f0d-b1c6-26e448e3b388 |
| 接続仕様 | e258278b-a4cf-43ac-b158-4fa0ca0d948b | 6d6b59bf-fb58-4107-9064-4d246c0e5bb2 |

以下のタブに、[!DNL Azure Blob] のレガシー接続と新規ベース接続の完全な例とターゲット接続の例が表示されます。 Azure Blob 宛先のベース接続を作成するために必要なパラメーターは変更されません。

同様に、ターゲット接続の作成に必要なパラメーターには、後方互換性のない変更はありません。

>[!BEGINTABS]

>[!TAB  レガシーベース接続とターゲット接続 ]

+++[!DNL Azure Blob] のレガシ [!DNL base connection] を表示

```json {line-numbers="true" start-line="1" highlight="5"}
{
  ...
  "name": "azure-blob",
  "connectionSpec": {
    "id": "e258278b-a4cf-43ac-b158-4fa0ca0d948b",
    "version": "1.0"
  },
  "state": "enabled",
  "auth": {
    "specName": "ConnectionString",
    "params": {
      "authorizedDate": "2022-06-02",
      "connectionString": "<your-connection-string>"
    }
  },
  "encryption": {
    "specName": "File Encryption",
    "params": {
      "encryptionAlgo": "PGP/GPG",
      "publicKey": "<publicKey>"
    }
  }, 
  "version": "\"d000d23c-0000-0200-0000-6299051c0000\"",
  "etag": "\"d000d23c-0000-0200-0000-6299051c0000\""
}
```

+++

+++[!DNL Azure Blob] のレガシ [!DNL target connection] を表示

```json {line-numbers="true" start-line="1" highlight="13"}
{
  ...
  "name": "v1",
  "description": "v2",
  "baseConnectionId": "d10fcecf-9963-4062-820c-0f878be98805",
  "state": "enabled",
  "data": {
    "format": "CSV",
    "schema": null,
    "properties": null
  },
  "connectionSpec": {
    "id": "e258278b-a4cf-43ac-b158-4fa0ca0d948b",
    "version": "1.0"
  },
  "params": {
    "mode": "AZURE_BLOB",
    "container": "usdasda",
    "path": "v3"
  },
  "version": "\"cb0468ba-0000-0200-0000-631ab0790000\"",
  "etag": "\"cb0468ba-0000-0200-0000-631ab0790000\"",
  "inheritedAttributes": {
    "baseConnection": {
      "id": "d10fcecf-9963-4062-820c-0f878be98805",
      "connectionSpec": {
        "id": "e258278b-a4cf-43ac-b158-4fa0ca0d948b",
        "version": "1.0"
      }
    }
  }
}
```

+++

>[!TAB  新しいベース接続とターゲット接続 ]

+++[!DNL Azure Blob] の新しい [!DNL base connection] を表示する

```json {line-numbers="true" start-line="1" highlight="5"}
{
  ...
  "name": "Azure Blob Storage",
  "connectionSpec": {
    "id": "6d6b59bf-fb58-4107-9064-4d246c0e5bb2",
    "version": "1.0"
  },
  "state": "enabled",
  "auth": {
    "specName": "ConnectionString",
    "params": {
      "authorizedDate": "2022-06-02",      
      "connectionString": "<your-connection-string>"
    }
  },
  "encryption": {
    "specName": "File Encryption",
    "params": {
      "encryptionAlgo": "PGP/GPG",
      "publicKey": "<publicKey>"
    }
  },
  "version": "\"4008a892-0000-0200-0000-6389890d0000\"",
  "etag": "\"4008a892-0000-0200-0000-6389890d0000\""
}
```

+++

+++[!DNL Azure Blob] の新しい [!DNL target connection] を表示する

```json {line-numbers="true" start-line="1" highlight="13, 17-25"}
{
  ...
  "name": "v1",
  "description": "v2",
  "baseConnectionId": "1329d183-a3ee-4454-ab3f-e2388082bf29",
  "state": "enabled",
  "data": {
    "format": "CSV",
    "schema": null,
    "properties": null
  },
  "connectionSpec": {
    "id": "6d6b59bf-fb58-4107-9064-4d246c0e5bb2",
    "version": "1.0"
  },
  "params": {
    "csvOptions": {
      "nullValue": "null",
      "emptyValue": "",
      "escape": "\\",
      "quote": "",
      "delimiter": ","
    },
    "compression": "NONE",
    "fileType": "CSV",
    "mode": "Server-to-server",
    "container": "usdasda",
    "path": "v3"
  },
  "version": "\"5509fe3f-0000-0200-0000-638a28880000\"",
  "etag": "\"5509fe3f-0000-0200-0000-638a28880000\"",
  "inheritedAttributes": {
    "baseConnection": {
      "id": "1329d183-a3ee-4454-ab3f-e2388082bf29",
      "connectionSpec": {
        "id": "6d6b59bf-fb58-4107-9064-4d246c0e5bb2",
        "version": "1.0"
      }
    }
  }
}
```

+++

>[!ENDTABS]

### SFTP 宛先に対する後方互換性のない変更 {#changes-sftp-destination}

次の表に示すように、API ユーザーに対する後方互換性のない変更は更新された `connection spec ID` であり、`flow spec ID` です。

| SFTP | レガシー | 新規 |
|---------|----------|---------|
| フロー仕様 | 71471eba-b620-49e4-90fd-23f1fa0174d8 | fd36aaa4-bf2b-43fb-9387-43785eeeb799 |
| 接続仕様 | 64ef4b8b-a6e0-41b5-9677-3805d1ee5dd0 | 36965a81-b1c6-401b-99f8-22508f1e6a26 |

上記の更新されたフローと接続仕様に加えて、SFTP ベース接続を作成する際に必要なパラメーターが変更されました。

* 以前は、SFTP 宛先のベース接続には `host` パラメーターが必要でした。 このパラメーターの名前は、`domain` に変更されました。

以下のタブに、SFTP の従来の接続と新しいベース接続およびターゲット接続の完全な例と、変更される行がハイライト表示されています。 SFTP 宛先のターゲット接続を作成するために必要なパラメーターは変更されません。

>[!BEGINTABS]

>[!TAB  レガシーベース接続とターゲット接続 ]

+++SFTP 用の従来の [!DNL base connection] の表示 – パスワード認証

```json {line-numbers="true" start-line="1" highlight="5,15"}
{
  ...
  "name": "sftp",
  "connectionSpec": {
    "id": "64ef4b8b-a6e0-41b5-9677-3805d1ee5dd0",
    "version": "1.0"
  },
  "state": "enabled",
  "auth": {
    "specName": "Basic Authentication for sftp",
    "params": {
      "authorizedDate": "2022-06-02",
      "password": "<your-password>",
      "userName": "DPID12345",
      "host": "ftp-out.demdex.com"
    }
  },
  "encryption": {
    "specName": "File Encryption",
    "params": {
      "encryptionAlgo": "PGP/GPG",
      "publicKey": "<publicKey>"
    }
  },
  "version": "\"d000013c-0000-0200-0000-629903bd0000\"",
  "etag": "\"d000013c-0000-0200-0000-629903bd0000\""
}
```

+++

+++[!DNL SFTP - SSH key] 認証のためのレガシ [!DNL base connection] ールの表示

```json {line-numbers="true" start-line="1" highlight="5,15"}
{
  ...
  "name": "sftp",
  "connectionSpec": {
    "id": "64ef4b8b-a6e0-41b5-9677-3805d1ee5dd0",
    "version": "1.0"
  },
  "state": "enabled",
  "auth": {
    "specName": "Basic Authentication for sftp",
    "params": {
      "authorizedDate": "2022-06-02",
      "sshKey": "<your-ssh-key>",
      "userName": "DPID12345",
      "port": 22
      "domain": "ftp-out.demdex.com"
    }
  },
  "encryption": {
    "specName": "File Encryption",
    "params": {
      "encryptionAlgo": "PGP/GPG",
      "publicKey": "<publicKey>"
    }
  },
  "version": "\"d000013c-0000-0200-0000-629903bd0000\"",
  "etag": "\"d000013c-0000-0200-0000-629903bd0000\""
}
```

+++

+++SFTP 用の従来の [!DNL target connection] の表示

```json {line-numbers="true" start-line="1" highlight="13"}
{
  ...
  "name": "test sftp 6/2",
  "description": "",
  "baseConnectionId": "e6f3a300-0bf7-4755-b7f8-308dc2a99133",
  "state": "enabled",
  "data": {
    "format": "CSV",
    "schema": null,
    "properties": null
  },
  "connectionSpec": {
    "id": "64ef4b8b-a6e0-41b5-9677-3805d1ee5dd0",
    "version": "1.0"
  },
  "params": {
    "mode": "FTP",
    "remotePath": "test"
  },
  "version": "\"8503ab91-0000-0200-0000-629903ce0000\"",
  "etag": "\"8503ab91-0000-0200-0000-629903ce0000\"",
  "inheritedAttributes": {
    "baseConnection": {
      "id": "e6f3a300-0bf7-4755-b7f8-308dc2a99133",
      "connectionSpec": {
        "id": "64ef4b8b-a6e0-41b5-9677-3805d1ee5dd0",
        "version": "1.0"
      }
    }
  }
}
```

+++

>[!TAB  新しいベース接続とターゲット接続 ]

+++[!DNL SFTP - password authentication] の新しい [!DNL base connection] を表示する

```json {line-numbers="true" start-line="1" highlight="5"}
{
  ...
  "name": "SFTP",
  "connectionSpec": {
    "id": "36965a81-b1c6-401b-99f8-22508f1e6a26",
    "version": "1.0"
  },
  "state": "enabled",
  "auth": {
    "specName": "SFTP with Password",
    "params": {
      "authorizedDate": "2022-06-02",
      "domain": "ftp-out.demdex.com",
      "username": "DPID12345",
      "password": "<your-password>",
      "port": 22      
    }
  },
  "encryption": {
    "specName": "File Encryption",
    "params": {
      "encryptionAlgo": "PGP/GPG",
      "publicKey": "<publicKey>"
    }
  },
  "version": "\"420826cc-0000-0200-0000-638999a60000\"",
  "etag": "\"420826cc-0000-0200-0000-638999a60000\""
}
```

+++

+++[!DNL SFTP - SSH key] 認証の新しい [!DNL base connection] を表示する

```json {line-numbers="true" start-line="1" highlight="5,12"}
{
  ...
  "name": "SFTP",
  "connectionSpec": {
    "id": "36965a81-b1c6-401b-99f8-22508f1e6a26",
    "version": "1.0"
  },
  "state": "enabled",
  "auth": {
    "specName": "Basic Authentication for sftp",
    "params": {
      "authorizedDate": "2022-06-02",
      "domain": "ftp-out.demdex.com",
      "username": "DPID12345",
      "sshKey": "<your-ssh-key>",
    }
  },
  "encryption": {
    "specName": "File Encryption",
    "params": {
      "encryptionAlgo": "PGP/GPG",
      "publicKey": "<publicKey>"
    }
  },
  "version": "\"420826cc-0000-0200-0000-638999a60000\"",
  "etag": "\"420826cc-0000-0200-0000-638999a60000\""
}
```

+++

+++SFTP 用の新しい [!DNL target connection] の表示

```json {line-numbers="true" start-line="1" highlight="13, 17-25"}
{
  ...
  "name": "test sftp 6/2",
  "description": "",
  "baseConnectionId": "af63fbe1-45ff-4722-a9de-fbbe789dc7b0",
  "state": "enabled",
  "data": {
    "format": "CSV",
    "schema": null,
    "properties": null
  },
  "connectionSpec": {
    "id": "36965a81-b1c6-401b-99f8-22508f1e6a26",
    "version": "1.0"
  },
  "params": {
    "csvOptions": {
      "nullValue": "null",
      "emptyValue": "",
      "escape": "\\",
      "quote": "",
      "delimiter": ","
    },
    "compression": "NONE",
    "fileType": "CSV",
    "mode": "FTP",
    "remotePath": "test"
  },
  "version": "\"5509b5cf-0000-0200-0000-638a2ab60000\"",
  "etag": "\"5509b5cf-0000-0200-0000-638a2ab60000\"",
  "inheritedAttributes": {
    "baseConnection": {
      "id": "af63fbe1-45ff-4722-a9de-fbbe789dc7b0",
      "connectionSpec": {
        "id": "36965a81-b1c6-401b-99f8-22508f1e6a26",
        "version": "1.0"
      }
    }
  }
}
```

+++

>[!ENDTABS]

### [!DNL Amazon S3]、[!DNL Azure Blob]、SFTP の宛先に共通する、後方互換性のない変更 {#changes-all-destinations}

3 つのすべての宛先のプロファイルセレクターの手順は、必要に応じて、書き出したファイルの列ヘッダーの名前を変更できるマッピング手順に置き換えられます。 古い属性セレクターの手順を左側に、新しいマッピングの手順を右側に、それぞれ以下の画像を並べて参照してください。

![&#x200B; 移行ガイドの概要画像 &#x200B;](/help/destinations/assets/api/api-migration-guide/old-and-new-mapping-step.png)

従来の例の `profileSelectors` オブジェクトが新しい `profileMapping` オブジェクトに置き換えられることに注意してください。

`profileMapping` オブジェクトの設定に関する詳細については、[API チュートリアルを参照して、クラウドストレージの宛先にデータを書き出してください &#x200B;](/help/destinations/api/activate-segments-file-based-destinations.md#attribute-and-identity-mapping)。

>[!BEGINTABS]

>[!TAB  古い変換パラメーター ]

+++古い変換パラメーターの例を示します

```json{line-numbers="true" start-line="1" highlight="4-40, 45-53"}
{
  "segmentSelectors": { // shortened for brevity since nothing changes in the audience selectors
  },  
  "profileSelectors": {
    "selectors": [
      {
        "type": "JSON_PATH",
        "value": {
          "path": "CORE",
          "operator": "EXISTS",
          "mapping": {
            "sourceType": "text/x.schema-path",
            "source": "CORE",
            "destination": "CORE",
            "identity": false,
            "primaryIdentity": false,
            "functionVersion": 0,
            "sourceAttribute": "CORE",
            "destinationXdmPath": "CORE"
          },
          "identity": {
            "namespace": "CORE"
          }
        }
      },
      ...
      {
        "type": "JSON_PATH",
        "value": {
          "path": "segmentMembership.status",
          "operator": "EXISTS",
          "mapping": {
            "sourceType": "text/x.schema-path",
            "source": "segmentMembership.status",
            "destination": "segmentMembership.status",
            "identity": false,
            "primaryIdentity": false,
            "functionVersion": 0,
            "sourceAttribute": "segmentMembership.status",
            "destinationXdmPath": "segmentMembership.status"
          }
        }
      }
    ],
    "mandatoryFields": [
      "CORE",
      "person.name.lastName",
      "personalEmail.address"
    ],
    "primaryFields": [
      {
        "identityNamespace": "CORE",
        "fieldType": "IDENTITY"
      }
    ]
  }
}
```

+++

>[!TAB  新しい変換パラメーター ]

+++移行後の変換パラメーターの例を示します

以下の設定例では、`profileSelectors` のフィールドが `profileMapping` オブジェクトに置き換えられました。

```json {line-numbers="true" start-line="1" highlight="4-12, 18-20"}
{
  "segmentSelectors": { // shortened for brevity since nothing changes in the audience selectors
  },  
  "mandatoryFields": [
    "CORE",
    "person_name_lastName",
    "personalEmail_address"
  ],
  "primaryFields": [
    {
      "identityNamespace": "CORE",
      "fieldType": "IDENTITY"
    }
  ],
  "identityMapping": {
    "mappings": []
  },
  "profileMapping": {
    "mappingId": "40dfd952fe09498ba65145c7a5de3e07",
    "mappingVersion": 0
  },
  "attributeMapping": {}
}
```

+++

>[!ENDTABS]

## 移行のタイムラインとアクション項目 {#timeline-and-action-items}

[!DNL Amazon S3]、[!DNL Azure Blob]、SFTP 宛先の新しい宛先カードへの従来のデータフローの移行は、組織が移行の準備を整え次第、2023 年 7 月 26 日 **までに行われます**。

移行日が近づくと、Adobeからリマインダーメールが届きます。 準備として、移行に備えて、以下のアクション項目の節を参照してください。

### アクション アイテム {#action-items}

[!DNL Amazon S3]、[!DNL Azure Blob]、SFTP クラウドストレージの宛先を新しいカードに移行する準備として、以下に示すように、スクリプトと自動 API 呼び出しを更新する準備を行ってください。

1. 2023 年 7 月 26 日（PT）までに、既存の [!DNL Amazon S3]、[!DNL Azure Blob] または SFTP クラウドストレージの宛先のスクリプトまたは自動 API 呼び出しを更新します。 従来の接続仕様またはフロー仕様を利用する自動 API 呼び出しまたはスクリプトは、新しい接続仕様またはフロー仕様に更新する必要があります。
2. 7 月 26 日（PT）より前にスクリプトが更新されたら、Adobeアカウント担当者にお問い合わせください。
3. 例えば、`targetConnectionSpecId` をフラグとして使用すると、データフローが新しい宛先カードに移行されたかどうかを判断できます。 `if` の条件を使用してスクリプトを更新すると、`flow.inheritedAttributes.targetConnections[0].connectionSpec.id` の従来のターゲット接続仕様と更新されたターゲット接続仕様を調べて、データフローが移行されているかどうかを判断できます。 各宛先については、このページの特定の節で、従来の接続仕様 ID と新しい接続仕様 ID を確認できます。
4. Adobeアカウントチームから、データフローが移行されるタイミングについて詳しく連絡します。
5. 7 月 26 日（PT）以降、すべてのデータフローが移行されます。 既存のすべてのデータフローに、新しいフローエンティティ（接続仕様、フロー仕様、ベース接続、ターゲット接続）が追加されます。 レガシーフローエンティティを使用するスクリプトまたは API 呼び出しは、動作を停止します。

## 移行に関するその他の考慮事項 {#other-considerations}

移行中または移行後の書き出しの既存のスケジュールには影響ありません。

## 次の手順 {#next-steps}

このページを参照すると、クラウドストレージの宛先の移行に備えて何らかのアクションを実行する必要があるかどうかがわかります。 また、Experience Platform設定でファイルを好みのクラウドストレージの宛先に書き出す API ベースのワークフローを設定する際に、参照するドキュメントページもわかります。 次に、API チュートリアルを参照して、[&#x200B; クラウドストレージの宛先へのデータの書き出し &#x200B;](/help/destinations/api/activate-segments-file-based-destinations.md) を確認できます。
