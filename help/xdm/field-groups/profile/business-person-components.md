---
title: XDM Business Personコンポーネントスキーマフィールドグループ
description: このドキュメントでは、XDMビジネスパーソンコンポーネントスキーマフィールドグループの概要を説明します。
exl-id: 965b89f4-59f5-43f4-8778-3549e15b44d4
source-git-commit: b5cdd72238f7b4519de1c789f4294b9698415327
workflow-type: tm+mt
source-wordcount: '441'
ht-degree: 5%

---

# [!UICONTROL XDMビジネスパーソンコン] ポーネントスキーマフィールドグループ（ベータ版）

>[!IMPORTANT]
>
>このフィールドグループは、現在ベータ版であるリアルタイム顧客データプラットフォームB2Bエディションの一部として利用できます。 ドキュメントと機能は変更される場合があります。

[!UICONTROL XDMビジネスパーソ] ンコンポーネントは、ある個人の複数のソースレコードと、個人のセグメント化に必要なその他の属性を取り込む、クラスの標準スキーマフィールドグループで [[!DNL XDM Individual Profile] ](../../classes/individual-profile.md) す。

リアルタイムCDPのB2B版で[リアルタイム顧客プロファイル](../../../profile/home.md)を使用して人のプロファイルを作成すると、そのプロファイルの作成に使用される情報は、多くのソースレコードから取得される可能性があります。 例えば、ある人が2つの異なる会社で作業する場合、多くのCRMシステムでは、あるコピーがA社に、他のコピーがB社にリンクされるように、その人のコピーを意図的に複製する必要があります。そのデータをAdobe Experience Platformに取り込む場合、このフィールドグループを使用して、異なるソースレコードを単一の表現に結合します。

フィールドグループには、ルートレベルの`personComponents`フィールドが含まれます。これは、オブジェクトの配列です。 配列内の各オブジェクトは、異なるソースレコードを表します。

![](../../images/field-groups/business-person-components.png)

| プロパティ | データタイプ | 説明 |
| --- | --- | --- |
| `sourceAccountKey` | [[!UICONTROL B2Bソース]](../../data-types/b2b-source.md) | 個人に関連付けられたアカウントの複合識別子。 |
| `sourceConvertedContactKey` | [[!UICONTROL B2Bソース]](../../data-types/b2b-source.md) | このリードが変換された場合の、関連する連絡先の複合識別子。 |
| `sourceExternalKey` | [[!UICONTROL B2Bソース]](../../data-types/b2b-source.md) | 個人のデータの元となるソース・システムの複合識別子。 |
| `sourcePersonKey` | [[!UICONTROL B2Bソース]](../../data-types/b2b-source.md) | 個人の複合識別子。 |
| `workEmail` | [[!UICONTROL 電子メールアドレス]](../../data-types/b2b-source.md) | 個人の勤務先の電子メールID。 |
| `personGroupID` | 文字列 | 個人のグループID。 |
| `personScore` | 文字列 | CRMシステムによって個人に生成されたスコア。 |
| `personSource` | 文字列 | 個人のデータの送信元のソース・システムの一意の文字列ベースの識別子。 |
| `personStatus` | 文字列 | 個人の現在のマーケティングまたは販売ステータス。 |
| `personType` | 文字列 | B2Bコンテキスト内の人のタイプ。 |
| `sourceAccountID` | 文字列 | 個人に関連付けられたソースシステム内のアカウントの一意の文字列ベースの識別子。 このフィールドは、この人が勤める様々な会社を検索するための外部キーとしてシステムで使用されます。 |
| `sourceConvertedContactID` | 文字列 | このリードが変換された場合の、関連する連絡先の一意の文字列ベースの識別子。 |
| `sourceExternalID` | 文字列 | 個人のデータの元となるソース・システムの一意の文字列ベースの識別子。 |
| `sourcePersonID` | 文字列 | 個人の一意の文字列ベースの識別子。 |

{style=&quot;table-layout:auto&quot;}

フィールドグループについて詳しくは、パブリックXDMリポジトリを参照してください。

* [入力済みの例](https://github.com/adobe/xdm/blob/master/components/fieldgroups/profile/b2b-person-components.example.1.json)
* [フルスキーマ](https://github.com/adobe/xdm/blob/master/components/fieldgroups/profile/b2b-person-components.schema.json)
