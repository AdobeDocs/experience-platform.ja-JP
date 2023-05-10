---
title: XDM ビジネス人物コンポーネントスキーマフィールドグループ
description: このドキュメントでは、XDM ビジネスユーザーコンポーネントスキーマフィールドグループの概要を説明します。
exl-id: 965b89f4-59f5-43f4-8778-3549e15b44d4
source-git-commit: 34e0381d40f884cd92157d08385d889b1739845f
workflow-type: tm+mt
source-wordcount: '549'
ht-degree: 6%

---

# [!UICONTROL XDM ビジネス人物コンポーネント] スキーマフィールドグループ

[!UICONTROL XDM ビジネス人物コンポーネント] は、 [[!DNL XDM Individual Profile] クラス](../../classes/individual-profile.md) は、人物の複数のソースレコードと、人物のセグメント化に必要なその他の属性をキャプチャします。

を通じて人物のプロファイルが作成されたとき [リアルタイム顧客プロファイル](../../../profile/home.md) Real-Time CDPの B2B エディションでは、そのプロファイルの作成に使用される情報は、多くのソースレコードから取得される可能性があります。 例えば、ある人が 2 つの異なる会社で作業する場合、多くの CRM システムでは、あるコピーが A 社にリンクされ、他のコピーが B 社にリンクされるように、その人のコピーを意図的に複製します。そのデータをAdobe Experience Platformに取り込む場合、このフィールドグループは、異なるソースレコードを単一の表現に結合するために使用されます。

フィールドグループは、ルートレベルを提供します `personComponents` フィールド。オブジェクトの配列です。 配列内の各オブジェクトは、異なるソースレコードを表します。

>[!IMPORTANT]
>
>取り込みパターンに従う必要があります ( [ソースドキュメント](../../../rtcdp/sources/b2b.md). その他のフィールドマッピングメソッドは、動作を保証するものではありません。
>
>例えば、 `personComponents` 配列は、標準の取り込みパターンで個別に送信され、Platform によって配列に追加されます。 オブジェクトの配列を Business Person コンポーネントに手動で追加すると、エラーが返されます。
>B2B データのスキーマを作成する場合は、自動生成ユーティリティを使用する必要があります。 の使用方法については、ドキュメントを参照してください [B2B 名前空間とスキーマ自動生成ユーティリティ](../../../sources/connectors/adobe-applications/marketo/marketo-namespaces.md). 自動生成ユーティリティを使用せず、データモデルを手動でマッピングする場合は、 [Adobe Real-time Customer Data Platform B2B Edition XDM クラス](../../../rtcdp/schemas/b2b.md) を参照してください。
>
>詳しくは、 [エンドツーエンドのチュートリアル](../../../rtcdp/b2b-tutorial.md) を参照してください。

![](../../images/field-groups/business-person-components.png)

| プロパティ | データタイプ | 説明 |
| --- | --- | --- |
| `sourceAccountKey` | [[!UICONTROL B2B ソース]](../../data-types/b2b-source.md) | 人物に関連付けられたアカウントの複合識別子。 |
| `sourceConvertedContactKey` | [[!UICONTROL B2B ソース]](../../data-types/b2b-source.md) | このリードがコンバートされた場合の、関連する連絡先の複合識別子。 |
| `sourceExternalKey` | [[!UICONTROL B2B ソース]](../../data-types/b2b-source.md) | 個人のデータの元となったソースシステムの複合識別子。 |
| `sourcePersonKey` | [[!UICONTROL B2B ソース]](../../data-types/b2b-source.md) | 人物の複合識別子。 |
| `workEmail` | [[!UICONTROL メールアドレス]](../../data-types/b2b-source.md) | 人物の仕事用電子メール ID。 |
| `personGroupID` | 文字列 | 人物のグループ識別子。 |
| `personScore` | 文字列 | CRM システムによって個人に対して生成されたスコア。 |
| `personSource` | 文字列 | 個人のデータの送信元のソースシステムの一意の文字列ベースの識別子。 |
| `personStatus` | 文字列 | 人物の現在のマーケティングまたは販売のステータス。 |
| `personType` | 文字列 | B2B コンテキスト内の人物のタイプ。 |
| `sourceAccountID` | 文字列 | 人物に関連付けられたソースシステム内のアカウントの一意の文字列ベースの識別子。 このフィールドは、この人が勤める様々な会社を検索するための外部キーとして、システムで使用されます。 |
| `sourceConvertedContactID` | 文字列 | このリードがコンバートされた場合の、関連する連絡先の一意の文字列ベースの識別子。 |
| `sourceExternalID` | 文字列 | 個人のデータの元となるソースシステムの一意の文字列ベースの識別子。 |
| `sourcePersonID` | 文字列 | 人物の一意の文字列ベースの識別子。 |

{style="table-layout:auto"}

フィールドグループについて詳しくは、パブリック XDM リポジトリを参照してください。

* [入力された例](https://github.com/adobe/xdm/blob/master/components/fieldgroups/profile/b2b-person-components.example.1.json)
* [フルスキーマ](https://github.com/adobe/xdm/blob/master/components/fieldgroups/profile/b2b-person-components.schema.json)
