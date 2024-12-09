---
title: 仮想サービスの詳細データ タイプ
description: 仮想サービス詳細エクスペリエンスデータモデル（XDM）データタイプについて説明します。
badgePrivateBeta: label="Private Beta" type="Informative"
hide: true
hidefromtoc: true
source-git-commit: 7f7de89a843d867d18ef84051eee69face0570a0
workflow-type: tm+mt
source-wordcount: '206'
ht-degree: 5%

---

# [!UICONTROL  仮想サービスの詳細 ] データタイプ

[!UICONTROL  仮想サービスの詳細 ] は、仮想サービスの連絡先の詳細を説明する標準の Experience Data Model （XDM）データタイプです。 このデータタイプは、HL7 FHIR リリース 5 の仕様に従って作成されます。

![ 仮想サービスの詳細データタイプ構造 ](../../images/data-types/healthcare/virtual-service-detail.png)

| 表示名 | プロパティ | データタイプ | 説明 |
| --- | --- | --- | --- |
| [!UICONTROL  連絡先の住所 ] | `addressContactPoint` | [[!UICONTROL  連絡窓口 ]](../healthcare/contact-point.md) | 電話、FAX、電子メールなど、テクノロジーを介した連絡先の詳細。 |
| [!UICONTROL  アドレス拡張連絡先詳細 ] | `addressExtendedContactDetail` | [[!UICONTROL  詳細な連絡先 ]](../healthcare/extended-contact-detail.md) | 詳細な連絡先情報。 |
| [!UICONTROL  チャネルタイプ ] | `channelType` | [[!UICONTROL  コーディング ]](../healthcare/coding.md) | 接続先の仮想サービスのタイプ （Teams、Zoom、WhatsApp など）。 |
| [!UICONTROL  追加情報 ] | `additionalInfo` | 文字列の配列 | 代替接続の詳細を確認するためのアドレス。URI で表されます。 |
| [!UICONTROL  アドレス文字列 ] | `addressString` | 文字列 | 仮想サービスへの接続に使用されるアドレス。 |
| [!UICONTROL  アドレス Url] | `addressUrl` | 文字列 | 仮想サービスへの接続に使用する URL。URI で表されます。 |
| [!UICONTROL  最大参加者数 ] | `maxParticipants` | 整数 | サポートされる参加者の最大数（最小値は `0`）。 |
| [!UICONTROL  セッションキー ] | `sessionKey` | 文字列 | 仮想サービスに必要なセッションキー。 |

データタイプについて詳しくは、公開 XDM リポジトリを参照してください。

* [ 入力された例 ](https://github.com/adobe/xdm/blob/master/extensions/industry/healthcare/fhir/datatypes/simplequantity.example.1.json)
* [ 完全なスキーマ ](https://github.com/adobe/xdm/blob/master/extensions/industry/healthcare/fhir/datatypes/simplequantity.schema.json)
