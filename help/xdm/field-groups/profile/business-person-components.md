---
title: XDM Business Personコンポーネントスキーマフィールドグループ
description: このドキュメントでは、XDMビジネスパーソンコンポーネントスキーマフィールドグループの概要を説明します。
source-git-commit: 319d508925d22e76a3d75ae473f6ea000b5c655b
workflow-type: tm+mt
source-wordcount: '435'
ht-degree: 4%

---


# [!UICONTROL XDM Business Person Componentsschemaフィー] ルドグループ

>[!NOTE]
>
>このフィールドグループは、リアルタイム顧客データプラットフォーム（リアルタイムCDP）のB2Bエディションにアクセスできる組織でのみ使用できます。

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
