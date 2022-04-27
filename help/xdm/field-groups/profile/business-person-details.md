---
title: XDM ビジネス担当者詳細スキーマフィールドグループ
description: このドキュメントでは、「XDM ビジネスユーザー詳細」スキーマフィールドグループの概要を説明します。
exl-id: e9da5c1c-5a30-4cbc-beb2-cc5efe57cab0
source-git-commit: 0084492ed467c5996a94c5c55a79c9faf8f5046e
workflow-type: tm+mt
source-wordcount: '601'
ht-degree: 21%

---

# [!UICONTROL XDM ビジネス人物の詳細] スキーマフィールドグループ

[!UICONTROL XDM ビジネス人物の詳細] は、 [[!DNL XDM Individual Profile] クラス](../../classes/individual-profile.md) B2B(B2B) 企業のコンテキストで個人に関する情報を取得する

![](../../images/field-groups/business-person-details.png)

| プロパティ | データタイプ | 説明 |
| --- | --- | --- |
| `b2b` | オブジェクト | 人物に関する B2B 固有の詳細をキャプチャするオブジェクト。 |
| `b2b.accountKey` | [[!UICONTROL B2B ソース]](../../data-types/b2b-source.md) | 人物に関連するビジネスアカウントの複合識別子。 |
| `b2b.convertedContactKey` | [[!UICONTROL B2B ソース]](../../data-types/b2b-source.md) | リードが変換された場合の、関連する連絡先の複合識別子。 |
| `b2b.personKey` | [[!UICONTROL B2B ソース]](../../data-types/b2b-source.md) | 人物またはプロファイルフラグメントの複合識別子。 |
| `b2b.accountID` | 文字列 | この人物が関連付けられているビジネスアカウントの一意の ID。 |
| `b2b.blockedCause` | 文字列 | ユーザーがブロックされた場合、このプロパティはその理由を示します。 |
| `b2b.convertedContactID` | 文字列 | リードが正常にコンバージョンされた場合の連絡先 ID。 |
| `b2b.convertedDate` | 日時 | リードが正常に変換された場合のコンバージョン日。 |
| `b2b.isBlocked` | ブール値 | 人物がブロックされているかどうかを示します。 |
| `b2b.isConverted` | ブール値 | リードが変換されたかどうかを示します。 |
| `b2b.isMarketingSuspended` | ブール値 | 人物に対してマーケティングが中断されているかどうかを示します。 |
| `b2b.marketingSuspendedCause` | 文字列 | その人物に対してマーケティングが中断されている場合、このプロパティがその理由を示します。 |
| `b2b.personGroupID` | 文字列 | 人物のグループ識別子。 |
| `b2b.personScore` | Double | CRM システムによって個人に対して生成されたスコア。 |
| `b2b.personSource` | 文字列 | 人物の情報を受け取ったソース。 |
| `b2b.personStatus` | 文字列 | 人物の現在のマーケティングまたは販売のステータス。 |
| `b2b.personType` | 文字列 | B2B 人物のタイプ。 |
| `extSourceSystemAudit` | [外部ソースシステム監査属性](../../data-types/external-source-system-audit-attributes.md) | ビジネス人物関係が外部ソースシステムから取得された場合、このオブジェクトはそのシステムの監査属性をキャプチャします。 |
| `extendedWorkDetails` | オブジェクト | 人物に関する作業関連の追加の詳細をキャプチャします。 |
| `extendedWorkDetails.assistantDetails` | オブジェクト | 人物のアシスタントに関連する次の属性をキャプチャします。 <ul><li>`name`:([人物名](../../data-types/person-name.md)) アシスタントの氏名。</li><li>`phone`:([電話番号](../../data-types/phone-number.md)) アシスタントの電話番号。</li></ul> |
| `extendedWorkDetails.departments` | 文字列の配列 | 人物が働く部門名のリスト。 |
| `extendedWorkDetails.jobTitle` | 文字列 | 人物の職位。 |
| `extendedWorkDetails.photoUrl` | 文字列 | 人物の写真への URL。 |
| `extendedWorkDetails.reportsToID` | 文字列 | 人物のレポートマネージャーの識別子。 |
| `faxPhone` | [電話番号](../../data-types/phone-number.md) | 人物の FAX 電話番号。 |
| `homeAddress` | [住所](../../data-types/postal-address.md) | 人の自宅住所。 |
| `homePhone` | [電話番号](../../data-types/phone-number.md) | 人の自宅電話番号。 |
| `mobilePhone` | [電話番号](../../data-types/phone-number.md) | 人物の携帯電話番号。 |
| `otherAddress` | [住所](../../data-types/postal-address.md) | 人物の代替住所。 |
| `otherPhone` | [電話番号](../../data-types/phone-number.md) | 人物の代替電話番号。 |
| `person` | [人物](../../data-types/person.md) | 個々のアクター、連絡先、または所有者。 |
| `personalEmail` | [メールアドレス](../../data-types/email-address.md) | 人物の個人の電子メールアドレス。 |
| `workAddress` | [住所](../../data-types/postal-address.md) | 人物の住所。 |
| `workEmail` | [メールアドレス](../../data-types/email-address.md) | 人物の勤務先の電子メールアドレス。 |
| `workPhone` | [電話番号](../../data-types/phone-number.md) | 人の勤務先の電話番号。 |
| `identityMap` | マップ | 人物の名前空間付き ID のセットを含む map フィールド。 このフィールドは、ID データが取り込まれると、システムによって自動的に更新されます。 このフィールドを適切に利用するため、[リアルタイム顧客プロファイル](../../../profile/home.md)の場合は、データ操作でフィールドの内容を手動で更新しようとしないでください。<br /><br />そのユースケースについては、[スキーマ構成の基本](../../schema/composition.md#identityMap) の ID マップの節を参照してください。 |
| `isDeleted` | ブール値 | この人物がMarketo Engageで削除されたかどうかを示します。<br><br>を使用する場合、 [Marketoソースコネクタ](../../../sources/connectors/adobe-applications/marketo/marketo.md)を指定した場合、Marketoで削除されたレコードは自動的にリアルタイム顧客プロファイルに反映されます。 ただし、これらのプロファイルに関連するレコードは、データレイク内で引き続き保持される場合があります。 設定別 `isDeleted` から `true`の場合は、フィールドを使用して、データレイクに対するクエリを実行する際に、ソースから削除されたレコードを除外できます。 |
| `organizations` | 文字列の配列 | 人物が働く組織名のリスト。 |

{style=&quot;table-layout:auto&quot;}

フィールドグループについて詳しくは、パブリック XDM リポジトリを参照してください。

* [入力された例](https://github.com/adobe/xdm/blob/master/components/fieldgroups/profile/b2b-person-details.example.1.json)
* [フルスキーマ](https://github.com/adobe/xdm/blob/master/components/fieldgroups/profile/b2b-person-details.schema.json)
