---
title: Acxiom データ取り込み
description: Real-time Customer Data Platformにデータを取り込み  [!DNL Acxiom]  ファーストパーティプロファイルをエンリッチメントし、オーディエンスを向上させ、複数のマーケティングチャネルにわたってアクティブ化する方法について説明します。
badge: ベータ版
exl-id: 3bbbe4e1-5e34-4104-bf39-2c452865b807
source-git-commit: 62bcaa532cdec68a2f4f62e5784c35b91b7d5743
workflow-type: tm+mt
source-wordcount: '473'
ht-degree: 43%

---

# [!DNL Acxiom Data Ingestion]

>[!NOTE]
>
>[!DNL Acxiom Prospecting Data Import] ソースはベータ版です。ベータラベル付きソースの使用について詳しくは、[ ソースの概要 ](../../home.md#terms-and-conditions) を参照してください。

[!DNL Acxiom Data Ingestion] ソースを使用すると、[!DNL Acxiom] データをReal-time Customer Data Platformに取り込み、ファーストパーティプロファイルを強化できます。 その後、エンリッチメントされ [!DNL Acxiom] ファーストパーティプロファイルを使用して、オーディエンスを向上させ、すべてのマーケティングチャネルをアクティブ化できます。

![acxiom-data-ingestion-workflow](../../images/tutorials/create/acxiom-data-enhancement-import/acxiom-data-ingestion.png)

[!DNL Acxiom Data Ingestion] ソースアカウントの設定方法については、以下のドキュメントを参照してください。

## 前提条件 {#prerequisites}

[!DNL Acxiom Data Ingestion] アカウントをExperience Platformに接続するには、次の認証資格情報の値を指定する必要があります。

| 資格情報 | 説明 |
| --- | --- |
| [!DNL Acxiom] 認証キー | 認証キー。 この値は [!DNL Acxiom] チームから取得できます。 |
| [!DNL Amazon S3] アクセスキー | バケットのアクセスキー ID。 この値は [!DNL Acxiom] チームから取得できます。 |
| [!DNL Amazon S3] 秘密鍵 | バケットの秘密鍵 ID。 この値は [!DNL Acxiom] チームから取得できます。 |
| バケット名 | これは、ファイルが共有されるバケットです。 この値は [!DNL Acxiom] チームから取得できます。 |

## IP アドレス許可リスト

ソースコネクタを操作する前に、IP アドレスのリストを許可リストに追加する必要があります。 地域固有の IP アドレスを許可リストに追加しないと、ソースを使用する際にエラーが発生したり、パフォーマンスが低下する場合があります。 詳しくは、[IP アドレスの許可リスト](../../ip-address-allow-list.md)ページを参照してください。

### Experience Platformーに対する権限の設定

[!DNL Acxiom Data Ingestion] アカウントをExperience Platformに接続するには、アカウントで **[!UICONTROL ソースの表示]** および **[!UICONTROL ソースの管理]** 権限の両方が有効になっている必要があります。 必要な権限を取得するには、製品管理者にお問い合わせください。 詳しくは、[ アクセス制御 UI ガイド ](../../../access-control/ui/overview.md) を参照してください。

### ファイルとディレクトリの命名制約

クラウドストレージファイルまたはディレクトリに名前を付ける場合は、次の制限事項を考慮する必要があります。

- ディレクトリ名とファイルコンポーネント名は 255 文字を超えてはなりません。
- ディレクトリ名とファイル名の末尾にスラッシュ（`/`）は使用できません。使用した場合、自動的に削除されます。
- 次の予約 URL 文字は、適切にエスケープする必要があります。`! ' ( ) ; @ & = + $ , % # [ ]`
- 次の文字は使用できません。`" \ / : | < > * ?`
- 無効な URL パス文字は使用できません。 `\uE000` のようなコードポイントは、NTFS ファイル名では有効ですが、有効な Unicode 文字ではありません。また、一部の ASCII 文字や Unicode 文字、例えば制御文字（0x00 ～ 0x1F、\u0081 など）も使用できません。HTTP/1.1 で Unicode 文字列を規定するルールについては、[RFC 2616、セクション 2.2：基本ルール](https://www.ietf.org/rfc/rfc2616.txt)および [RFC 3987](https://www.ietf.org/rfc/rfc3987.txt) を参照してください。
- 次のファイル名は使用できません：LPT1、LPT2、LPT3、LPT4、LPT5、LPT6、LPT7、LPT8、LPT9、COM1、COM2、COM3、COM4、COM5、COM6、COM7、COM8、COM9、PRN、AUX、NUL、CON、CLOCK$、ドット文字（.）、2 つのドット文字（..）。

## 次の手順

このドキュメントでは、[!DNL Acxiom] アカウントからExperience Platformにデータを取り込むために必要な、前提条件の設定を完了しました。 これで、ユーザーインターフェイスを使用した [Experience Platformへの接続  [!DNL Acxiom Data Ingestion]  に関するガイドに進むことができ ](../../tutorials/ui/create/data-partners/acxiom-data-ingestion.md) す。
