---
title: Acxiom プロスペクティングデータの読み込み
description: UI を使用して Acxiom プロスペクティングデータを Adobe Experience Platform および Adobe Real-Time Customer Data Platform に接続する方法を説明します。
badge: ベータ版
exl-id: 6df674d9-c14b-42ea-a287-5377484e567d
source-git-commit: 9419da451616ca7f087ecea7aa66a6c10a474fb3
workflow-type: tm+mt
source-wordcount: '556'
ht-degree: 40%

---

# [!DNL Acxiom Prospecting Data Import]

>[!NOTE]
>
>[!DNL Acxiom Prospecting Data Import] ソースはベータ版です。ベータラベル付きソースの使用について詳しくは、[ ソースの概要 ](../../home.md#terms-and-conditions) を参照してください。

Adobe Experience Platformは、データパートナーアプリケーションからのデータ取り込みをサポートしています。 データおよび ID パートナーのサポートには、[!DNL Acxiom Prospecting Data Import] が含まれます。

[!DNL Acxiom] のAdobe Real-time Customer Data Platformの見込み客データインポートは、可能な限り生産的な見込み客オーディエンスを提供するプロセスです。 [!DNL Acxiom] は、Real-Time CDPのファーストパーティデータを安全な書き出しで取得し、そのデータを受賞歴のあるハイジーンおよび id 解決システムで実行します。 抑制リストとして利用できるデータファイルを生成する。 その後、このデータファイルと [!DNL Acxiom Global] データベースが照合され、見込み客リストをインポート用にカスタマイズできます。

[!DNL Acxiom] ソースを使用すると、[!DNL Amazon S3] をドロップポイントとして使用して、[!DNL Acxiom] 見込み客サービスから応答を取得し、マッピングできます。

![acxiom-prospecting-workflow](../../images/tutorials/create/acxiom-prospect-suppression-data-sourcing/acxiom-prospecting.png)

[!DNL Acxiom Prospecting Data Import] ソースアカウントの設定方法については、以下のドキュメントを参照してください。

## 前提条件

Experience Platformのバケットにアクセスするには、次の資格情報に対して有効な値を指定する必要があります。

| 資格情報 | 説明 |
| --- | --- |
| [!DNL Acxiom] 認証キー | 認証キー。 この値は [!DNL Acxiom] チームから取得できます。 |
| [!DNL Amazon S3] アクセスキー | バケットのアクセスキー ID。 この値は [!DNL Acxiom] チームから取得できます。 |
| [!DNL Amazon S3] 秘密鍵 | バケットの秘密鍵 ID。 この値は [!DNL Acxiom] チームから取得できます。 |
| バケット名 | これは、ファイルが共有されるバケットです。 この値は [!DNL Acxiom] チームから取得できます。 |

## IP アドレス許可リスト

ソースコネクタを操作する前に、IP アドレスのリストを許可リストに追加する必要があります。 地域固有の IP アドレスを許可リストに追加しないと、ソースを使用する際にエラーが発生したり、パフォーマンスが低下する場合があります。 詳しくは、[IP アドレスの許可リスト](../../ip-address-allow-list.md)ページを参照してください。

### Experience Platformーに対する権限の設定

[!DNL Acxiom Prospecting Data Import] アカウントをExperience Platformに接続するには、アカウントで **[!UICONTROL ソースの表示]** および **[!UICONTROL ソースの管理]** 権限の両方が有効になっている必要があります。 必要な権限を取得するには、製品管理者にお問い合わせください。 詳しくは、[ アクセス制御 UI ガイド ](../../../access-control/abac/ui/permissions.md) を参照してください。

## ファイルとディレクトリの命名制約

クラウドストレージファイルまたはディレクトリに名前を付ける場合は、次の制限事項を考慮する必要があります。

- ディレクトリ名とファイルコンポーネント名は 255 文字を超えてはなりません。
- ディレクトリ名とファイル名の末尾にスラッシュ（`/`）は使用できません。使用した場合、自動的に削除されます。
- 次の予約 URL 文字は、適切にエスケープする必要があります。`! ' ( ) ; @ & = + $ , % # [ ]`
- 次の文字は使用できません。`" \ / : | < > * ?`
- 無効な URL パス文字は使用できません。 `\uE000` のようなコードポイントは、NTFS ファイル名では有効ですが、有効な Unicode 文字ではありません。また、一部の ASCII 文字や Unicode 文字、例えば制御文字（0x00 ～ 0x1F、\u0081 など）も使用できません。HTTP/1.1 で Unicode 文字列を規定するルールについては、[RFC 2616、セクション 2.2：基本ルール](https://www.ietf.org/rfc/rfc2616.txt)および [RFC 3987](https://www.ietf.org/rfc/rfc3987.txt) を参照してください。
- 次のファイル名は使用できません：LPT1、LPT2、LPT3、LPT4、LPT5、LPT6、LPT7、LPT8、LPT9、COM1、COM2、COM3、COM4、COM5、COM6、COM7、COM8、COM9、PRN、AUX、NUL、CON、CLOCK$、ドット文字（.）、2 つのドット文字（..）。

## 次の手順

このドキュメントでは、[!DNL Acxiom] アカウントからExperience Platformにデータを取り込むために必要な、前提条件の設定を完了しました。 これで、ユーザーインターフェイスを使用した [Experience Platformへの接続  [!DNL Acxiom Prospecting Data Import]  に関するガイドに進むことができ ](../../tutorials/ui/create/data-partners/acxiom-prospecting-data-import.md) す。
