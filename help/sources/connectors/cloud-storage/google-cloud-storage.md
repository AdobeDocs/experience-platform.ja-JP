---
keywords: Experience Platform;home;popular topics;Google Cloud Storage;google cloud storage
solution: Experience Platform
title: Google Cloudストレージコネクタ
topic: overview
description: 以下のドキュメントは、APIまたはユーザーインターフェイスを使用してGoogle Cloudストレージをプラットフォームに接続する方法に関する情報を提供しています。
translation-type: tm+mt
source-git-commit: e0a0b7fc28b8cc85c5140d3840e06e5c7078c307
workflow-type: tm+mt
source-wordcount: '557'
ht-degree: 4%

---


# Google Cloudストレージコネクタ

Adobe Experience Platformは、AWS、 [!DNL Google Cloud Platform]およびなどのクラウドプロバイダーに対してネイティブの接続を提供し、これらのシステムからデータを取得でき [!DNL Azure]ます。

Cloud storage sources can bring your own data into [!DNL Platform] without the need to download, format, or upload. 取り込んだデータは、XDM JSON、XDMパーケー、または区切り文字として形式設定できます。 プロセスの各手順は、Sourcesワークフローに統合されます。 [!DNL Platform] バッチを使用してデータを取り込むこ [!DNL Google Cloud Storage] とができます。

## IPアドレス許可リスト

IPアドレスのリストは、ソースコネクタを使用する前に許可リストに追加する必要があります。 地域固有のIPアドレスを許可リストに追加できないと、ソースを使用する際にエラーが発生したり、パフォーマンスが低下したりする可能性があります。 詳しくは、「 [IPアドレスの許可リスト](../../ip-address-allow-list.md) 」ページを参照してください。

## アカウント接続の前提条件の [!DNL Google Cloud Storage] 設定

に接続するに [!DNL Platform]は、最初にアカウントの相互運用性を有効にする必要があり [!DNL Google Cloud Storage] ます。 相互運用性設定にアクセスするには、を開き、ナビゲーションパネルの [!DNL Google Cloud Platform] ストレージ **[!UICONTROL (]** Settings **[!UICONTROL )オプションから「]** Settings」を選択します。

![](../../images/tutorials/create/google-cloud-storage/nav.png)

The **[!UICONTROL Settings]** page appears. ここから、 [!DNL Google] プロジェクトIDに関する情報とアカウントに関する詳細を確認でき [!DNL Google Cloud Storage] ます。 相互運用性設定にアクセスするには、上部のヘッダーで **[!UICONTROL 「Interoperability]** 」を選択します。

![](../../images/tutorials/create/google-cloud-storage/project-access.png)

[ **[!UICONTROL 相互運用性]** ]ページには、認証、アクセスキー、およびユーザアカウントに関連付けられた既定のプロジェクトに関する情報が含まれます。 相互運用可能アクセス用の既定のプロジェクトをまだ確立していない場合は、[相互運用可能アクセス用の **[!UICONTROL 既定のプロジェクト]** ]セクションで設定できます。 既定のプロジェクトが既に確立されている場合は、このセクションに、プロジェクトが既定として設定されていることを確認するメッセージが表示されます。

ユーザーアカウントの新しいアクセスキーIDと秘密鍵を生成するには、「キーを **[!UICONTROL 作成]**」を選択します。

![](../../images/tutorials/create/google-cloud-storage/interoperability.png)

新しく生成したアクセスキーIDと秘密鍵を使用して、 [!DNL Google Cloud Storage] アカウントをに接続でき [!DNL Platform]ます。

## ファイルとディレクトリの命名規則

次に、クラウドストレージのファイルやディレクトリに名前を付ける際に考慮する必要がある制約のリストを示します。

- ディレクトリ名とファイルコンポーネント名は255文字以内にする必要があります。
- ディレクトリ名とファイル名の末尾にスラッシュ(`/`)を付けることはできません。 指定した場合は、自動的に削除されます。
- 次の予約済みURL文字は、適切にエスケープする必要があります。 `! * ' ( ) ; : @ & = + $ , / ? % # [ ]`
- 次の文字は使用できません。 `" \ / : | < > * ?`.
- 無効なURLパス文字は使用できません。 NTFSファイル名で有効なコードポイント `\uE000`は、有効なUnicode文字ではありません。 また、制御文字（0x00 ～ 0x1F、\u0081など）のようなASCII文字やUnicode文字も使用できません。 HTTP/1.1でUnicode文字列を扱うルールについては、 [RFC 2616, Section 2.2を参照してください。基本規則](https://www.ietf.org/rfc/rfc2616.txt) と [RFC 3987](https://www.ietf.org/rfc/rfc3987.txt)。
- 次のファイル名は使用できません。LPT1、LPT2、LPT3、LPT4、LPT5、LPT6、LPT7、LPT8、LPT9、COM1、COM2、COM3、COM4、COM5、COM6、COM7、COM8、COM9、prn、AUX、NUL、CON、CLOCK$、ドット文字(.)および2つのドット文字(..)。

## 接続 [!DNL Google Cloud Storage] 先 [!DNL Platform]

次のドキュメントは、APIまたはユーザーインターフェイス [!DNL Google Cloud Storage] を [!DNL Platform] 使用して接続する方法に関する情報を提供しています。

### APIの使用

- [Flow Service APIを使用してGoogle Cloudストレージコネクタを作成する](../../tutorials/api/create/cloud-storage/google.md)
- [Flow Service APIを使用したクラウドストレージシステムの調査](../../tutorials/api/explore/cloud-storage.md)
- [Flow Service APIを使用してクラウドストレージデータを収集する](../../tutorials/api/collect/cloud-storage.md)

### UI の使用

- [UI での Google Cloud ストレージソースコネクタの作成](../../tutorials/ui/create/cloud-storage/google-cloud-storage.md)
- [UIでのクラウドストレージコネクタのデータフローの設定](../../tutorials/ui/dataflow/batch/cloud-storage.md)