---
keywords: Experience Platform；ホーム；人気のトピック；Google Cloud Storage;google cloud storage
solution: Experience Platform
title: Google Cloud Storage Source コネクタの概要
description: API またはユーザーインターフェイスを使用してGoogle クラウドストレージをAdobe Experience Platformに接続する方法について説明します。
exl-id: f7ebd213-f914-4c49-aebd-1df4514ffec0
source-git-commit: f129c215ebc5dc169b9a7ef9b3faa3463ab413f3
workflow-type: tm+mt
source-wordcount: '616'
ht-degree: 62%

---

# Google Cloud Storage コネクタ

>[!IMPORTANT]
>
>Amazon Web Services（AWS）でAdobe Experience Platformを実行するときに、[!DNL Google Cloud Storage] ソースを使用できるようになりました。 AWS上で動作するExperience Platformは、現在、限られた数のお客様が利用できます。 サポートされるExperience Platform インフラストラクチャについて詳しくは、[Experience Platform multi-cloud overview](../../../landing/multi-cloud.md) を参照してください。

Adobe Experience Platform には、AWS、[!DNL Google Cloud Platform]、[!DNL Azure] などのクラウドプロバイダーとのネイティブ接続が用意されており、これらのシステムからデータを取り込むことができます。

クラウドストレージソースを使用すると、ダウンロード、フォーマット、アップロードを行う必要なく、独自のデータをExperience Platformに取り込むことができます。 取り込んだデータは、Experience Data Model （XDM）に準拠した JSON や Parquet として書式設定することも、区切り形式として書式設定することもできます。 プロセスのすべての手順がソースワークフローに統合されます。 Experience Platformでは、[!DNL Google Cloud Storage] からバッチでデータを取り込むことができます。

## IP アドレス許可リスト

ソースコネクタを操作する前に、IP アドレスのリストを許可リストに追加する必要があります。 地域固有の IP アドレスを許可リストに追加しないと、ソースを使用する際にエラーが発生したり、パフォーマンスが低下する場合があります。 詳しくは、[IP アドレスの許可リスト](../../ip-address-allow-list.md)ページを参照してください。

## [!DNL Google Cloud Storage] アカウントを接続するための前提条件の設定

Experience Platformに接続するには、まず [!DNL Google Cloud Storage] アカウントの相互運用性を有効にする必要があります。 相互運用性設定にアクセスするには、[!DNL Google Cloud Platform] を開き、ナビゲーションパネルの「**[!UICONTROL クラウドストレージ]**」オプションから「**[!UICONTROL 設定]**」を選択します。

<!-- ![](../../images/tutorials/create/google-cloud-storage/nav.png) -->

**[!UICONTROL 設定]**&#x200B;ページが表示されます。ここから、[!DNL Google] プロジェクト ID に関する情報と [!DNL Google Cloud Storage] アカウントの詳細を確認できます。相互運用性設定にアクセスするには、上部のヘッダーから「**[!UICONTROL 相互運用性]**」を選択します。

<!-- ![](../../images/tutorials/create/google-cloud-storage/project-access.png) -->

**[!UICONTROL 相互運用性]**&#x200B;ページには、認証、アクセスキーおよびサービスアカウントに関連付けられたデフォルトプロジェクトに関する情報が含まれています。サービスアカウントの新しいアクセスキー ID と秘密アクセスキーを生成するには、「**[!UICONTROL サービスアカウントのキーを作成]**」を選択します。

<!-- ![](../../images/tutorials/create/google-cloud-storage/interoperability.png) -->

新しく生成されたアクセスキー ID と秘密アクセスキーを使用して、[!DNL Google Cloud Storage] アカウントをExperience Platformに接続できます。

詳しくは、[!DNL Google Cloud] ドキュメントの [&#x200B; サービスアカウントキーの作成と管理 &#x200B;](https://cloud.google.com/iam/docs/creating-managing-service-account-keys) に関するガイドを参照してください。

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
