---
title: 仮想サービスの詳細データ タイプ
description: 仮想サービス詳細エクスペリエンスデータモデル（XDM）データタイプについて説明します。
badgePrivateBeta: label="Private Beta" type="Informative"
hide: true
hidefromtoc: true
exl-id: bde7363c-43b7-402d-96b2-7aa0160cd2ea
source-git-commit: 3071d16b6b98040ea3f2e3a34efffae517253b8e
workflow-type: tm+mt
source-wordcount: '206'
ht-degree: 5%

---

# [!UICONTROL &#x200B; 仮想サービスの詳細 &#x200B;] データタイプ

[!UICONTROL &#x200B; 仮想サービスの詳細 &#x200B;] は、仮想サービスの連絡先の詳細を説明する標準の Experience Data Model （XDM）データタイプです。 このデータタイプは、HL7 FHIR リリース 5 の仕様に従って作成されます。

![&#x200B; 仮想サービスの詳細データタイプ構造 &#x200B;](../../../images/healthcare/data-types/virtual-service-detail.png)

| 表示名 | プロパティ | データタイプ | 説明 |
| --- | --- | --- | --- |
| [!UICONTROL &#x200B; 連絡先の住所 &#x200B;] | `addressContactPoint` | [[!UICONTROL &#x200B; 連絡窓口 &#x200B;]](../data-types/contact-point.md) | 電話、FAX、電子メールなど、テクノロジーを介した連絡先の詳細。 |
| [!UICONTROL &#x200B; アドレス拡張連絡先詳細 &#x200B;] | `addressExtendedContactDetail` | [[!UICONTROL &#x200B; 詳細な連絡先 &#x200B;]](../data-types/extended-contact-detail.md) | 詳細な連絡先情報。 |
| [!UICONTROL &#x200B; チャネルタイプ &#x200B;] | `channelType` | [[!UICONTROL &#x200B; コーディング &#x200B;]](../data-types/coding.md) | 接続先の仮想サービスのタイプ （Teams、Zoom、WhatsApp など）。 |
| [!UICONTROL &#x200B; 追加情報 &#x200B;] | `additionalInfo` | 文字列の配列 | 代替接続の詳細を確認するためのアドレス。URI で表されます。 |
| [!UICONTROL &#x200B; アドレス文字列 &#x200B;] | `addressString` | 文字列 | 仮想サービスへの接続に使用されるアドレス。 |
| [!UICONTROL &#x200B; アドレス Url] | `addressUrl` | 文字列 | 仮想サービスへの接続に使用する URL。URI で表されます。 |
| [!UICONTROL &#x200B; 最大参加者数 &#x200B;] | `maxParticipants` | 整数 | サポートされる参加者の最大数（最小値は `0`）。 |
| [!UICONTROL &#x200B; セッションキー &#x200B;] | `sessionKey` | 文字列 | 仮想サービスに必要なセッションキー。 |

データタイプについて詳しくは、公開 XDM リポジトリを参照してください。

* [&#x200B; 入力された例 &#x200B;](https://github.com/adobe/xdm/blob/master/extensions/industry/healthcare/fhir/datatypes/simplequantity.example.1.json)
* [&#x200B; 完全なスキーマ &#x200B;](https://github.com/adobe/xdm/blob/master/extensions/industry/healthcare/fhir/datatypes/simplequantity.schema.json)
