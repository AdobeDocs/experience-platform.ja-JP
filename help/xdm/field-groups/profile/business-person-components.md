---
title: XDM ビジネスユーザーコンポーネントスキーマフィールドグループ
description: XDM ビジネスユーザーコンポーネントスキーマフィールドグループについて説明します。
exl-id: 965b89f4-59f5-43f4-8778-3549e15b44d4
source-git-commit: f129c215ebc5dc169b9a7ef9b3faa3463ab413f3
workflow-type: tm+mt
source-wordcount: '515'
ht-degree: 4%

---

# [!UICONTROL XDM ビジネスユーザーコンポーネント &#x200B;] スキーマフィールドグループ

[!UICONTROL XDM ビジネスユーザーコンポーネント &#x200B;] は、[[!DNL XDM Individual Profile]  クラス ](../../classes/individual-profile.md) の標準スキーマフィールドグループで、1 人の人物の複数のソースレコードや、人物のセグメント化に必要なその他の属性をキャプチャするものです。

Real-Time CDPのB2B editionで [ リアルタイム顧客プロファイル ](../../../profile/home.md) を使用して個人のプロファイルを作成すると、そのプロファイルの作成に使用される情報が多数のソースレコードから取得される可能性があります。 例えば、ある人物が 2 つの異なる会社で働いている場合、多くの CRM システムでは、その人物のコピーが意図的に複製されるので、1 つのコピーは会社 A にリンクされ、もう 1 つは会社 B にリンクされます。そのデータをAdobe Experience Platformに取り込む場合、このフィールドグループを使用して、これらの異なるソースレコードを 1 つの表現に結合します。

フィールドグループは、ルートレベルの `personComponents` フィールドを提供します。これは、オブジェクトの配列です。 配列内の各オブジェクトは、異なるソースレコードを表します。

>[!IMPORTANT]
>
>[ ソースのドキュメント ](../../../rtcdp/sources/b2b.md) に記載されている取り込みパターンに従う必要があります。 その他のフィールドマッピングメソッドは、動作を保証するものではありません。
>
>例えば、`personComponents` 配列の各オブジェクトは、標準の取り込みパターン中に個別に送信され、Experience Platformによって配列に追加されます。 オブジェクトの配列を手動でビジネスユーザーコンポーネントに追加すると、エラーが返されます。
>B2B データのスキーマを作成する際は、自動生成ユーティリティを使用する必要があります。 [B2B 名前空間とスキーマ自動生成ユーティリティ ](../../../sources/connectors/adobe-applications/marketo/marketo-namespaces.md) の使用方法については、ドキュメントを参照してください。 自動生成ユーティリティを使用せず、データモデルを手動でマッピングする場合は、データをマッピングする前に、[Adobe Real-Time Customer Data Platform B2B edition XDM クラス ](../../../rtcdp/schemas/b2b.md) に関するドキュメントを必ずお読みください。
>
>B2B データの推奨ワークフローについて詳しくは、[ エンドツーエンドのチュートリアル ](../../../rtcdp/b2b-tutorial.md) を参照してください。

![](../../images/field-groups/business-person-components.png)

| プロパティ | データタイプ | 説明 |
| --- | --- | --- |
| `sourceAccountKey` | [[!UICONTROL B2B Source]](../../data-types/b2b-source.md) | ユーザーに関連付けられたアカウントの複合識別子。 |
| `sourceConvertedContactKey` | [[!UICONTROL B2B Source]](../../data-types/b2b-source.md) | このリードがコンバージョンされた場合の関連連絡先の複合識別子。 |
| `sourceExternalKey` | [[!UICONTROL B2B Source]](../../data-types/b2b-source.md) | ユーザーのデータの元となるソースシステムの複合識別子。 |
| `sourcePersonKey` | [[!UICONTROL B2B Source]](../../data-types/b2b-source.md) | ユーザーの複合識別子。 |
| `workEmail` | [[!UICONTROL &#x200B; メールアドレス &#x200B;]](../../data-types/b2b-source.md) | 人物の仕事用電子メール ID。 |
| `personGroupID` | 文字列 | 人物のグループ識別子。 |
| `personScore` | 文字列 | CRM システムによって人物に対して生成されたスコア。 |
| `personSource` | 文字列 | ユーザーのデータの元となるソースシステムの一意の文字列ベースの識別子。 |
| `personStatus` | 文字列 | 人物の現在のマーケティングまたは販売ステータス。 |
| `personType` | 文字列 | B2B コンテキストにおける人物のタイプ。 |
| `sourceAccountID` | 文字列 | 人物に関連付けられたソースシステム内のアカウントの一意の文字列ベースの識別子。 このフィールドは、この人物が働く様々な会社を検索するために、システムによって外部キーとして使用されます。 |
| `sourceConvertedContactID` | 文字列 | このリードが変換された場合の、関連する連絡先の一意の文字列ベースの ID。 |
| `sourceExternalID` | 文字列 | ユーザーのデータの元となるソースシステムの一意の文字列ベースの識別子。 |
| `sourcePersonID` | 文字列 | 人物の一意の文字列ベースの識別子。 |

{style="table-layout:auto"}

フィールドグループについて詳しくは、公開 XDM リポジトリを参照してください。

* [ 入力された例 ](https://github.com/adobe/xdm/blob/master/components/fieldgroups/profile/b2b-person-components.example.1.json)
* [ 完全なスキーマ ](https://github.com/adobe/xdm/blob/master/components/fieldgroups/profile/b2b-person-components.schema.json)
