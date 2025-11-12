---
title: Data Landing Zone Source
description: データランディングゾーンをAdobe Experience Platformに接続する方法を学ぶ
exl-id: bdc10095-7de4-4183-bfad-a7b5c89197e3
source-git-commit: 06b2108715ce368ff4ecf5c6c7dd3a327d9f61b1
workflow-type: tm+mt
source-wordcount: '1361'
ht-degree: 18%

---

# [!DNL Data Landing Zone]

>[!IMPORTANT]
>
>このページは、Experience Platformの [!DNL Data Landing Zone] *ソース* コネクタに固有のページです。 [!DNL Data Landing Zone] *宛先* コネクタへの接続について詳しくは、[[!DNL Data Landing Zone]  宛先ドキュメントページ &#x200B;](/help/destinations/catalog/cloud-storage/data-landing-zone.md) を参照してください。

[!DNL Data Landing Zone] はAdobe Experience Platformによってプロビジョニングされた [!DNL Azure Blob] ストレージインターフェイスであり、ファイルをExperience Platformに取り込むための安全なクラウドベースのファイルストレージ機能にアクセスできます。 サンドボックスごとに 1 つの [!DNL Data Landing Zone] コンテナに対するアクセス権があります。すべてのコンテナの合計データ量は、Experience Platform製品およびサービスライセンスで提供される合計データ量に制限されます。 Experience Platformのすべてのユーザーは、サンドボックスごとに 1 つの [!DNL Data Landing Zone] コンテナを使用してプロビジョニングされます。 [!DNL Azure Storage Explorer] またはコマンドラインインターフェイスを通じて、コンテナに対してファイルの読み取りと書き込みを行うことができます。

[!DNL Data Landing Zone] は SAS ベースの認証をサポートし、そのデータは保存時および転送中は標準 [!DNL Azure Blob] ストレージセキュリティメカニズムで保護されます。SAS ベースの認証を使用すると、パブリックインターネット接続を介して [!DNL Data Landing Zone] コンテナに安全にアクセスできます。[!DNL Data Landing Zone] コンテナにアクセスする場合、ネットワークの変更は必要ありません。つまり、ネットワーク用に許可リストや地域間設定を行う必要はありません。 Experience Platformでは、[!DNL Data Landing Zone] コンテナにアップロードされるすべてのファイルおよびフォルダーに対して厳密に 7 日間の有効期限が適用されます。 すべてのファイルとフォルダーは、7 日後に削除されます。

## Azure 上のExperience Platformの [!DNL Data Landing Zone] ソースを設定する {#azure}

Azure でExperience Platform用に [!DNL Data Landing Zone] アカウントを設定する方法については、次の手順に従います。

>[!NOTE]
>
>[!DNL Data Landing Zone] から [!DNL Azure Data Factory] にアクセスする場合は、Experience Platformから提供される [!DNL Data Landing Zone]SAS 資格情報 [&#x200B; を使用して、](../../tutorials/ui/create/cloud-storage/data-landing-zone.md#retrieve-your-data-landing-zone-credentials) 用にリンクされたサービスを作成する必要があります。 リンクされたサービスを作成したら、デフォルトのルートパスの代わりにコンテナパスを選択して、サー [!DNL Data Landing Zone] スを参照できます。

### ファイルとディレクトリの命名制約

クラウドストレージファイルまたはディレクトリに名前を付ける際に考慮する必要がある制約のリストを次に示します。

- ディレクトリ名とファイルコンポーネント名は 255 文字を超えてはなりません。
- ディレクトリ名とファイル名の末尾にスラッシュ（`/`）は使用できません。使用した場合、自動的に削除されます。
- 次の予約 URL 文字は、適切にエスケープする必要があります。`! ' ( ) ; @ & = + $ , % # [ ]`
- 次の文字は使用できません。`" \ / : | < > * ?`
- 無効な URL パス文字は使用できません。`\uE000` のようなコードポイントは、NTFS ファイル名では有効ですが、有効な Unicode 文字ではありません。さらに、制御文字（`0x00` から `0x1F`、`\u0081` など）など、一部の ASCII 文字または Unicode 文字も許可されていません。 HTTP/1.1 で Unicode 文字列を規定するルールについては、[RFC 2616、セクション 2.2：基本ルール](https://www.ietf.org/rfc/rfc2616.txt)および [RFC 3987](https://www.ietf.org/rfc/rfc3987.txt) を参照してください。
- 次のファイル名は使用できません：LPT1、LPT2、LPT3、LPT4、LPT5、LPT6、LPT7、LPT8、LPT9、COM1、COM2、COM3、COM4、COM5、COM6、COM7、COM8、COM9、PRN、AUX、NUL、CON、CLOCK$、ドット文字（.）、2 つのドット文字（..）。

### データランディングゾーンのコンテンツを管理{#manage-the-contents-of-your-data-landing-zone}

[[!DNL Azure Storage Explorer]](https://azure.microsoft.com/ja-jp/features/storage-explorer/) を使用して [!DNL Data Landing Zone] コンテナのコンテンツを管理することができます。

[!DNL Azure Storage Explorer] UI で、左側のナビゲーションにある「接続」アイコンを選択します。 **リソースを選択**&#x200B;ウィンドウが開き、接続するオプションが表示されます。**[!DNL Blob container]** に接続する [!DNL Data Landing Zone] を選択してください。

![Azure エクスプローラーのリソース ワークスペースの選択。](../../images/tutorials/create/dlz/select-resource.png)

次に、接続方法として「**共有アクセス署名 URL (SAS)**」を選択し、「**次へ**」をクリックします。

![&#x200B; 共有アクセス署名が選択された Azure エクスプローラーの接続方法を選択 &#x200B;](../../images/tutorials/create/dlz/select-connection-method.png)

接続方法を選択した後、次に、**コンテナに対応する** 表示名 **[!DNL Blob]および** コンテナ SAS URL[!DNL Data Landing Zone] を指定する必要があります。

>[!TIP]
>
>Experience Platform UI のソースカタログから [!DNL Data Landing Zone] の資格情報を取得できます。

[!DNL Data Landing Zone] SAS URL を入力し、「次へ **を選択します**

![&#x200B; 表示名と SAS URL が入力されている Azure エクスプローラーの接続情報を入力ワークスペース。](../../images/tutorials/create/dlz/enter-connection-info.png)

**概要**&#x200B;ウィンドウが開き、[!DNL Blob] エンドポイントと権限を含む設定の概要が表示されます。準備ができたら、「**接続**」を選択します。

![&#x200B; リソース接続の設定を再度取り込む Azure エクスプローラーの概要ワークスペース。](../../images/tutorials/create/dlz/summary.png)

接続が成功すると、[!DNL Azure Storage Explorer] UI と [!DNL Data Landing Zone] コンテナが更新されます。

![Azure エクスプローラーのデータランディングゾーンナビゲーションワークスペース。](../../images/tutorials/create/dlz/dlz-user-container.png)

[!DNL Data Landing Zone] コンテナが [!DNL Azure Storage Explorer] に接続され、[!DNL Data Landing Zone] コンテナへのファイルのアップロードを開始できるようになりました。 アップロードするには、「**アップロード**」を選択し、「**ファイルをアップロード**」を選択します。

![Azure エクスプローラーのファイルのアップロードワークスペース。](../../images/tutorials/create/dlz/upload.png)

アップロードするファイルを選択したら、そのファイルをアップロードする [!DNL Blob] タイプと目的の宛先ディレクトリを特定する必要があります。 終了したら「**アップロード**」を選択します。

| [!DNL Blob] タイプ | 説明 |
| --- | --- |
| ブロック [!DNL Blob] | ブロック [!DNL Blobs] は、大量のデータを効率的にアップロードするために最適化されています。 ブロック [!DNL Blobs] は、[!DNL Data Landing Zone] の既定のオプションです。 |
| Append [!DNL Blob] | 追加 [!DNL Blobs] は、ファイルの最後にデータを追加する場合に最適化されています。 |

![Azure エクスプローラーのファイルのアップロードウィンドウ。選択したファイル、BLOB タイプおよび宛先カテゴリが表示されます。](../../images/tutorials/create/dlz/upload-files.png)

### コマンドラインインターフェイスを使用した [!DNL Data Landing Zone] へのファイルのアップロード

また、デバイスのコマンドラインインターフェイスを使用して、[!DNL Data Landing Zone] ーバーへのアップロードファイルにアクセスすることもできます。

### Bash を使用したファイルのアップロード

次の例では、Bash と cURL を使用して、[!DNL Data Landing Zone] REST API でファイルを [!DNL Azure Blob Storage] にアップロードします。

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

次の例では [!DNL Microsoft's]Python v12 SDKを使用して、ファイルを [!DNL Data Landing Zone] にアップロードします。

>[!TIP]
>
>次の例では、完全な SAS URI を使用して [!DNL Azure Blob] コンテナに接続しますが、他の方法や操作を使用して認証することもできます。 詳しくは、こちらの [[!DNL Microsoft] Python v12 SDKのドキュメント &#x200B;](https://docs.microsoft.com/en-us/azure/storage/blobs/storage-quickstart-blobs-python) を参照してください。

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

次の例では、[!DNL Microsoft's] ユーティリティ [!DNL AzCopy] 使用してファイルを [!DNL Data Landing Zone] にアップロードします。

>[!TIP]
>
>以下の例では `copy` コマンドを使用していますが、他のコマンドおよびオプションを使用して、[!DNL Data Landing Zone] を使用して [!DNL AzCopy] にファイルをアップロードできます。 詳しくは、この [[!DNL Microsoft AzCopy]  ドキュメント &#x200B;](https://docs.microsoft.com/en-us/azure/storage/common/storage-ref-azcopy?toc=/azure/storage/blobs/toc.json) を参照してください。

```bat
set sasUri=<FULL SAS URI, PROPERLY ESCAPED>
set srcFilePath=<PATH TO LOCAL FILE(S); WORKS WITH WILDCARD PATTERNS>

azcopy copy "%srcFilePath%" "%sasUri%" --overwrite=true --recursive=true
```

## Amazon Web Services上のExperience Platform用の [!DNL Data Landing Zone] ソースの設定 {#aws}

>[!AVAILABILITY]
>
>この節の内容は、Amazon Web Services（AWS）上で動作するExperience Platformの実装に適用されます。 AWS上で動作するExperience Platformは、現在、限られた数のお客様が利用できます。 サポートされるExperience Platform インフラストラクチャについて詳しくは、[Experience Platform multi-cloud overview](https://experienceleague.adobe.com/ja/docs/experience-platform/landing/multi-cloud) を参照してください。

Amazon Web Services（AWS）上のExperience Platform用に [!DNL Data Landing Zone] アカウントを設定する方法については、次の手順に従います。

### 許可リストに加える AWSでの接続用 IP アドレス。

ソースをAWSのExperience Platformに接続する前に、地域固有の IP アドレスを許可リストに追加する必要があります。 詳しくは、[AWSでExperience Platformに接続するための IP アドレスの許可リストに加える](../../ip-address-allow-list.md) に関するガイドを参照してください。

### AWS CLI の設定と操作の実行

- [AWS CLI の最新バージョンへのインストールまたはアップデート &#x200B;](https://docs.aws.amazon.com/cli/latest/userguide/getting-started-install.html) に関するガイドを参照。

### 一時的な資格情報を使用してAWS CLI を設定

AWS `configure` コマンドを使用して、アクセスキーとセッショントークンを使用して CLI を設定します。

```shell
aws configure
```

プロンプトが表示されたら、次の値を入力します。

- AWS アクセスキー ID : `{YOUR_ACCESS_KEY_ID}`
- AWS秘密アクセスキー：`{YOUR_SECRET_ACCESS_KEY}`
- デフォルトの地域名：`{YOUR_REGION}` （例：`us-west-2`）
- デフォルトの出力形式：`json`

次に、セッショントークンを設定します。

```shell
aws configure set aws_session_token your-session-token
```

### [!DNL Amazon S3] でのファイルの操作

>[!BEGINTABS]

>[!TAB Amazon S3 へのファイルのアップロード ]

テンプレート：

```shell
aws s3 cp local-file-path s3://bucketName/dlzFolder/remote-file-Name
```

例：

```shell
aws s3 cp example.txt s3://bucketName/dlzFolder/example.txt
```


>[!TAB Amazon S3 からのファイルのダウンロード ]

テンプレート：

```shell
aws s3 cp s3://bucketName/dlzFolder/remote-file local-file-path
```

例：

```shell
aws s3 cp s3://bucketName/dlzFolder/example.txt example.txt
```

>[!ENDTABS]

### [!DNL Data Landing Zone] 資格情報を使用してAWS コンソールにログインします

#### 認証情報を抽出

まず、次の情報を取得する必要があります。

- `awsAccessKeyId`
- `awsSecretAccessKey`
- `awsSessionToken`

#### ログイントークンの生成

次に、抽出した資格情報を使用してセッションを作成し、AWS フェデレーションエンドポイントを使用してログイントークンを生成します。

```py
import json
import requests
 
# Example DLZ response with credentials
response_json = '''{
    "credentials": {
        "awsAccessKeyId": "your-access-key",
        "awsSecretAccessKey": "your-secret-key",
        "awsSessionToken": "your-session-token"
    }
}'''
 
# Parse credentials
response_data = json.loads(response_json)
aws_access_key_id = response_data['credentials']['awsAccessKeyId']
aws_secret_access_key = response_data['credentials']['awsSecretAccessKey']
aws_session_token = response_data['credentials']['awsSessionToken']
 
# Create session dictionary
session = {
    'sessionId': aws_access_key_id,
    'sessionKey': aws_secret_access_key,
    'sessionToken': aws_session_token
}
 
# Generate the sign-in token
signin_token_url = "https://signin.aws.amazon.com/federation"
signin_token_payload = {
    "Action": "getSigninToken",
    "Session": json.dumps(session)
}
signin_token_response = requests.post(signin_token_url, data=signin_token_payload)
signin_token = signin_token_response.json()['SigninToken']
```

#### AWS コンソールのログイン URL の作成

ログイントークンを取得したら、AWS コンソールにログインし、目的の [!DNL Amazon S3] バケットを直接指す URL を作成できます。

```py
from urllib.parse import quote
 
# Define the S3 bucket and folder path you want to access
bucket_name = "your-bucket-name"
bucket_path = "your-bucket-folder"
 
# Construct the destination URL
destination_url = f"https://s3.console.aws.amazon.com/s3/buckets/{bucket_name}?prefix={bucket_path}/&tab=objects"
 
# Create the final sign-in URL
signin_url = f"https://signin.aws.amazon.com/federation?Action=login&Issuer=YourAppName&Destination={quote(destination_url)}&SigninToken={signin_token}"
 
print(f"Sign-in URL: {signin_url}")
```

#### AWS コンソールへのアクセス

最後に、生成された URL に移動して、[!DNL Data Landing Zone] 資格情報を使用してAWS コンソールに直接ログインします。この資格情報を使用して、[!DNL Amazon S3] バケット内の特定のフォルダーにアクセスできます。 ログイン URL を使用すると、そのフォルダーに直接移動し、許可されたデータのみを表示および管理できるようになります。

## [!DNL Data Landing Zone] をExperience Platformに接続

>[!IMPORTANT]
>
>- ソースに接続するには、**[!UICONTROL View Sources]** および **[!UICONTROL Manage Sources]** アクセス制御権限が必要です。 詳しくは、[&#x200B; アクセス制御の概要 &#x200B;](../../../access-control/home.md) を参照するか、製品管理者に問い合わせて、必要な権限を取得してください。
>
>- [!DNL Data Landing Zone] を使用してExperience Platformに接続する場合、プライベートリンクは現在サポートされていません。 アクセスでサポートされているメソッドは、[&#x200B; こちら &#x200B;](#manage-the-contents-of-your-data-landing-zone) に示すメソッドのみです。

以下のドキュメントでは、API またはユーザーインターフェイスを使用して、[!DNL Data Landing Zone] コンテナからAdobe Experience Platformにデータを取り込む方法に関する情報を提供します。

### API の使用

- [Flow Service API [!DNL Data Landing Zone]  使用したソース接続の作成](../../tutorials/api/create/cloud-storage/data-landing-zone.md)
- [Flow Service API を使用して、クラウドストレージソースのデータフローを作成](../../tutorials/api/collect/cloud-storage.md)

### UI の使用

- [UI [!DNL Data Landing Zone]  使用したExperience Platformへの接続](../../tutorials/ui/create/cloud-storage/data-landing-zone.md)
- [UI でクラウドストレージ接続のデータフローを作成](../../tutorials/ui/dataflow/batch/cloud-storage.md)

