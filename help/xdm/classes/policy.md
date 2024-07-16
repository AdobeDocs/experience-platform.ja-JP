---
title: ポリシークラス
description: エクスペリエンスデータモデル（XDM）のポリシークラスについて説明します。
exl-id: 56cc8c69-84a0-493e-85c5-e0cd994e4bee
source-git-commit: de8e944cfec3b52d25bb02bcfebe57d6a2a35e39
workflow-type: tm+mt
source-wordcount: '237'
ht-degree: 3%

---

# [!UICONTROL Policy] クラス

エクスペリエンスデータモデル（XDM）の [!UICONTROL  ポリシー ] クラスは、保険ポリシーを定義するプロパティの最小セットを取得します。

![](../images/classes/policy.png)

| プロパティ | データタイプ | 説明 |
| --- | --- | --- |
| `assignedBeneficiary` | [[!UICONTROL Person]](../data-types/person.md) データタイプの配列 | ポリシーに割り当てられた受取人（または受取人）をキャプチャします。 |
| `benefitAmount` | [[!UICONTROL 通貨]](../data-types/currency.md) | 保険期間ごとに支払われる金額。 |
| `location` | [[!UICONTROL  郵送先住所 ]](../data-types/postal-address.md) | 保険証券が発行される場所。 |
| `owner` | [!UICONTROL  オブジェクト ] | ポリシー所有者のプロファイル情報をキャプチャします。 |
| `owner.faxPhone` | [[!UICONTROL  電話番号 ]](../data-types/phone-number.md) | 所有者の FAX 電話番号。 |
| `owner.homeAddress` | [[!UICONTROL  郵送先住所 ]](../data-types/postal-address.md) | 所有者の自宅の住所。 |
| `owner.homePhone` | [[!UICONTROL  電話番号 ]](../data-types/phone-number.md) | 所有者の自宅の電話番号。 |
| `owner.mobilePhone` | [[!UICONTROL  電話番号 ]](../data-types/phone-number.md) | 所有者の携帯電話番号。 |
| `owner.personalEmail` | [[!UICONTROL  メールアドレス ]](../data-types/email-address.md) | 所有者の個人メールアドレス。 |
| `ID` | [!UICONTROL 文字列] | 保険契約の識別子。 |
| `_id` | [!UICONTROL 文字列] | レコードに対してシステムで生成された一意の文字列識別子。 このフィールドは、個々のレコードの一意性を追跡、データの重複を防ぎ、ダウンストリームのサービスでそのレコードを検索するために使用されます。<br><br> このフィールドはシステムで生成されるので、データ取り込み時に明示的な値は指定されません。 ただし、必要に応じて、独自の一意の ID 値を指定することもできます。 |
| `endDate` | [!UICONTROL  日時 ] | 保険契約が終了（または終了）する日付。 |
| `hasAssignedBeneficiary` | [!UICONTROL  ブール値 ] | ポリシーに受取人が割り当てられているかどうかを示します。 |
| `name` | [!UICONTROL 文字列] | 保険証券の名前。 |
| `startDate` | [!UICONTROL  日時 ] | 保険契約が開始（または開始）する日付。 |
| `type` | [!UICONTROL 文字列] | 保険証券のタイプ （住宅、自動車、レンターズ、ボートなど）。 |

{style="table-layout:auto"}
