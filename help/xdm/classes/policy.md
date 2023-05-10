---
title: ポリシークラス
description: このドキュメントでは、エクスペリエンスデータモデル (XDM) の Policy クラスの概要を説明します。
exl-id: 56cc8c69-84a0-493e-85c5-e0cd994e4bee
source-git-commit: f5df893260f0772ad54ccdb00d99ed8f328d35a9
workflow-type: tm+mt
source-wordcount: '241'
ht-degree: 10%

---

# [!UICONTROL ポリシー] クラス

エクスペリエンスデータモデル (XDM) では、 [!UICONTROL ポリシー] クラスは、保険証券を定義するプロパティの最小セットをキャプチャします。

![](../images/classes/policy.png)

| プロパティ | データタイプ | 説明 |
| --- | --- | --- |
| `assignedBeneficiary` | の配列 [[!UICONTROL 人物]](../data-types/person.md) データタイプ | ポリシーに割り当てられた受取人（または受取人）をキャプチャします。 |
| `benefitAmount` | [[!UICONTROL 通貨]](../data-types/currency.md) | ポリシー条件に従って支払われる金額。 |
| `location` | [[!UICONTROL 住所]](../data-types/postal-address.md) | 保険証券が発行される場所。 |
| `owner` | [!UICONTROL オブジェクト] | ポリシー所有者のプロファイル情報をキャプチャします。 |
| `owner.faxPhone` | [[!UICONTROL 電話番号]](../data-types/phone-number.md) | 所有者の FAX 電話番号。 |
| `owner.homeAddress` | [[!UICONTROL 住所]](../data-types/postal-address.md) | 所有者の自宅住所。 |
| `owner.homePhone` | [[!UICONTROL 電話番号]](../data-types/phone-number.md) | 所有者の自宅の電話番号。 |
| `owner.mobilePhone` | [[!UICONTROL 電話番号]](../data-types/phone-number.md) | 所有者の携帯電話番号。 |
| `owner.personalEmail` | [[!UICONTROL メールアドレス]](../data-types/email-address.md) | 所有者の個人の電子メールアドレス。 |
| `ID` | [!UICONTROL 文字列] | 保険証券の識別子。 |
| `_id` | [!UICONTROL 文字列] | レコードの、システムで生成される一意の文字列識別子。 このフィールドは、個々のレコードの一意性を追跡し、データの重複を防ぎ、ダウンストリームサービスでそのレコードを検索するために使用します。<br><br>このフィールドはシステムで生成されるので、データの取り込み中に明示的な値は指定されません。 ただし、必要に応じて独自の一意の ID 値を指定することもできます。 |
| `endDate` | [!UICONTROL 日時] | 保険証券の補償範囲が終了（または終了）する日付。 |
| `hasAssignedBeneficiary` | [!UICONTROL ブール値] | ポリシーに受取人が割り当てられているかどうかを示します。 |
| `name` | [!UICONTROL 文字列] | 保険証券の名前。 |
| `startDate` | [!UICONTROL 日時] | 保険契約の適用が開始（または開始）される日付。 |
| `type` | [!UICONTROL 文字列] | 保険証券のタイプ（自宅、自動車、レンター、ボートなど）。 |

{style="table-layout:auto"}
