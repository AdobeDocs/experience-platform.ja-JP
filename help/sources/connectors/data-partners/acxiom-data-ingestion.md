---
title: Acxiom データ取り込み
description: Real-Time Customer Data Platformにデータを取り込み  [!DNL Acxiom]  ファーストパーティプロファイルをエンリッチメントし、オーディエンスを向上させ、複数のマーケティングチャネルにわたってアクティブ化する方法について説明します。
exl-id: 3bbbe4e1-5e34-4104-bf39-2c452865b807
source-git-commit: e402a58f51de49b26f9d279cebf551ec11e4698f
workflow-type: tm+mt
source-wordcount: '461'
ht-degree: 32%

---

# [!DNL Acxiom Data Ingestion]

[!DNL Acxiom Data Ingestion] ソースを使用すると、[!DNL Acxiom] データをReal-Time Customer Data Platformに取り込み、ファーストパーティプロファイルを強化できます。 その後、エンリッチメントされ [!DNL Acxiom] ファーストパーティプロファイルを使用して、オーディエンスを向上させ、すべてのマーケティングチャネルをアクティブ化できます。

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

## IP アドレスの許可リスト

ソースコネクタを使用する前に、地域に必要な IP アドレスを許可リストに追加する必要があります。 これらの IP アドレスを追加しないと、ソースコネクタが正しく動作しないか、エラーが発生する可能性があります。 許可リストに加える詳細な手順と許可する IP アドレスの一覧については、[IP アドレス &#x200B;](../../ip-address-allow-list.md) を参照してください。

### Experience Platformに対する権限の設定

**[!UICONTROL アカウントをExperience Platformに接続するには、アカウントで]** ソースの表示 **[!UICONTROL および]** ソースの管理 [!DNL Acxiom Data Ingestion] 権限の両方が有効になっている必要があります。 必要な権限を取得するには、製品管理者にお問い合わせください。 詳しくは、[&#x200B; アクセス制御 UI ガイド &#x200B;](../../../access-control/ui/overview.md) を参照してください。

### ファイルとディレクトリの命名制約

クラウドストレージファイルまたはディレクトリに名前を付ける場合は、次の制限事項を考慮する必要があります。

- ディレクトリ名とファイルコンポーネント名は 255 文字を超えてはなりません。
- ディレクトリ名とファイル名の末尾にスラッシュ（`/`）は使用できません。使用した場合、自動的に削除されます。
- 次の予約 URL 文字は、適切にエスケープする必要があります。`! ' ( ) ; @ & = + $ , % # [ ]`
- 次の文字は使用できません。`" \ / : | < > * ?`
- 無効な URL パス文字は使用できません。 `\uE000` のようなコードポイントは、NTFS ファイル名では有効ですが、有効な Unicode 文字ではありません。また、一部の ASCII 文字や Unicode 文字、例えば制御文字（0x00 ～ 0x1F、\u0081 など）も使用できません。HTTP/1.1 で Unicode 文字列を規定するルールについては、[RFC 2616、セクション 2.2：基本ルール](https://www.ietf.org/rfc/rfc2616.txt)および [RFC 3987](https://www.ietf.org/rfc/rfc3987.txt) を参照してください。
- 次のファイル名は使用できません：LPT1、LPT2、LPT3、LPT4、LPT5、LPT6、LPT7、LPT8、LPT9、COM1、COM2、COM3、COM4、COM5、COM6、COM7、COM8、COM9、PRN、AUX、NUL、CON、CLOCK$、ドット文字（.）、2 つのドット文字（..）。

## 次の手順

このドキュメントでは、[!DNL Acxiom] アカウントからExperience Platformにデータを取り込むために必要な、前提条件の設定を完了しました。 これで、ユーザーインターフェイスを使用した [Experience Platformへの接続  [!DNL Acxiom Data Ingestion]  に関するガイドに進むことができ &#x200B;](../../tutorials/ui/create/data-partners/acxiom-data-ingestion.md) す。
