---
keywords: Experience Platform;ホーム;人気のトピック
solution: Experience Platform
title: Data Landing Zone Source
description: データランディングゾーンをAdobe Experience Platformに接続する方法を学ぶ
exl-id: bdc10095-7de4-4183-bfad-a7b5c89197e3
source-git-commit: cb37eda87b8fcc0d0284db7a0bab8d48eab5aae6
workflow-type: tm+mt
source-wordcount: '844'
ht-degree: 35%

---

# [!DNL Data Landing Zone]

>[!IMPORTANT]
>
>このページは、Experience Platformの [!DNL Data Landing Zone] *ソース* コネクタに固有のページです。 [!DNL Data Landing Zone] *宛先* コネクタへの接続について詳しくは、[[!DNL Data Landing Zone]  宛先ドキュメントページ ](/help/destinations/catalog/cloud-storage/data-landing-zone.md) を参照してください。

[!DNL Data Landing Zone] は、Adobe Experience Platformによってプロビジョニングされた [!DNL Azure Blob] ストレージインターフェイスであり、ファイルを Platform に取り込むための安全なクラウドベースのファイルストレージ機能にアクセスできます。 サンドボックスごとに 1 つの [!DNL Data Landing Zone] コンテナに対するアクセス権があります。すべてのコンテナの合計データ量は、Platform 製品およびサービスライセンスで提供される合計データ量に制限されます。Platform とそのアプリケーション（[!DNL Customer Journey Analytics]、[!DNL Journey Orchestration]、[!DNL Intelligent Services]、[!DNL Adobe Real-Time Customer Data Platform] など）のすべての顧客は、サンドボックスごとに 1 つの [!DNL Data Landing Zone] コンテナを使用してプロビジョニングされます。 [!DNL Azure Storage Explorer] またはコマンドラインインターフェイスを通じて、コンテナに対してファイルの読み取りと書き込みを行うことができます。

[!DNL Data Landing Zone] は SAS ベースの認証をサポートし、そのデータは保存時および転送中は標準 [!DNL Azure Blob] ストレージセキュリティメカニズムで保護されます。SAS ベースの認証を使用すると、パブリックインターネット接続を介して [!DNL Data Landing Zone] コンテナに安全にアクセスできます。ユーザーが [!DNL Data Landing Zone] コンテナにアクセスする場合、ネットワークの変更は必要ありません。つまり、ネットワークの許可リストや地域間設定は必要ありません。 Platform では、[!DNL Data Landing Zone] コンテナにアップロードされるすべてのファイルに対して厳密に 7 日間の有効期限が適用されます。 すべてのファイルは 7 日後に削除されます。

## ファイルとディレクトリの命名制約

クラウドストレージファイルまたはディレクトリに名前を付ける際に考慮する必要がある制約のリストを次に示します。

- ディレクトリ名とファイルコンポーネント名は 255 文字を超えてはなりません。
- ディレクトリ名とファイル名の末尾にスラッシュ（`/`）は使用できません。使用した場合、自動的に削除されます。
- 次の予約 URL 文字は、適切にエスケープする必要があります。`! ' ( ) ; @ & = + $ , % # [ ]`
- 次の文字は使用できません。`" \ / : | < > * ?`
- 無効な URL パス文字は使用できません。`\uE000` のようなコードポイントは、NTFS ファイル名では有効ですが、有効な Unicode 文字ではありません。さらに、制御文字（`0x00` から `0x1F`、`\u0081` など）など、一部の ASCII 文字または Unicode 文字も許可されていません。 HTTP/1.1 で Unicode 文字列を規定するルールについては、[RFC 2616、セクション 2.2：基本ルール](https://www.ietf.org/rfc/rfc2616.txt)および [RFC 3987](https://www.ietf.org/rfc/rfc3987.txt) を参照してください。
- 次のファイル名は使用できません：LPT1、LPT2、LPT3、LPT4、LPT5、LPT6、LPT7、LPT8、LPT9、COM1、COM2、COM3、COM4、COM5、COM6、COM7、COM8、COM9、PRN、AUX、NUL、CON、CLOCK$、ドット文字（.）、2 つのドット文字（..）。

## データランディングゾーンのコンテンツを管理{#manage-the-contents-of-your-data-landing-zone}

[[!DNL Azure Storage Explorer]](https://azure.microsoft.com/ja-jp/features/storage-explorer/) を使用して [!DNL Data Landing Zone] コンテナのコンテンツを管理することができます。

[!DNL Azure Storage Explorer] UI で、左側のナビゲーションにある「接続」アイコンを選択します。 **リソースを選択**&#x200B;ウィンドウが開き、接続するオプションが表示されます。[!DNL Data Landing Zone] に接続する **[!DNL Blob container]** を選択してください。

![select-resource](../../images/tutorials/create/dlz/select-resource.png)

次に、接続方法として「**共有アクセス署名 URL (SAS)**」を選択し、「**次へ**」をクリックします。

![select-connection-method](../../images/tutorials/create/dlz/select-connection-method.png)

接続方法を選択した後、次に、[!DNL Data Landing Zone] コンテナに対応する **表示名** および **[!DNL Blob]コンテナ SAS URL** を指定する必要があります。

>[!TIP]
>
>[!DNL Data Landing Zone] の資格情報は、Platform UI のソースカタログから取得できます。

[!DNL Data Landing Zone] SAS URL を入力し、「次へ **を選択します**

![enter-connection-info](../../images/tutorials/create/dlz/enter-connection-info.png)

**概要**&#x200B;ウィンドウが開き、[!DNL Blob] エンドポイントと権限を含む設定の概要が表示されます。準備ができたら、「**接続**」を選択します。

![概要](../../images/tutorials/create/dlz/summary.png)

接続が成功すると、[!DNL Azure Storage Explorer] UI と [!DNL Data Landing Zone] コンテナが更新されます。

![dlz-user-container](../../images/tutorials/create/dlz/dlz-user-container.png)

[!DNL Data Landing Zone] コンテナが [!DNL Azure Storage Explorer] に接続され、[!DNL Data Landing Zone] コンテナへのファイルのアップロードを開始できるようになりました。 アップロードするには、「**アップロード**」を選択し、「**ファイルをアップロード**」を選択します。

![ アップロード ](../../images/tutorials/create/dlz/upload.png)

アップロードするファイルを選択したら、そのファイルをアップロードする [!DNL Blob] タイプと目的の宛先ディレクトリを特定する必要があります。 終了したら「**アップロード**」を選択します。

| [!DNL Blob] タイプ | 説明 |
| --- | --- |
| ブロック [!DNL Blob] | ブロック [!DNL Blobs] は、大量のデータを効率的にアップロードするために最適化されています。 ブロック [!DNL Blobs] は、[!DNL Data Landing Zone] の既定のオプションです。 |
| Append [!DNL Blob] | 追加 [!DNL Blobs] は、ファイルの最後にデータを追加する場合に最適化されています。 |

![upload-files](../../images/tutorials/create/dlz/upload-files.png)

## コマンドラインインターフェイスを使用した [!DNL Data Landing Zone] へのファイルのアップロード

また、デバイスのコマンドラインインターフェイスを使用して、[!DNL Data Landing Zone] ーバーへのアップロードファイルにアクセスすることもできます。

### Bash を使用したファイルのアップロード

次の例では、Bash と cURL を使用して、[!DNL Azure Blob Storage] REST API でファイルを [!DNL Data Landing Zone] にアップロードします。

```shell
# Set Azure Blob-related settings
DATE_NOW=$(date -Ru | sed 's/\+0000/GMT/')
AZ_VERSION="2018-03-28"
AZ_BLOB_URL="<URL TO BLOB ACCOUNT>"
AZ_BLOB_CONTAINER="<BLOB CONTAINER NAME>"
AZ_BLOB_TARGET="${AZ_BLOB_URL}/${AZ_BLOB_CONTAINER}"
AZ_SAS_TOKEN="<SAS TOKEN, STARTING WITH ? AND ENDING WITH %3D>"

# Path to the file we wish to upload
FILE_PATH="</PATH/TO/FILE>"
FILE_NAME=$(basename "$FILE_PATH")

# Execute HTTP PUT to upload file (remove '-v' flag to suppress verbose output)
curl -v -X PUT \
   -H "Content-Type: application/octet-stream" \
   -H "x-ms-date: ${DATE_NOW}" \
   -H "x-ms-version: ${AZ_VERSION}" \
   -H "x-ms-blob-type: BlockBlob" \
   --data-binary "@${FILE_PATH}" "${AZ_BLOB_TARGET}/${FILE_NAME}${AZ_SAS_TOKEN}"
```

### Python を使用したファイルのアップロード

次の例では [!DNL Microsoft's]Python v12 SDK を使用して、ファイルを [!DNL Data Landing Zone] にアップロードします。

>[!TIP]
>
>次の例では、完全な SAS URI を使用して [!DNL Azure Blob] コンテナに接続しますが、他の方法や操作を使用して認証することもできます。 詳しくは、この [[!DNL Microsoft] Python v12 SDK に関するドキュメント ](https://docs.microsoft.com/en-us/azure/storage/blobs/storage-quickstart-blobs-python) を参照してください。

```py
import os
from azure.storage.blob import ContainerClient

try:
    # Set Azure Blob-related settings
    sasUri = "<SAS URI>"
    srcFilePath = "<FULL PATH TO FILE>" 
    srcFileName = os.path.basename(srcFilePath)

    # Connect to container using SAS URI
    containerClient = ContainerClient.from_container_url(sasUri)

    # Upload file to Data Landing Zone with overwrite enabled
    with open(srcFilePath, "rb") as fileToUpload:
        containerClient.upload_blob(srcFileName, fileToUpload, overwrite=True)

except Exception as ex:
    print("Exception: " + ex.strerror)
```

### [!DNL AzCopy] を使用したファイルのアップロード

次の例では、[!DNL AzCopy] ユーティリティ [!DNL Microsoft's] 使用してファイルを [!DNL Data Landing Zone] にアップロードします。

>[!TIP]
>
>以下の例では `copy` コマンドを使用していますが、他のコマンドおよびオプションを使用して、[!DNL AzCopy] を使用して [!DNL Data Landing Zone] にファイルをアップロードできます。 詳しくは、この [[!DNL Microsoft AzCopy]  ドキュメント ](https://docs.microsoft.com/en-us/azure/storage/common/storage-ref-azcopy?toc=/azure/storage/blobs/toc.json) を参照してください。

```bat
set sasUri=<FULL SAS URI, PROPERLY ESCAPED>
set srcFilePath=<PATH TO LOCAL FILE(S); WORKS WITH WILDCARD PATTERNS>

azcopy copy "%srcFilePath%" "%sasUri%" --overwrite=true --recursive=true
```

## [!DNL Data Landing Zone] の [!DNL Platform] への接続

以下のドキュメントでは、API またはユーザーインターフェイスを使用して、[!DNL Data Landing Zone] コンテナからAdobe Experience Platformにデータを取り込む方法に関する情報を提供します。

### API の使用

- [Flow Service API [!DNL Data Landing Zone]  使用したソース接続の作成](../../tutorials/api/create/cloud-storage/data-landing-zone.md)
- [Flow Service API を使用して、クラウドストレージソースのデータフローを作成](../../tutorials/api/collect/cloud-storage.md)

### UI の使用

- [UI [!DNL Data Landing Zone]  使用したへの Platform の接続](../../tutorials/ui/create/cloud-storage/data-landing-zone.md)
- [UI でクラウドストレージ接続のデータフローを作成](../../tutorials/ui/dataflow/batch/cloud-storage.md)

>[!IMPORTANT]
>
>[!DNL Data Landing Zone] を使用してExperience Platformに接続する場合、プライベートリンクは現在サポートされていません。 アクセスでサポートされているメソッドは、[ こちら ](#manage-the-contents-of-your-data-landing-zone) に示すメソッドのみです。

