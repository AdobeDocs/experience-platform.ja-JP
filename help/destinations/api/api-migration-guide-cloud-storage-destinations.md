---
solution: Experience Platform
title: クラウドストレージ宛先の API 移行ガイド
description: 追加機能を持つ新しいクラウドストレージの宛先カードへの移行の一環として、クラウドストレージの宛先をアクティブ化するワークフローの変更点について説明します。
type: Tutorial
exl-id: 4acaf718-794e-43a3-b8f0-9b19177a2bc0
source-git-commit: 07a91ef15075b6c438e85aecff12dfab704cc6a2
workflow-type: tm+mt
source-wordcount: '1418'
ht-degree: 4%

---

# クラウドストレージ宛先の API 移行ガイド

>[!IMPORTANT]
>
>* このページで説明する機能は、Real-Time CDP Prime および Ultimate パッケージを購入したお客様が利用できます。 詳しくは、アドビ担当者にお問い合わせください。


## 移行コンテキスト {#migration-context}

開始中 [2022 年 10 月](/help/release-notes/2022/october-2022.md#new-or-updated-destinations)を使用すると、新しいファイルエクスポート機能を使用して、Experience Platformからファイルをエクスポートする際に、拡張カスタマイズ機能にアクセスできます。

* 追加の[ファイル命名オプション](/help/destinations/ui/activate-batch-profile-destinations.md#file-names)。
* 書き出されたファイルに、 [新しいマッピング手順](/help/destinations/ui/activate-batch-profile-destinations.md#mapping).
* 次の項目を選択できます。 [ファイルタイプ](/help/destinations/ui/connect-destination.md#file-formatting-and-compression-options) 」と入力します。
* 次の機能を持つ [書き出された CSV データファイルの形式をカスタマイズする](/help/destinations/ui/batch-destinations-file-formatting-options.md).

この機能は、以下に示すベータ版クラウドストレージカードでサポートされています。

* [[!DNL (Beta) Amazon S3]](../../destinations/catalog/cloud-storage/amazon-s3.md#changelog)
* [[!DNL (Beta) Azure Blob]](../../destinations/catalog/cloud-storage/azure-blob.md#changelog)
* [[!DNL (Beta) SFTP]](../../destinations/catalog/cloud-storage/sftp.md#changelog)

<!--

Commenting out the three net new cloud storage destinations

* [[!DNL (Beta) Azure Data Lake Storage Gen2]](../../destinations/catalog/cloud-storage/adls-gen2.md)
* [[!DNL (Beta) Data Landing Zone]](../../destinations/catalog/cloud-storage/data-landing-zone.md)
* [[!DNL (Beta) Google Cloud Storage]](../../destinations/catalog/cloud-storage/google-cloud-storage.md)

-->

現在、Experience PlatformUI では、3 つの宛先のうち、2 枚の宛先カードを並べて表示できます。 以下に、 [!DNL Amazon S3] 従来の宛先と新しい宛先。 どの場合でも、 **ベータ版** は新しい宛先カードです。

![2 つの Amazon S3 の宛先カードを並べて表示した画像。](../assets/catalog/cloud-storage/amazon-s3/two-amazons3-destination-cards.png)

これらの拡張機能を持つ宛先は最初はベータ版として提供されていましたが、 *Adobeは、すべてのReal-Time CDPのお客様を新しいクラウドストレージの宛先に移行します*. 既に [!DNL Amazon S3], [!DNL Azure Blob]または SFTP の場合、既存のデータフローは新しいカードに移行されます。 移行の一環としての特定の変更の詳細については、以降の説明を参照してください。

## このページの適用先 {#who-this-applies-to}

既に [フローサービス API](https://developer.adobe.com/experience-platform-apis/references/destinations/) Amazon S3、Azure Blob、または SFTP クラウドストレージの宛先にプロファイルを書き出すには、この API 移行ガイドを適用します。

内でスクリプトを実行している場合、 [!DNL Amazon S3], [!DNL Azure Blob]または、Experience Platformから書き出されたファイルの上にある SFTP クラウドストレージの場所は、新しいカードの接続とフロー仕様、およびマッピング手順に関して、一部のパラメーターが変更されることに注意してください。

例えば、スクリプトを使用して宛先のデータフローを [!DNL Amazon S3] 宛先 ( [!DNL Amazon S3] の宛先に追加する場合は、接続仕様が変更されるので、フィルターを更新する必要があります。

## 関連するドキュメントのリンク {#relevant-documentation-links}

この節では、クラウドストレージの宛先にデータを書き出すための拡張機能に関する、関連する API チュートリアルとリファレンスドキュメントを示します。

<!--

TBD if we keep this link but will likely remove it

[Legacy API tutorial to export data to cloud storage destinations](/help/destinations/api/connect-activate-batch-destinations.md) (outdated, do not use anymore)

-->
* [クラウドストレージの宛先にセグメントを書き出すための API チュートリアル](/help/destinations/api/activate-segments-file-based-destinations.md)
* [宛先フローサービス API リファレンスドキュメント](https://developer.adobe.com/experience-platform-apis/references/destinations/)

## 下位互換性のない変更の概要 {#summary-backwards-incompatible-changes}

新しい宛先への移行に伴い、既存のすべてのデータフローがに移行される [!DNL Amazon S3], [!DNL Azure Blob]、および SFTP の宛先には、新しいターゲット接続とベース接続が割り当てられます。 プロファイルマッピングの手順も変更されます。 下位互換性のない変更は、各宛先に関して以下の節で要約されています。 また、 [宛先の用語集](https://developer.adobe.com/experience-platform-apis/references/destinations/#tag/Glossary) を参照してください。

![移行ガイドの概要の画像](/help/destinations/assets/api/api-migration-guide/migration-guide-diagram.png)

### 後方互換性のない [!DNL Amazon S3] 宛先 {#changes-amazon-s3-destination}

API ユーザーに対する後方互換性のない変更が更新されました。 `connection spec ID` および `flow spec ID` 次の表に示すように：

| [!DNL Amazon S3] | レガシー | 新規 |
|---------|----------|---------|
| フロー仕様 | 71471eba-b620-49e4-90fd-23f1fa0174d8 | 1a0514a6-33d4-4c7f-aff8-594799c47549 |
| 接続仕様 | 4890fc95-5a1f-4983-94bb-e060c08e3f81 | 4fce964d-3f37-408f-9778-e597338a21ee |

の完全なレガシーおよび新しいベース接続とターゲット接続の例を表示します。 [!DNL Amazon S3] 」をクリックします。 の基本接続を作成するために必要なパラメータ [!DNL Amazon S3] の宛先は変更されません。

同様に、ターゲット接続の作成に必要なパラメーターには、下位互換性のない変更はありません。

>[!BEGINTABS]

>[!TAB 従来のベース接続とターゲット接続]

+++レガシーの表示 [!DNL base connection] 対象 [!DNL Amazon S3]

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

+++レガシーの表示 [!DNL target connection] 対象 [!DNL Amazon S3]

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

>[!TAB 新しいベース接続とターゲット接続]

+++新規を表示 [!DNL base connection] 対象 [!DNL Amazon S3]

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

+++新規を表示 [!DNL target connection] 対象 [!DNL Amazon S3]

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

### 下位互換性のない変更： [!DNL Azure Blob] 宛先 {#changes-azure-blob-destination}

API ユーザーに対する後方互換性のない変更が更新されました。 `connection spec ID` および `flow spec ID` 次の表に示すように：

| [!DNL Azure Blob] | レガシー | 新規 |
|---------|----------|---------|
| フロー仕様 | 71471eba-b620-49e4-90fd-23f1fa0174d8 | 752d422f-b16f-4f0d-b1c6-26e448e3b388 |
| 接続仕様 | e258278b-a4cf-43ac-b158-4fa0ca0d948b | 6d6b59bf-fb58-4107-9064-4d246c0e5bb2 |

の完全なレガシーおよび新しいベース接続とターゲット接続の例を表示します。 [!DNL Azure Blob] 」をクリックします。 Azure Blob 宛先のベース接続を作成するために必要なパラメーターは変更されません。

同様に、ターゲット接続の作成に必要なパラメーターには、下位互換性のない変更はありません。

>[!BEGINTABS]

>[!TAB 従来のベース接続とターゲット接続]

+++レガシーの表示 [!DNL base connection] 対象 [!DNL Azure Blob]

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

+++レガシーの表示 [!DNL target connection] 対象 [!DNL Azure Blob]

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

>[!TAB 新しいベース接続とターゲット接続]

+++新規を表示 [!DNL base connection] 対象 [!DNL Azure Blob]

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

+++新規を表示 [!DNL target connection] 対象 [!DNL Azure Blob]

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

### SFTP の宛先に対する後方互換性のない変更 {#changes-sftp-destination}

API ユーザーに対する後方互換性のない変更が更新されました。 `connection spec ID` および `flow spec ID` 次の表に示すように：

| SFTP | レガシー | 新規 |
|---------|----------|---------|
| フロー仕様 | 71471eba-b620-49e4-90fd-23f1fa0174d8 | fd36aaa4-bf2b-43fb-9387-43785eeeb799 |
| 接続仕様 | 64ef4b8b-a6e0-41b5-9677-3805d1ee5dd0 | 36965a81-b1c6-401b-99f8-22508f1e6a26 |

上記の更新されたフローおよび接続仕様に加えて、SFTP ベース接続を作成する際に必要なパラメーターに変更が加えられました。

* 以前は、SFTP の宛先のベース接続には、 `host` パラメーター。 このパラメーターの名前は「 `domain`.

下のタブで、SFTP の完全なレガシーおよび新しいベース接続とターゲット接続の例を確認してください。変更される行が強調表示されます。 SFTP の宛先のターゲット接続を作成するために必要なパラメーターは変更されません。

>[!BEGINTABS]

>[!TAB 従来のベース接続とターゲット接続]

+++レガシーの表示 [!DNL base connection] （SFTP の場合） — パスワード認証

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

+++レガシーの表示 [!DNL base connection] 対象 [!DNL SFTP - SSH key] 認証

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

+++レガシーの表示 [!DNL target connection] （SFTP 用）

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

>[!TAB 新しいベース接続とターゲット接続]

+++新規を表示 [!DNL base connection] 対象 [!DNL SFTP - password authentication]

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

+++新規を表示 [!DNL base connection] 対象 [!DNL SFTP - SSH key] 認証

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

+++新規を表示 [!DNL target connection] （SFTP 用）

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

### 一般的な下位互換性のない変更 [!DNL Amazon S3], [!DNL Azure Blob]、および SFTP の宛先 {#changes-all-destinations}

3 つの宛先すべてのプロファイルセレクターの手順は、マッピング手順に置き換えられます。必要に応じて、書き出したファイルの列ヘッダーの名前を変更できます。 下の画像を並べて表示します。左側に古い属性セレクターの手順、右側に新しいマッピングの手順が表示されます。

![移行ガイドの概要の画像](/help/destinations/assets/api/api-migration-guide/old-and-new-mapping-step.png)

この `profileSelectors` オブジェクトは、 `profileMapping` オブジェクト。

の設定に関する完全な情報を見つけます。 `profileMapping` オブジェクトを [クラウドストレージの宛先にデータを書き出すための API チュートリアル](/help/destinations/api/activate-segments-file-based-destinations.md#attribute-and-identity-mapping).

>[!BEGINTABS]

>[!TAB 古い変換パラメーター]

+++古い変換パラメーターの例を表示する

```json{line-numbers="true" start-line="1" highlight="4-40, 45-53"}
{
  "segmentSelectors": { // shortened for brevity since nothing changes in the segment selectors
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

>[!TAB 新しい変換パラメーター]

+++移行後の変換パラメーターの例の表示

次の設定例では、 `profileSelectors` フィールドは `profileMapping` オブジェクト。

```json {line-numbers="true" start-line="1" highlight="4-12, 18-20"}
{
  "segmentSelectors": { // shortened for brevity since nothing changes in the segment selectors
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

## 移行タイムラインとアクション項目 {#timeline-and-action-items}

の新しい宛先カードへの従来のデータフローの移行 [!DNL Amazon S3], [!DNL Azure Blob]、および SFTP の宛先は、組織が移行する準備が整い次第、おこなわれます。 **2023 年 6 月 31 日**.

移行日が近づくと、Adobeからリマインダー E メールが届きます。 準備中に、以下の「アクション項目」の節を読んで、移行の準備をしてください。

### アクション項目 {#action-items}

の移行の準備中 [!DNL Amazon S3], [!DNL Azure Blob]、および SFTP クラウドストレージの宛先を新しいカードに追加する場合は、以下で推奨されるように、スクリプトおよび自動 API 呼び出しを更新する準備をしてください。

1. 既存のスクリプトまたは自動 API 呼び出しを更新する [!DNL Amazon S3], [!DNL Azure Blob]、または 2023 年 6 月 30 日までの SFTP クラウドストレージの宛先。 従来の接続仕様またはフロー仕様を利用する自動 API 呼び出しまたはスクリプトは、新しい接続仕様またはフロー仕様に更新する必要があります。
2. スクリプトが 6 月 30 日より前にAdobeされたら、担当のアカウント担当者にお問い合わせください。
3. 例えば、 `targetConnectionSpecId` をフラグとして使用して、データフローが新しい宛先カードに移行されたかどうかを判断できます。 スクリプトを `if` の従来の接続仕様と更新されたターゲット接続仕様を調べるための条件 `flow.inheritedAttributes.targetConnections[0].connectionSpec.id` およびは、データフローが移行されたかどうかを判断します。 各宛先について、このページの特定のセクションで、従来の接続仕様 ID と新しい接続仕様 ID を確認できます。
4. Adobeアカウントチームは、データフローを移行する際の詳細情報を提供します。
5. 6 月 30 日以降、すべてのデータフローが移行されます。 既存のすべてのデータフローに、新しいフローエンティティ（接続仕様、フロー仕様、ベース接続、ターゲット接続）が追加されます。 レガシーフローエンティティを使用する側のスクリプトまたは API 呼び出しは、動作を停止します。

## その他の移行に関する考慮事項 {#other-considerations}

移行中または移行後の既存のエクスポートスケジュールには影響しません。

## 次の手順 {#next-steps}

このページを読むと、クラウドストレージの宛先の移行に備えて何らかのアクションを実行する必要があるかを把握できます。 API ベースのワークフローを設定して、Experience Platformから目的のクラウドストレージの宛先にファイルを書き出す際に参照するドキュメントページも把握できます。 次に、 API チュートリアルで [クラウドストレージの宛先へのデータの書き出し](/help/destinations/api/activate-segments-file-based-destinations.md).
