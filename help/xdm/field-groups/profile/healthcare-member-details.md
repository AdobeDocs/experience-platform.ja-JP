---
title: ヘルスケアメンバー詳細スキーマフィールドグループ
description: このドキュメントでは、「ヘルスケアメンバーの詳細」スキーマフィールドグループの概要を説明します。
exl-id: 43ba025e-2acf-4cb7-8487-e6c7c7240867
source-git-commit: 2fd35c4ac29f43391f9dc03c636d20558b701be7
workflow-type: tm+mt
source-wordcount: '655'
ht-degree: 6%

---

# [!UICONTROL 医療メンバーの詳細] スキーマフィールドグループ

[!UICONTROL 医療メンバーの詳細] は、 [[!DNL XDM Individual Profile] クラス](../../classes/individual-profile.md) 医療サービスまたはケアを受ける人物の詳細（連絡先情報、プライマリケア医師、プラン情報など）をキャプチャします。

![フィールドグループ構造](../../images/field-groups/healthcare-member-details/structure.png)

| プロパティ | データタイプ | 説明 |
| --- | --- | --- |
| `billingAddress` | [[!UICONTROL 住所]](../../data-types/postal-address.md) | 人物の請求先住所。 |
| `faxPhone` | [[!UICONTROL 電話番号]](../../data-types/phone-number.md) | 人物の FAX 電話番号。 |
| `homeAddress` | [[!UICONTROL 住所]](../../data-types/postal-address.md) | 人の自宅住所。 |
| `homePhone` | [[!UICONTROL 電話番号]](../../data-types/phone-number.md) | 人の自宅電話番号。 |
| `mailingAddress` | [[!UICONTROL 住所]](../../data-types/postal-address.md) | 人物の住所。 |
| `memberDetails` | オブジェクト | 人物の医療関連の属性と関係に関する詳細な情報を含むオブジェクト。 詳しくは、 [次の款](#memberDetails) を参照してください。 |
| `mobilePhone` | [[!UICONTROL 電話番号]](../../data-types/phone-number.md) | 人物の携帯電話番号。 |
| `person` | [[!UICONTROL 人物]](../../data-types/person.md) | 個人のヘルスケアメンバーシップに関連する個人のアクター、連絡先、または所有者。 |
| `personalEmail` | [[!UICONTROL メールアドレス]](../../data-types/email-address.md) | 人物の個人の電子メールアドレス。 |
| `shippingAddress` | [[!UICONTROL 住所]](../../data-types/postal-address.md) | 人物の配送先住所。 |

{style="table-layout:auto"}

## `memberDetails` {#memberDetails}

`memberDetails` は、人物の医療関連の属性と関係に関する詳細な情報を含むオブジェクトです。 の構造 `memberDetails` 以下に説明します。

![memberDetails 構造](../../images/field-groups/healthcare-member-details/memberDetails.png)

| プロパティ | データタイプ | 説明 |
| --- | --- | --- |
| `emergencyContact` | オブジェクト | 人物の次の緊急連絡先詳細をキャプチャします。 <ul><li>`fullName`:(String) 緊急連絡先のフルネーム。</li><li>`phone`:(String) 緊急連絡先の電話番号。</li><li>`relationshipToMember`:（文字列）緊急連絡先の人物との関係。</li></ul> |
| `medications` | オブジェクトの配列 | 人物に関連する現在および過去の薬の詳細をリストします。 各配列項目は、次の詳細を取り込むオブジェクトです。 <ul><li>`refillLocation`:([[!UICONTROL 郵送先住所]](../../data-types/postal-address.md)) 薬物の補充場所。</li><li>`ID`:（文字列）薬の ID。</li><li>`isCurrent`:(Boolean) 薬剤が最新か過去かを示します。</li><li>`numberOfRefills`:（整数）この薬の提供者が規定したリフィルの数。</li><li>`startDate`:(DateTime) 人が投薬を開始した日付。</li></ul> |
| `multipleBirth` | オブジェクト | 複数の出生に関連する詳細をキャプチャします： <ul><li>`isMultipleBirth`:(Boolean) 人が複数の誕生を与えたかどうかを示します。</li><li>`multipleBirthNumber`:（整数）生まれた赤ちゃんの数。 `isMultipleBirth` が true の場合は除外されます。</li></ul> |
| `plans` | オブジェクトの配列 | 人物に関連する現在および過去の医療計画の詳細をリストします。 各配列項目は、次の詳細を取り込むオブジェクトです。 <ul><li>`coverageEndDate`:(DateTime) 計画カバレッジが終了する日付。</li><li>`coverageStartDate`:(DateTime) 計画カバレッジが開始する日付。</li><li>`isActive`:(Boolean) プランがアクティブかどうかを示します。</li><li>`planId`:(String) プラン ID。</li></ul> |
| `primaryCarePhysicians` | オブジェクトの配列 | 人物に関連するプライマリケア医師の詳細をリストします。 各配列項目は、次の詳細を取り込むオブジェクトです。 <ul><li>`endDate`:(DateTime) プライマリケア医がその人のケアを終了した日付。</li><li>`fullname`:(String) 医師の氏名。</li><li>`providerId`:(String) 医師の一意の識別子。</li><li>`startDate`:(DateTime) プライマリケア医がその人のケアを開始した日付。</li></ul> |
| `specialists` | オブジェクトの配列 | 人物に関連する医療専門家の詳細をリストします。 各配列項目は、次の詳細を取り込むオブジェクトです。 <ul><li>`fullname`:(String) スペシャリストの氏名。</li><li>`providerId`:(String) スペシャリストの一意の識別子。</li><li>`specialty`:（文字列）提供者の専門分野（麻酔科、泌尿器科、放射線科、皮膚科など）。</li></ul> |
| `beneficiaryRelationship` | 文字列 | 個人が扶養家族の場合は、医療メンバーとの受益者関係（例えば、自己、配偶者、子など）。 |
| `billingAccountID` | 文字列 | 人物の請求アカウントの一意の ID。 |
| `dateAgeCollected` | 日時 | 年齢が収集された日付。 |
| `deceasedDate` | 日時 | 死亡した場合の死亡日。 |
| `isDeceased` | ブール値 | 人物が故人かどうかを示します。 |
| `isDependent` | ブール値 | 人物が依存関係であるかどうかを示します。 |
| `nationality` | 文字列 | ISO 3166-1 Alpha-2 コードを使用して表される、人物とその州との法的関係。 |
| `preferredAvailability` | 文字列 | 人物の予定の希望日時。 |
| `primaryMemberID` | 文字列 | 個人が依存している場合のプライマリサブスクライバーの一意の識別子。 |

{style="table-layout:auto"}

フィールドグループについて詳しくは、パブリック XDM リポジトリを参照してください。

* [入力された例](https://github.com/adobe/xdm/blob/master/components/fieldgroups/profile/profile-healthcare-member.example.1.json)
* [フルスキーマ](https://github.com/adobe/xdm/blob/master/components/fieldgroups/profile/profile-healthcare-member.schema.json)

このフィールドグループを一般的なフィールドグループで使用する方法の詳細については、業界スキーマのドキュメントを参照してください [医療業界の使用例](../../schema/industries/healthcare.md).
