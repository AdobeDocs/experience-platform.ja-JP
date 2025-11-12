---
keywords: Experience Platform；ホーム；人気のトピック；Google Cloud Storage;google cloud storage
solution: Experience Platform
title: Google Cloud Storage Source コネクタの概要
description: API またはユーザーインターフェイスを使用してGoogle クラウドストレージをAdobe Experience Platformに接続する方法について説明します。
exl-id: f7ebd213-f914-4c49-aebd-1df4514ffec0
source-git-commit: 06b2108715ce368ff4ecf5c6c7dd3a327d9f61b1
workflow-type: tm+mt
source-wordcount: '589'
ht-degree: 44%

---

# Google Cloud Storage コネクタ

>[!IMPORTANT]
>
>Amazon Web Services（AWS）でAdobe Experience Platformを実行するときに、[!DNL Google Cloud Storage] ソースを使用できるようになりました。 AWS上で動作するExperience Platformは、現在、限られた数のお客様が利用できます。 サポートされるExperience Platform インフラストラクチャについて詳しくは、[Experience Platform multi-cloud overview](../../../landing/multi-cloud.md) を参照してください。

Adobe Experience Platform には、AWS、[!DNL Google Cloud Platform]、[!DNL Azure] などのクラウドプロバイダーとのネイティブ接続が用意されており、これらのシステムからデータを取り込むことができます。

クラウドストレージソースを使用すると、ダウンロード、フォーマット、アップロードを行う必要なく、独自のデータをExperience Platformに取り込むことができます。 取り込んだデータは、Experience Data Model （XDM）に準拠した JSON や Parquet として書式設定することも、区切り形式として書式設定することもできます。 プロセスのすべての手順がソースワークフローに統合されます。 Experience Platformでは、[!DNL Google Cloud Storage] からバッチでデータを取り込むことができます。

## IP アドレスの許可リスト

ソースをExperience Platformに接続する前に、地域固有の IP アドレスを許可リストに追加する必要があります。 詳しくは、[Experience Platformへの接続に対する IP アドレスの許可リストに加える](../../ip-address-allow-list.md) に関するガイドを参照してください。

## [!DNL Google Cloud Storage] アカウントを接続するための前提条件の設定

Experience Platformに接続するには、まず [!DNL Google Cloud Storage] アカウントの相互運用性を有効にする必要があります。 相互運用性設定にアクセスするには、[!DNL Google Cloud Platform] を開き、ナビゲーションパネルの「**[!UICONTROL Settings]**」オプションから「**[!UICONTROL Cloud Storage]**」を選択します。

<!-- ![](../../images/tutorials/create/google-cloud-storage/nav.png) -->

**[!UICONTROL Settings]** ページが表示されます。 ここから、[!DNL Google] プロジェクト ID に関する情報と [!DNL Google Cloud Storage] アカウントの詳細を確認できます。相互運用性設定にアクセスするには、上部のヘッダーから「**[!UICONTROL Interoperability]**」を選択します。

<!-- ![](../../images/tutorials/create/google-cloud-storage/project-access.png) -->

**[!UICONTROL Interoperability]** ページには、認証、アクセスキーおよびサービスアカウントに関連付けられたデフォルトプロジェクトに関する情報が含まれています。 サービスアカウントの新しいアクセスキー ID と秘密アクセスキーを生成するには、「**[!UICONTROL Create a Key for a Service Account]**」を選択します。

<!-- ![](../../images/tutorials/create/google-cloud-storage/interoperability.png) -->

新しく生成されたアクセスキー ID と秘密アクセスキーを使用して、[!DNL Google Cloud Storage] アカウントをExperience Platformに接続できます。

詳しくは、[&#x200B; ドキュメントの &#x200B;](https://cloud.google.com/iam/docs/creating-managing-service-account-keys) サービスアカウントキーの作成と管理 [!DNL Google Cloud] に関するガイドを参照してください。

## ファイルとディレクトリの命名制約

クラウドストレージファイルまたはディレクトリに名前を付ける際に考慮する必要がある制約のリストを次に示します。

- ディレクトリ名とファイルコンポーネント名は 255 文字を超えてはなりません。
- ディレクトリ名とファイル名の末尾にスラッシュ（`/`）は使用できません。使用した場合、自動的に削除されます。
- 次の予約 URL 文字は、適切にエスケープする必要があります。`! ' ( ) ; @ & = + $ , % # [ ]`
- 次の文字は使用できません。`" \ / : | < > * ?`
- 無効な URL パス文字は使用できません。`\uE000` のようなコードポイントは、NTFS ファイル名では有効ですが、有効な Unicode 文字ではありません。また、一部の ASCII 文字や Unicode 文字、例えば制御文字（0x00 ～ 0x1F、\u0081 など）も使用できません。HTTP/1.1 で Unicode 文字列を規定するルールについては、[RFC 2616、セクション 2.2：基本ルール](https://www.ietf.org/rfc/rfc2616.txt)および [RFC 3987](https://www.ietf.org/rfc/rfc3987.txt) を参照してください。
- 次のファイル名は使用できません：LPT1、LPT2、LPT3、LPT4、LPT5、LPT6、LPT7、LPT8、LPT9、COM1、COM2、COM3、COM4、COM5、COM6、COM7、COM8、COM9、PRN、AUX、NUL、CON、CLOCK$、ドット文字（.）、2 つのドット文字（..）。

## [!DNL Google Cloud Storage] をExperience Platformに接続

以下のドキュメントでは、API やユーザーインターフェイスを使用して [!DNL Google Cloud Storage] をExperience Platformに接続する方法について説明しています。

### API の使用

- [Flow Service API を使用したGoogle クラウドストレージベース接続の作成](../../tutorials/api/create/cloud-storage/google.md)
- [Flow Service API を使用して、クラウドストレージソースのデータ構造とコンテンツを探索](../../tutorials/api/explore/cloud-storage.md)
- [Flow Service API を使用して、クラウドストレージソースのデータフローを作成](../../tutorials/api/collect/cloud-storage.md)

### UI の使用

- [UI でのGoogle Cloud Storage ソース接続の作成](../../tutorials/ui/create/cloud-storage/google-cloud-storage.md)
- [UI でクラウドストレージ接続のデータフローを作成](../../tutorials/ui/dataflow/batch/cloud-storage.md)
