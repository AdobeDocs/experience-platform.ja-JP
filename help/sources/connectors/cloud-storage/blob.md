---
title: Azure Blob Source コネクタの概要
description: Azure Blob アカウントをExperience Platformに接続する方法を説明します
exl-id: 62adc74f-3570-42c7-9ae6-3ddbc09eccc7
source-git-commit: 8e932a25026bef2b785cfddfb8b668b1dd47eb0d
workflow-type: tm+mt
source-wordcount: '1021'
ht-degree: 21%

---

# [!DNL Azure Blob Storage] ソース

[!DNL Azure Blob Storage] は、[!DNL Microsoft Azure] が提供するクラウドベースのオブジェクトストレージサービスです。 テキスト、画像、ビデオ、バックアップ、ログなど、構造化されていないデータを大量に保存するように設計されています。 [!DNL Azure Blob Storage] を使用して、ドキュメント、画像、ビデオ、オーディオファイルなど、構造化されていない大量のデータを保存および管理できます。 データのバックアップとアーカイブ、ディザスタリカバリのサポート、および分析のためのビッグデータワークロードの処理に最適です。

[!DNL Azure Blob Storage] ソースを使用してアカウントを接続し、[!DNL Azure Blob Storage] からAdobe Experience Platformにデータを取り込みます。

## 前提条件 {#prerequisites}

[!DNL Azure Blob Storage] アカウントをExperience Platformに接続する前に、以下の節を参照して前提条件の設定を完了してください。

### IP アドレスの許可リスト

ソースをExperience Platformに接続する前に、地域固有の IP アドレスを許可リストに追加する必要があります。 詳しくは、[Experience Platformへの接続に対する IP アドレスの許可リストに加える](../../ip-address-allow-list.md) に関するガイドを参照してください。

>[!IMPORTANT]
>
>[!DNL Azure Blob] ソースは、Experience Platformへの同じリージョンの接続をサポートしていません。 [!DNL Azure] インスタンスがExperience Platformと同じネットワーク地域を使用している場合、Experience Platform ソースへの接続を確立できません。 現在、クロス地域接続のみがサポートされています。

### ファイルとディレクトリの命名制約

クラウドストレージファイルまたはディレクトリに名前を付ける際に考慮する必要がある制約のリストを次に示します。

- ディレクトリ名とファイルコンポーネント名は 255 文字を超えてはなりません。
- ディレクトリ名とファイル名の末尾にスラッシュ（`/`）は使用できません。使用した場合、自動的に削除されます。
- 次の予約 URL 文字は、適切にエスケープする必要があります。`! ' ( ) ; @ & = + $ , % # [ ]`
- 次の文字は使用できません。`" \ / : | < > * ?`
- 無効な URL パス文字は使用できません。`\uE000` のようなコードポイントは、NTFS ファイル名では有効ですが、有効な Unicode 文字ではありません。また、一部の ASCII 文字や Unicode 文字、例えば制御文字（0x00 ～ 0x1F、\u0081 など）も使用できません。HTTP/1.1 で Unicode 文字列を規定するルールについては、[RFC 2616、セクション 2.2：基本ルール](https://www.ietf.org/rfc/rfc2616.txt)および [RFC 3987](https://www.ietf.org/rfc/rfc3987.txt) を参照してください。
- 次のファイル名は使用できません：LPT1、LPT2、LPT3、LPT4、LPT5、LPT6、LPT7、LPT8、LPT9、COM1、COM2、COM3、COM4、COM5、COM6、COM7、COM8、COM9、PRN、AUX、NUL、CON、CLOCK$、ドット文字（.）、2 つのドット文字（..）。

### Experience Platformへの [!DNL Azure Blob Storage] の認証 {#authentication}

次の認証タイプを使用して、[!DNL Azure Blob Storage] アカウントをExperience Platformに接続できます。

- **アカウントキー認証**: ストレージアカウントのアクセスキーを使用して認証し、[!DNL Azure Blob Storage] アカウントに接続します。
- **共有アクセス署名（SAS）**:SAS URI を使用して、[!DNL Azure Blob Storage] アカウントのリソースへのデリゲートされた時間制限アクセスを提供します。
- **サービスプリンシパルベースの認証**:Azure Active Directory （AAD）サービスプリンシパル（クライアント ID とシークレット）を使用して、Azure Blob Storage アカウントに対して安全に認証します。

>[!BEGINTABS]

>[!TAB  アカウントキー認証 ]

アカウントキー認証を使用して [!DNL Azure Blob Storage] アカウントをExperience Platformに接続するには、次の資格情報の値を指定します。

| 資格情報 | 説明 |
| --- | --- |
| `connectionString` | ストレージアカウントの [!DNL Azure Blob Storage] の接続文字列。 この文字列には、[!DNL Azure Blob Storage] インスタンスの認証および接続に必要な情報が含まれています。 形式の例：`DefaultEndpointsProtocol=https;AccountName={ACCOUNT_NAME};AccountKey={ACCOUNT_KEY};EndpointSuffix=core.windows.net` |
| `container` | データファイルが格納されている [!DNL Azure Blob Storage] コンテナの名前。 コンテナは、ファイルシステム内のディレクトリと同様に、一連の BLOB を整理します。 |
| `folderPath` | ファイルが配置されている、指定されたコンテナ内のパス。 これは、コンテナ内のオプションのサブディレクトリパス（仮想フォルダー）です。 空白の場合は、コンテナのルートが使用されます。 |
| `connectionSpec.id` | 接続仕様 ID は、ベース接続とソース接続の作成に関連する認証仕様などの、ソースのコネクタプロパティを返します。 [!DNL Azure Blob Storage] の接続仕様 ID は `4c10e202-c428-4796-9208-5f1f5732b1cf` です。 **注意**：この資格情報は、[!DNL Flow Service] API を使用して接続する場合にのみ必要です。 |

[!DNL Azure Blob Storage] でのアカウントキー認証の使用方法について詳しくは、公式の [Microsoft Azure 認証ガイド &#x200B;](https://learn.microsoft.com/en-us/azure/data-factory/connector-azure-blob-storage?tabs=data-factory#account-key-authentication) を参照してください。

>[!TAB  共有アクセス署名 ]

共有アクセス署名を使用して [!DNL Azure Blob Storage] アカウントをExperience Platformに接続するには、次の資格情報の値を指定します。

| 資格情報 | 説明 |
| --- | --- |
| `SasURI` | アカウントを接続するための代替認証タイプとして使用できる共有アクセス署名 URI。 SAS URI パターンは `https://{ACCOUNT_NAME}.blob.core.windows.net/?sv={STORAGE_VERSION}&st={START_TIME}&se={EXPIRE_TIME}&sr={RESOURCE}&sp={PERMISSIONS}>&sip=<{IP_RANGE}>&spr={PROTOCOL}&sig={SIGNATURE}` です。 詳しくは、この [!DNL Azure] ドキュメント [&#x200B; 共有アクセス署名 URI](https://docs.microsoft.com/en-us/azure/data-factory/connector-azure-blob-storage#shared-access-signature-authentication) を参照してください。 |
| `container` | データファイルが格納されている [!DNL Azure Blob Storage] コンテナの名前。 コンテナは、ファイルシステム内のディレクトリと同様に、一連の BLOB を整理します。 |
| `folderPath` | ファイルが配置されている、指定されたコンテナ内のパス。 これは、コンテナ内のオプションのサブディレクトリパス（仮想フォルダー）です。 空白の場合は、コンテナのルートが使用されます。 |
| `connectionSpec.id` | 接続仕様 ID は、ベース接続とソース接続の作成に関連する認証仕様などの、ソースのコネクタプロパティを返します。 [!DNL Azure Blob Storage] の接続仕様 ID は `4c10e202-c428-4796-9208-5f1f5732b1cf` です。 **注意**：この資格情報は、[!DNL Flow Service] API を使用して接続する場合にのみ必要です。 |

[!DNL Azure Blob Storage] で共有アクセス署名を使用する方法について詳しくは、公式の [Microsoft Azure 認証ガイド &#x200B;](https://docs.microsoft.com/en-us/azure/data-factory/connector-azure-blob-storage#shared-access-signature-authentication) を参照してください。

>[!TAB  サービスプリンシパルベースの認証 ]

サービスプリンシパルベースの認証を使用して [!DNL Azure Blob Storage] アカウントをExperience Platformに接続するには、次の資格情報の値を指定します。

| 資格情報 | 説明 |
| --- | --- |
| `serviceEndpoint` | [!DNL Azure Blob Storage] アカウントのエンドポイント URL。 通常は、`https://{ACCOUNT_NAME}.blob.core.windows.net` の形式です。 |
| `accountKind` | [!DNL Azure Blob Storage] アカウントのタイプ。 共通の値には、`Storage` （汎用 V1）、`StorageV2` （汎用 V2）、`BlobStorage`、`BlockBlobStorage` などがあります。 |
| `servicePrincipalId` | 認証に使用する Azure Active Directory （AAD） サービス プリンシパルのクライアント/アプリケーション ID。 |
| `servicePrincipalKey` | Azure サービスプリンシパルに関連付けられたクライアントシークレットまたはパスワード。 |
| `tenant` | サービスプリンシパルが登録されている Azure Active Directory （AAD）テナント ID。 |
| `container` | データファイルが保存されている Azure Blob Storage コンテナの名前。 |
| `folderPath` | ファイルが配置されている、指定されたコンテナ内のパス。 これは、コンテナ内のオプションのサブディレクトリパス（仮想フォルダー）です。 空白の場合は、コンテナのルートが使用されます。 |
| `connectionSpec.id` | 接続仕様 ID は、ベース接続とソース接続の作成に関連する認証仕様などの、ソースのコネクタプロパティを返します。 Azure Blob Storage の接続仕様 ID は `4c10e202-c428-4796-9208-5f1f5732b1cf` です。 **注意**：この資格情報は、[!DNL Flow Service] API を使用して接続する場合にのみ必要です。 |

[!DNL Azure Blob Storage] でのサービスプリンシパルベースの認証の使用方法について詳しくは、公式の [Microsoft Azure 認証ガイド &#x200B;](https://learn.microsoft.com/en-us/azure/data-factory/connector-azure-blob-storage?tabs=data-factory#service-principal-authentication) を参照してください。

>[!ENDTABS]

## [!DNL Azure Blob Storage] の [!DNL Experience Platform] への接続

以下のドキュメントでは、API またはユーザーインターフェイスを使用して Azure Blob をAdobe Experience Platformに接続する方法に関する情報を提供します。

### API の使用

- [Experience Platform [!DNL Azure Blob Storage]  の接続](../../tutorials/api/create/cloud-storage/blob.md)
- [Flow Service API を使用して、クラウドストレージソースのデータ構造とコンテンツを探索](../../tutorials/api/explore/cloud-storage.md)
- [Flow Service API を使用して、クラウドストレージソースのデータフローを作成](../../tutorials/api/collect/cloud-storage.md)

### UI の使用

- [Experience Platform [!DNL Azure Blob Storage]  接続](../../tutorials/ui/create/cloud-storage/blob.md)
- [UI でクラウドストレージ接続のデータフローを作成](../../tutorials/ui/dataflow/batch/cloud-storage.md)
