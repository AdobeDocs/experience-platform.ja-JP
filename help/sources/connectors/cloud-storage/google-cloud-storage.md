---
keywords: Experience Platform；ホーム；人気のあるトピック；Google Cloud Storage;google クラウドストレージ
solution: Experience Platform
title: Google Cloud Storage Source コネクタの概要
topic-legacy: overview
description: API またはユーザーインターフェイスを使用してGoogle Cloud Storage をAdobe Experience Platformに接続する方法を説明します。
exl-id: f7ebd213-f914-4c49-aebd-1df4514ffec0
source-git-commit: 1f9948d6e419ee5d6a021a589378f7aa990b7291
workflow-type: tm+mt
source-wordcount: '551'
ht-degree: 5%

---

# Google Cloud Storage コネクタ

Adobe Experience Platformは、AWS、[!DNL Google Cloud Platform]、[!DNL Azure] などのクラウドプロバイダーにネイティブ接続を提供し、これらのシステムからデータを取り込むことができます。

クラウドストレージソースを使用すると、ダウンロード、フォーマット、アップロードをおこなう必要なく、独自のデータを Platform に取り込むことができます。取り込んだデータは、エクスペリエンスデータモデル (XDM) に準拠した JSON 形式または Parquet 形式の形式にするか、区切り形式の形式にすることができます。 プロセスの各ステップは、ソースワークフローに統合されます。 Platform では、[!DNL Google Cloud Storage] からバッチを使用してデータを取り込むことができます。

## IP アドレス許可リスト

ソースコネクタを操作する前に、IP アドレスのリストを許可リストに追加する必要があります。 地域固有の IP アドレスを許可リストに追加しないと、ソースを使用する際にエラーが発生したり、パフォーマンスが低下する可能性があります。 詳しくは、[IP アドレスの許可リスト](../../ip-address-allow-list.md) ページを参照してください。

## [!DNL Google Cloud Storage] アカウントを接続するための前提条件の設定

Platform に接続するには、まず [!DNL Google Cloud Storage] アカウントの相互運用性を有効にする必要があります。 相互運用性の設定にアクセスするには、[!DNL Google Cloud Platform] を開き、ナビゲーションパネルの「**[!UICONTROL クラウドストレージ]**」オプションから「**[!UICONTROL 設定]**」を選択します。

![](../../images/tutorials/create/google-cloud-storage/nav.png)

**[!UICONTROL 設定]** ページが表示されます。 ここから、[!DNL Google] プロジェクト ID に関する情報と [!DNL Google Cloud Storage] アカウントの詳細を確認できます。 相互運用性の設定にアクセスするには、上部のヘッダーから「**[!UICONTROL Interoperability]**」を選択します。

![](../../images/tutorials/create/google-cloud-storage/project-access.png)

**[!UICONTROL Interoperability]** ページには、認証、アクセスキー、およびサービスアカウントに関連付けられたデフォルトのプロジェクトに関する情報が含まれています。 新しいアクセスキー ID とサービスアカウントの秘密アクセスキーを生成するには、**[!UICONTROL Create a Key for a Service Account]** を選択します。

![](../../images/tutorials/create/google-cloud-storage/interoperability.png)

新しく生成したアクセスキー ID と秘密アクセスキーを使用して、[!DNL Google Cloud Storage] アカウントを Platform に接続できます。

## ファイルとディレクトリの命名制約

以下は、クラウドストレージファイルまたはディレクトリに名前を付ける際に考慮する必要がある制約のリストです。

- ディレクトリとファイルコンポーネントの名前は 255 文字以下にする必要があります。
- ディレクトリ名とファイル名の末尾にスラッシュ (`/`) を付けることはできません。 指定した場合、自動的に削除されます。
- 次の予約 URL 文字は、適切にエスケープする必要があります。`! ' ( ) ; @ & = + $ , % # [ ]`
- 次の文字は使用できません。`" \ / : | < > * ?`.
- 無効な URL パス文字は使用できません。 `\uE000` のようなコードポイントは、NTFS ファイル名では有効ですが、有効な Unicode 文字ではありません。 また、制御文字（0x00 ～ 0x1F、\u0081 など）など、ASCII 文字や Unicode 文字の一部も使用できません。 HTTP/1.1 で Unicode 文字列を規定するルールについては、[RFC 2616、Section 2.2 を参照してください。基本規則 ](https://www.ietf.org/rfc/rfc2616.txt) と [RFC 3987](https://www.ietf.org/rfc/rfc3987.txt)。
- 次のファイル名は使用できません。LPT1, LPT2, LPT3, LPT4, LPT5, LPT6, LPT7, LPT8, LPT9, COM1, COM2, COM3, COM4, COM5, COM6, COM8, COM9,prn、AUX、NUL、CON、CLOCK$、ドット文字 (.)、2 つのドット文字 (..)。

## [!DNL Google Cloud Storage] を Platform に接続

以下のドキュメントでは、API またはユーザーインターフェイスを使用して [!DNL Google Cloud Storage] を Platform に接続する方法について説明します。

### API の使用

- [フローサービス API を使用したGoogle Cloud Storage ベース接続の作成](../../tutorials/api/create/cloud-storage/google.md)
- [フローサービス API を使用したクラウドストレージソースのデータ構造とコンテンツの調査](../../tutorials/api/explore/cloud-storage.md)
- [フローサービス API を使用したクラウドストレージソースのデータフローの作成](../../tutorials/api/collect/cloud-storage.md)

### UI の使用

- [UI でのGoogle Cloud ストレージソース接続の作成](../../tutorials/ui/create/cloud-storage/google-cloud-storage.md)
- [UI でのクラウドストレージ接続のデータフローの作成](../../tutorials/ui/dataflow/batch/cloud-storage.md)
