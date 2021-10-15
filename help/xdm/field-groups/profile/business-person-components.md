---
title: XDM ビジネス担当者コンポーネントスキーマフィールドグループ
description: このドキュメントでは、XDM ビジネスパーソンコンポーネントスキーマフィールドグループの概要を説明します。
exl-id: 965b89f4-59f5-43f4-8778-3549e15b44d4
source-git-commit: b5cdd72238f7b4519de1c789f4294b9698415327
workflow-type: tm+mt
source-wordcount: '441'
ht-degree: 5%

---

# [!UICONTROL XDM ビジネス担当者コンポー] ネントスキーマフィールドグループ（ベータ版）

>[!IMPORTANT]
>
>このフィールドグループは、現在ベータ版のReal-time Customer Data Platform B2B Edition の一部です。 ドキュメントと機能は変更される場合があります。

[!UICONTROL XDM ビジネス担当者コ] ンポーネントは、ある人の複数のソースレコードと、個人のセグメント化に必要なその他の属性を取り込む、分類の標準スキーマフィールドグループで [[!DNL XDM Individual Profile] ](../../classes/individual-profile.md) す。

リアルタイム CDP の B2B 版で [ リアルタイム顧客プロファイル ](../../../profile/home.md) を使用してユーザーのプロファイルを作成すると、そのプロファイルの作成に使用される情報は、多くのソースレコードから取得される可能性があります。 例えば、ある人が 2 つの異なる会社で働いている場合、多くの CRM システムは意図的にその人のコピーを作成して、1 つのコピーが A 社に、もう 1 つのコピーが B 社にリンクされるようにします。そのデータをAdobe Experience Platformに取り込む場合、このフィールドグループは、異なるソースレコードを単一の表現に結合するために使用されます。

フィールドグループは、ルートレベルの `personComponents` フィールドを提供します。これはオブジェクトの配列です。 配列内の各オブジェクトは、異なるソースレコードを表します。

![](../../images/field-groups/business-person-components.png)

| プロパティ | データタイプ | 説明 |
| --- | --- | --- |
| `sourceAccountKey` | [[!UICONTROL B2B ソース]](../../data-types/b2b-source.md) | 個人に関連付けられた勘定科目の複合識別子。 |
| `sourceConvertedContactKey` | [[!UICONTROL B2B ソース]](../../data-types/b2b-source.md) | この潜在顧客が変換された場合の、関連する連絡先の複合識別子。 |
| `sourceExternalKey` | [[!UICONTROL B2B ソース]](../../data-types/b2b-source.md) | 個人のデータの送信元のソース・システムの複合識別子。 |
| `sourcePersonKey` | [[!UICONTROL B2B ソース]](../../data-types/b2b-source.md) | 個人の複合識別子。 |
| `workEmail` | [[!UICONTROL 電子メールアドレス]](../../data-types/b2b-source.md) | 個人の仕事用電子メール ID。 |
| `personGroupID` | 文字列 | 個人のグループ ID。 |
| `personScore` | 文字列 | CRM システムで個人に生成されたスコア。 |
| `personSource` | 文字列 | 個人のデータの送信元のソース・システムの一意の文字列ベースの識別子。 |
| `personStatus` | 文字列 | 個人の現在のマーケティングまたは販売のステータス。 |
| `personType` | 文字列 | B2B コンテキスト内の個人のタイプ。 |
| `sourceAccountID` | 文字列 | 個人に関連付けられているソースシステム内のアカウントの一意の文字列ベースの識別子。 このフィールドは、この人が勤める様々な会社を検索するための外部キーとして、システムによって使用されます。 |
| `sourceConvertedContactID` | 文字列 | このリードが変換された場合の、関連する連絡先の一意の文字列ベースの識別子。 |
| `sourceExternalID` | 文字列 | 個人のデータの元となったソース・システムの一意の文字列ベースの識別子。 |
| `sourcePersonID` | 文字列 | 個人の一意の文字列ベースの識別子。 |

{style=&quot;table-layout:auto&quot;}

フィールドグループについて詳しくは、パブリック XDM リポジトリを参照してください。

* [入力例](https://github.com/adobe/xdm/blob/master/components/fieldgroups/profile/b2b-person-components.example.1.json)
* [フルスキーマ](https://github.com/adobe/xdm/blob/master/components/fieldgroups/profile/b2b-person-components.schema.json)
