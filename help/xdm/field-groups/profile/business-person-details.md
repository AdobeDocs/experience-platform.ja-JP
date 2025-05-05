---
title: XDM ビジネス人物詳細スキーマフィールドグループ
description: XDM ビジネスユーザー詳細スキーマフィールドグループについて説明します。
exl-id: e9da5c1c-5a30-4cbc-beb2-cc5efe57cab0
source-git-commit: de8e944cfec3b52d25bb02bcfebe57d6a2a35e39
workflow-type: tm+mt
source-wordcount: '567'
ht-degree: 11%

---

# [!UICONTROL XDM ビジネスユーザー詳細 &#x200B;] スキーマフィールドグループ

[!UICONTROL XDM ビジネスユーザー詳細 &#x200B;] は、[[!DNL XDM Individual Profile]  クラス ](../../classes/individual-profile.md) の標準スキーマフィールドグループで、B2B （Business to Business）エンタープライズのコンテキストでの個人に関する情報をキャプチャします。

![](../../images/field-groups/business-person-details.png)

| プロパティ | データタイプ | 説明 |
| --- | --- | --- |
| `b2b` | オブジェクト | 人物に関する B2B 固有の詳細をキャプチャするオブジェクト。 |
| `b2b.accountKey` | [[!UICONTROL B2B Source]](../../data-types/b2b-source.md) | 人物に関連するビジネスアカウントの複合識別子。 |
| `b2b.convertedContactKey` | [[!UICONTROL B2B Source]](../../data-types/b2b-source.md) | リードが変換された場合の、関連付けられた連絡先の複合識別子。 |
| `b2b.personKey` | [[!UICONTROL B2B Source]](../../data-types/b2b-source.md) | ユーザーまたはプロファイルフラグメントの複合識別子。 |
| `b2b.accountID` | 文字列 | この人物が関連付けられているビジネスアカウントの一意の ID。 |
| `b2b.blockedCause` | 文字列 | その人物がブロックされた場合、このプロパティはその理由を示します。 |
| `b2b.convertedContactID` | 文字列 | リードが正常にコンバージョンされた場合の連絡先 ID。 |
| `b2b.convertedDate` | 日時 | リードが正常にコンバージョンされた場合のコンバージョンの日付。 |
| `b2b.isBlocked` | ブール値 | 人物がブロックされているかどうかを示します。 |
| `b2b.isConverted` | ブール値 | リードが変換されているかどうかを示します。 |
| `b2b.isMarketingSuspended` | ブール値 | その人物のマーケティングが中断されているかどうかを示します。 |
| `b2b.marketingSuspendedCause` | 文字列 | その人物のマーケティングが停止されている場合、このプロパティがその理由となります。 |
| `b2b.personGroupID` | 文字列 | 人物のグループ識別子。 |
| `b2b.personScore` | Double | CRM システムによって人物に対して生成されたスコア。 |
| `b2b.personSource` | 文字列 | 人物の情報を受信したソース。 |
| `b2b.personStatus` | 文字列 | 人物の現在のマーケティングまたは販売ステータス。 |
| `b2b.personType` | 文字列 | B2B 人物のタイプ。 |
| `extSourceSystemAudit` | [ 外部Source システム監査属性 ](../../data-types/external-source-system-audit-attributes.md) | ビジネスパーソンの関係が外部ソースシステムから取得されたものである場合、このオブジェクトはそのシステムの監査属性を取得します。 |
| `extendedWorkDetails` | オブジェクト | 人物に関する追加の作業関連の詳細をキャプチャします。 |
| `extendedWorkDetails.assistantDetails` | オブジェクト | 人物のアシスタントに関する次の属性をキャプチャします。 <ul><li>`name`: （[ 氏名 ](../../data-types/person-name.md)）アシスタントの氏名。</li><li>`phone`: （[ 電話番号 ](../../data-types/phone-number.md)） アシスタントの電話番号。</li></ul> |
| `extendedWorkDetails.departments` | 文字列の配列 | その人物が働いている部門名のリスト。 |
| `extendedWorkDetails.jobTitle` | 文字列 | 人物の役職。 |
| `extendedWorkDetails.photoUrl` | 文字列 | 人物の写真への URL。 |
| `extendedWorkDetails.reportsToID` | 文字列 | 人物のレポートマネージャーの識別子。 |
| `faxPhone` | [ 電話番号 ](../../data-types/phone-number.md) | 人物の FAX 電話番号。 |
| `homeAddress` | [ 郵送先住所 ](../../data-types/postal-address.md) | 人物の自宅の住所。 |
| `homePhone` | [ 電話番号 ](../../data-types/phone-number.md) | 人物の自宅電話番号。 |
| `mobilePhone` | [ 電話番号 ](../../data-types/phone-number.md) | 人物の携帯電話番号。 |
| `otherAddress` | [ 郵送先住所 ](../../data-types/postal-address.md) | 人物の代替住所。 |
| `otherPhone` | [ 電話番号 ](../../data-types/phone-number.md) | 人物の代替の電話番号。 |
| `person` | [ 人物 ](../../data-types/person.md) | 個々のアクター、連絡先、または所有者。 |
| `personalEmail` | [ メールアドレス ](../../data-types/email-address.md) | 人物の個人メールアドレス。 |
| `workAddress` | [ 郵送先住所 ](../../data-types/postal-address.md) | 人物の仕事用アドレス。 |
| `workEmail` | [ メールアドレス ](../../data-types/email-address.md) | 人物の仕事用電子メールアドレス。 |
| `workPhone` | [ 電話番号 ](../../data-types/phone-number.md) | 人物の仕事用電話番号。 |
| `identityMap` | マップ | 人物の名前空間 ID のセットを含む map フィールド。 このフィールドは、ID データが取り込まれると、システムによって自動的に更新されます。 このフィールドを適切に利用するため [ リアルタイム顧客プロファイル ](../../../profile/home.md)、データ操作ではフィールドの内容を手動で更新しようとしないでください。<br /><br />そのユースケースについては、[スキーマ構成の基本](../../schema/composition.md#identityMap) の ID マップの節を参照してください。 |
| `isDeleted` | ブール値 | この人物がMarketo Engageで削除されたかどうかを示します。<br><br>[Marketo ソースコネクタを使用 ](../../../sources/connectors/adobe-applications/marketo/marketo.md) すると、Marketoで削除されたすべてのレコードがリアルタイム顧客プロファイルに自動的に反映されます。 ただし、これらのプロファイルに関連するレコードは、引き続きデータレイクに保持される場合があります。 `isDeleted` を `true` に設定すると、フィールドを使用して、データレイクのクエリ時にソースから削除されたレコードをフィルターで除外できます。 |
| `organizations` | 文字列の配列 | 人物が働いている組織名のリスト。 |

{style="table-layout:auto"}

フィールドグループについて詳しくは、公開 XDM リポジトリを参照してください。

* [ 入力された例 ](https://github.com/adobe/xdm/blob/master/components/fieldgroups/profile/b2b-person-details.example.1.json)
* [ 完全なスキーマ ](https://github.com/adobe/xdm/blob/master/components/fieldgroups/profile/b2b-person-details.schema.json)
