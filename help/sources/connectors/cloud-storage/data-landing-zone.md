---
keywords: Experience Platform;ホーム;人気のトピック
solution: Experience Platform
title: データランディングゾーンのソース
description: データランディングゾーンをAdobe Experience Platformに接続する方法を説明します
exl-id: bdc10095-7de4-4183-bfad-a7b5c89197e3
source-git-commit: d57060ddeed64d3863f71ac1ea34ccc5c97265ea
workflow-type: tm+mt
source-wordcount: '842'
ht-degree: 43%

---

# [!DNL Data Landing Zone]

>[!IMPORTANT]
>
>このページは、 [!DNL Data Landing Zone] *ソース* コネクタのExperience Platform に接続する方法については、 [!DNL Data Landing Zone] *宛先* コネクタ ( [[!DNL Data Landing Zone] 宛先のドキュメントページ](/help/destinations/catalog/cloud-storage/data-landing-zone.md).

[!DNL Data Landing Zone] は [!DNL Azure Blob] Adobe Experience Platformによってプロビジョニングされたストレージインターフェイス。ファイルを Platform に取り込むための、セキュリティで保護されたクラウドベースのファイルストレージ機能にアクセスできます。 サンドボックスごとに 1 つの [!DNL Data Landing Zone] コンテナに対するアクセス権があります。すべてのコンテナの合計データ量は、Platform 製品およびサービスライセンスで提供される合計データ量に制限されます。Platform とそのアプリケーションサービスのすべての顧客（[!DNL Customer Journey Analytics]、[!DNL Journey Orchestration]、[!DNL Intelligent Services]、および [!DNL Adobe Real-Time Customer Data Platform]）は、サンドボックスごとに 1 つの [!DNL Data Landing Zone] のコンテナを使用してプロビジョニングされます。を通じて、コンテナに対してファイルの読み取りと書き込みをおこなうことができます。 [!DNL Azure Storage Explorer] またはコマンドラインインターフェイスを使用します。

[!DNL Data Landing Zone] は SAS ベースの認証をサポートし、そのデータは保存時および転送中は標準 [!DNL Azure Blob] ストレージセキュリティメカニズムで保護されます。SAS ベースの認証を使用すると、パブリックインターネット接続を介して [!DNL Data Landing Zone] コンテナに安全にアクセスできます。ユーザーが [!DNL Data Landing Zone] コンテナにアクセスする場合、ネットワークの変更は必要ありません。つまり、ネットワークに対して許可リストの設定や地域間設定は必要ありません。Platform では、 [!DNL Data Landing Zone] コンテナ。 すべてのファイルは 7 日後に削除されます。

## ファイルとディレクトリの命名制約

以下は、クラウドストレージファイルまたはディレクトリに名前を付ける際に考慮する必要がある制約のリストです。

- ディレクトリ名とファイルコンポーネント名は 255 文字を超えてはなりません。
- ディレクトリ名とファイル名の末尾にスラッシュ（`/`）は使用できません。使用した場合、自動的に削除されます。
- 次の予約 URL 文字は、適切にエスケープする必要があります。`! ' ( ) ; @ & = + $ , % # [ ]`
- 次の文字は使用できません。`" \ / : | < > * ?`
- 無効な URL パス文字は使用できません。`\uE000` のようなコードポイントは、NTFS ファイル名では有効ですが、有効な Unicode 文字ではありません。また、制御文字 ( `0x00` から `0x1F`, `\u0081`など ) は許可されていません。 HTTP/1.1 で Unicode 文字列を規定するルールについては、[RFC 2616、セクション 2.2：基本ルール](https://www.ietf.org/rfc/rfc2616.txt)および [RFC 3987](https://www.ietf.org/rfc/rfc3987.txt) を参照してください。
- 次のファイル名は使用できません：LPT1、LPT2、LPT3、LPT4、LPT5、LPT6、LPT7、LPT8、LPT9、COM1、COM2、COM3、COM4、COM5、COM6、COM7、COM8、COM9、PRN、AUX、NUL、CON、CLOCK$、ドット文字（.）、2 つのドット文字（..）。

## [!DNL Data Landing Zone] のコンテンツを管理

[[!DNL Azure Storage Explorer]](https://azure.microsoft.com/ja-jp/features/storage-explorer/) を使用して [!DNL Data Landing Zone] コンテナのコンテンツを管理することができます。

内 [!DNL Azure Storage Explorer] UI で、左側のナビゲーションで接続アイコンを選択します。 **リソースを選択**&#x200B;ウィンドウが開き、接続するオプションが表示されます。選択 **[!DNL Blob container]** 接続する [!DNL Data Landing Zone].

![select-resource](../../images/tutorials/create/dlz/select-resource.png)

次に、接続方法として「**共有アクセス署名 URL (SAS)**」を選択し、「**次へ**」をクリックします。

![select-connection-method](../../images/tutorials/create/dlz/select-connection-method.png)

接続方法を選択した後、次に **表示名** そして **[!DNL Blob]コンテナ SAS URL** は、 [!DNL Data Landing Zone] コンテナ。

>[!TIP]
>
>次を取得： [!DNL Data Landing Zone] 資格情報を Platform UI のソースカタログから取得します。

次を指定： [!DNL Data Landing Zone] SAS URL を選択し、 **次へ**

![enter-connection-info](../../images/tutorials/create/dlz/enter-connection-info.png)

**概要**&#x200B;ウィンドウが開き、[!DNL Blob] エンドポイントと権限を含む設定の概要が表示されます。準備ができたら、「**接続**」を選択します。

![概要](../../images/tutorials/create/dlz/summary.png)

接続が成功すると、[!DNL Azure Storage Explorer] UI と [!DNL Data Landing Zone] コンテナが更新されます。

![dlz-user-container](../../images/tutorials/create/dlz/dlz-user-container.png)

を [!DNL Data Landing Zone] ～に接続された容器 [!DNL Azure Storage Explorer]次に、 [!DNL Data Landing Zone] コンテナ。 アップロードする場合は、「 **アップロード** 次に、 **ファイルをアップロード**.

![アップロード](../../images/tutorials/create/dlz/upload.png)

アップロードするファイルを選択したら、 [!DNL Blob] としてアップロードするを入力し、目的のアップロード先ディレクトリを指定します。 終了したら、「 」を選択します。 **アップロード**.

| [!DNL Blob] タイプ | 説明 |
| --- | --- |
| ブロック [!DNL Blob] | ブロック [!DNL Blobs] は、大量のデータを効率的にアップロードするために最適化されています。 ブロック [!DNL Blobs] は、 [!DNL Data Landing Zone]. |
| 追加 [!DNL Blob] | 追加 [!DNL Blobs] は、ファイルの末尾にデータを追加するように最適化されています。 |

![upload-files](../../images/tutorials/create/dlz/upload-files.png)

## ファイルを [!DNL Data Landing Zone] コマンドラインインターフェイスを使用する

また、デバイスのコマンドラインインターフェイスを使用して、 [!DNL Data Landing Zone].

### Bash を使用したファイルのアップロード

次の例では、Bash と cURL を使用して、ファイルを [!DNL Data Landing Zone] と [!DNL Azure Blob Storage] REST API:

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

次の例では、 [!DNL Microsoft's] ファイルをにアップロードする Python v12 SDK [!DNL Data Landing Zone]:

>[!TIP]
>
>次の例では完全な SAS URI を使用して [!DNL Azure Blob] コンテナに含まれる場合は、他のメソッドや操作を使用して認証をおこなうことができます。 参照 [[!DNL Microsoft] Python v12 SDK のドキュメント](https://docs.microsoft.com/en-us/azure/storage/blobs/storage-quickstart-blobs-python) を参照してください。

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

### を使用してファイルをアップロード [!DNL AzCopy]

次の例では、 [!DNL Microsoft's] [!DNL AzCopy] ファイルをにアップロードするユーティリティ [!DNL Data Landing Zone]:

>[!TIP]
>
>以下の例では `copy` コマンドを使用すると、他のコマンドやオプションを使用して、ファイルを [!DNL Data Landing Zone]，次を使用 [!DNL AzCopy]. 参照 [[!DNL Microsoft AzCopy] 文書](https://docs.microsoft.com/en-us/azure/storage/common/storage-ref-azcopy?toc=/azure/storage/blobs/toc.json) を参照してください。

```bat
set sasUri=<FULL SAS URI, PROPERLY ESCAPED>
set srcFilePath=<PATH TO LOCAL FILE(S); WORKS WITH WILDCARD PATTERNS>

azcopy copy "%srcFilePath%" "%sasUri%" --overwrite=true --recursive=true
```

## 接続 [!DNL Data Landing Zone] から [!DNL Platform]

以下のドキュメントでは、 [!DNL Data Landing Zone] API またはユーザーインターフェイスを使用してAdobe Experience Platformにコンテナを追加する方法について説明します。

### API の使用

- [の作成 [!DNL Data Landing Zone] フローサービス API を使用したソース接続](../../tutorials/api/create/cloud-storage/data-landing-zone.md)
- [Flow Service API を使用して、クラウドストレージソースのデータフローを作成](../../tutorials/api/collect/cloud-storage.md)

### UI の使用

- [UI を使用した  [!DNL Data Landing Zone]  の Platform への接続](../../tutorials/ui/create/cloud-storage/data-landing-zone.md)
- [UI でクラウドストレージ接続のデータフローを作成](../../tutorials/ui/dataflow/batch/cloud-storage.md)
