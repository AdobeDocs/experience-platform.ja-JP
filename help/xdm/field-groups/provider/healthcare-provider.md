---
title: ヘルスケアプロバイダースキーマフィールドグループ
description: ヘルスケアプロバイダーのスキーマフィールドグループについて説明します。
exl-id: e39b4082-4b66-47b3-a8e2-951d8a96f742
source-git-commit: de8e944cfec3b52d25bb02bcfebe57d6a2a35e39
workflow-type: tm+mt
source-wordcount: '254'
ht-degree: 3%

---

# [!UICONTROL &#x200B; ヘルスケア提供組織 &#x200B;] スキーマフィールドグループ

[!UICONTROL Healthcare Provider] は、[[!UICONTROL Provider] クラス &#x200B;](../../classes/provider.md) の標準スキーマフィールドグループです。 診断および治療のヘルスケアサービスを提供するための免許を持つ医療従事者個人または医療施設組織に関連するプロパティを収集する単一のオブジェクトタイプのフィールド `healthcareProvider` ードを提供します。

![](../../images/field-groups/healthcare-provider.png)

| プロパティ | データタイプ | 説明 |
| --- | --- | --- |
| `addressDetails` | オブジェクトの配列 | プロバイダのアドレスの詳細を一覧表示します。 各オブジェクトには、次のプロパティが含まれます。 <ul><li>`address`: （[[!UICONTROL &#x200B; 郵送先住所 &#x200B;]](../../data-types/postal-address.md)）：プロバイダーの郵送先住所。</li><li>`addressType`: （文字列）プロバイダーがサービスを提供する場所を示す、アドレスのタイプ。</li></ul> |
| `emailAddress` | [[!UICONTROL &#x200B; メールアドレス &#x200B;]](../../data-types/email-address.md) | プロバイダーのメールアドレス。 |
| `fax` | [[!UICONTROL &#x200B; 電話番号 &#x200B;]](../../data-types/phone-number.md) | プロバイダの FAX 番号。 |
| `phoneNumber` | [[!UICONTROL &#x200B; 電話番号 &#x200B;]](../../data-types/phone-number.md) | プロバイダーの電話番号。 |
| `qualifications` | オブジェクトの配列 | 介護の提供に関する認定、ライセンス、またはトレーニングを一覧表示します。 各オブジェクトには、次のプロパティが含まれます。 <ul><li>`issuer`:（[[!UICONTROL &#x200B; アカウントの詳細 &#x200B;]](../../data-types/account-details.md)）：選定を規制し、発行する組織。</li><li>`activePeriod`: （整数）検証が有効になるまでの年。</li><li>`code`:（文字列）選定のコード化された表現。</li></ul> |
| `classification` | 文字列 | サービスプロバイダーのクラスまたはカテゴリに基づく分類（患者のケア、患者のいないケアなど）。 |
| `isActive` | ブール値 | プロバイダがアクティブかどうかを示します。 |
| `languages` | 文字列の配列 | プロバイダーが操作を実行する言語のリスト。 |
| `practiceGroupName` | 文字列 | サービスプロバイダーの練習グループ名。 |
| `practiceGroupType` | 文字列 | サービスプロバイダーの練習グループのタイプ。 |
| `practiceType` | 文字列 | サービスプロバイダーの練習タイプ。 |
| `specialties` | 文字列の配列 | このプロバイダーが提供するスペシャルティのリスト。 |

{style="table-layout:auto"}

フィールドグループについて詳しくは、[&#x200B; 公開 XDM リポジトリ &#x200B;](https://github.com/adobe/xdm/blob/master/components/fieldgroups/provider/healthcare-provider-details.schema.json) を参照してください。
