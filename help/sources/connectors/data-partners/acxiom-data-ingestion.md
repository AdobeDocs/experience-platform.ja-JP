---
title: Acxiom データ取り込み
description: 取り込み方法を学ぶ [!DNL Acxiom] データをReal-time Customer Data Platformに送信し、ファーストパーティプロファイルを強化し、オーディエンスを向上させて、マーケティングチャネル全体でアクティブ化します。
badge: ベータ版
source-git-commit: 9419da451616ca7f087ecea7aa66a6c10a474fb3
workflow-type: tm+mt
source-wordcount: '473'
ht-degree: 42%

---

# [!DNL Acxiom Data Ingestion]

>[!NOTE]
>
>[!DNL Acxiom Prospecting Data Import] ソースはベータ版です。詳しくは、 [ソースの概要](../../home.md#terms-and-conditions) ベータラベル付きのソースの使用に関する詳細

以下を使用します。 [!DNL Acxiom Data Ingestion] 取り込むソース [!DNL Acxiom] データをReal-time Customer Data Platformに取り込み、ファーストパーティプロファイルを強化します。 次に、 [!DNL Acxiom] — 強化されたファーストパーティプロファイル：オーディエンスを改善し、マーケティングチャネルをまたいでアクティブ化します。

![acxiom-data-ingestion-workflow](../../images/tutorials/create/acxiom-data-enhancement-import/acxiom-data-ingestion.png)

の設定方法については、以下のドキュメントをお読みください。 [!DNL Acxiom Data Ingestion] ソースアカウント。

## 前提条件 {#prerequisites}

接続するには [!DNL Acxiom Data Ingestion] アカウントからExperience Platformに、次の認証資格情報の値を指定する必要があります。

| 資格情報 | 説明 |
| --- | --- |
| [!DNL Acxiom] 認証キー | 認証キー。 この値は、 [!DNL Acxiom] チーム。 |
| [!DNL Amazon S3] アクセスキー | バケットのアクセスキー ID。 この値は、 [!DNL Acxiom] チーム。 |
| [!DNL Amazon S3] 秘密鍵 | バケットの秘密鍵 ID。 この値は、 [!DNL Acxiom] チーム。 |
| バケット名 | これはファイルが共有されるグループです。 この値は、 [!DNL Acxiom] チーム。 |

## IP アドレス許可リスト

ソースコネクタを操作する前に、IP アドレスのリストを許可リストに追加する必要があります。 地域固有の IP アドレスを許可リストに追加しないと、ソースを使用する際にエラーが発生したり、パフォーマンスが低下する場合があります。 詳しくは、[IP アドレスの許可リスト](../../ip-address-allow-list.md)ページを参照してください。

### Experience Platformの権限の設定

次の両方を持つ必要があります。 **[!UICONTROL ソースを表示]** および **[!UICONTROL ソースの管理]** アカウントに対して有効になっている権限で、 [!DNL Acxiom Data Ingestion] アカウントからExperience Platformへ。 製品管理者に問い合わせて、必要な権限を取得してください。 詳しくは、 [アクセス制御 UI ガイド](../../../access-control/ui/overview.md).

### ファイルとディレクトリの命名制約

クラウドストレージファイルまたはディレクトリに名前を付ける際は、以下の制限事項を考慮に入れる必要があります。

- ディレクトリ名とファイルコンポーネント名は 255 文字を超えてはなりません。
- ディレクトリ名とファイル名の末尾にスラッシュ（`/`）は使用できません。使用した場合、自動的に削除されます。
- 次の予約 URL 文字は、適切にエスケープする必要があります。`! ' ( ) ; @ & = + $ , % # [ ]`
- 次の文字は使用できません。`" \ / : | < > * ?`
- 無効な URL パス文字は使用できません。 `\uE000` のようなコードポイントは、NTFS ファイル名では有効ですが、有効な Unicode 文字ではありません。また、一部の ASCII 文字や Unicode 文字、例えば制御文字（0x00 ～ 0x1F、\u0081 など）も使用できません。HTTP/1.1 で Unicode 文字列を規定するルールについては、[RFC 2616、セクション 2.2：基本ルール](https://www.ietf.org/rfc/rfc2616.txt)および [RFC 3987](https://www.ietf.org/rfc/rfc3987.txt) を参照してください。
- 次のファイル名は使用できません：LPT1、LPT2、LPT3、LPT4、LPT5、LPT6、LPT7、LPT8、LPT9、COM1、COM2、COM3、COM4、COM5、COM6、COM7、COM8、COM9、PRN、AUX、NUL、CON、CLOCK$、ドット文字（.）、2 つのドット文字（..）。

## 次の手順

このドキュメントを読むと、データを [!DNL Acxiom] アカウントからExperience Platformへ。 次のガイドに進むことができます： [接続 [!DNL Acxiom Data Ingestion] ユーザーインターフェイスを使用してExperience Platformを設定するには](../../tutorials/ui/create/data-partners/acxiom-data-ingestion.md).
