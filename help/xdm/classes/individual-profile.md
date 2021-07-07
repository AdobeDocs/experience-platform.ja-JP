---
keywords: Experience Platform;home;popular topics;schema;Schema;XDM;individual profile;fields;schemas;Schemas;identityMap;identity map;Identity map;Schema design;map;Map;union schema;union
solution: Experience Platform
title: XDM Individual Profile Class
topic-legacy: overview
description: このドキュメントでは、XDM Individual Profileクラスの概要を説明します。
exl-id: 83b22462-79ce-4024-aa50-a9bd800c0f81
source-git-commit: 79fcc44ec5e08f63bfd5eed6e90d7538273f4dab
workflow-type: tm+mt
source-wordcount: '569'
ht-degree: 1%

---

# [!DNL XDM Individual Profile] class

[!DNL XDM Individual Profile] is a standard Experience Data Model (XDM) class which forms a singular representation (or &quot;profile&quot;) of an individual person. 特に、クラス（およびその互換性のあるフィールドグループ）は、ブランドとやり取りする、特定された個人と部分的に識別された個人の両方の属性と関心を取り込みます。

プロファイルには、匿名の行動シグナル（ブラウザーのCookieなど）から、名前、生年月日、場所、Eメールアドレスなどの詳細情報を含む、高度に識別されたプロファイルまで多岐にわたります。 As a profile grows, it becomes a robust repository of personal information, identities, contact details, and communication preferences for an individual. プラットフォームエコシステムでのこのクラスの使用に関する概要については、[XDMの概要](../home.md#data-behaviors)を参照してください。

[!DNL XDM Individual Profile]クラス自体は、データの取得時に自動的に入力される、システム生成値を複数提供します。一方、他のすべてのフィールドは、[互換性のあるスキーマフィールドグループ](#field-groups)を使用して追加する必要があります。

![](../images/classes/individual-profile.png)

| プロパティ | 説明 |
| --- | --- |
| `_repo` | 次の[!UICONTROL DateTime]フィールドを含むオブジェクト： <ul><li>`createDate`: The date and time when the resource was created in the data store, such as when data was first ingested.</li><li>`modifyDate`:リソースが最後に変更された日時。</li></ul> |
| `_id` | レコードの一意の文字列識別子。 このフィールドは、個々のレコードの一意性を追跡し、データの重複を防ぎ、ダウンストリームサービスでそのレコードを検索するために使用します。 場合によっては、`_id`を[Universally Unique Identifier(UUID)](https://tools.ietf.org/html/rfc4122)または[Globally Unique Identifier(GUID)](https://docs.microsoft.com/en-us/dotnet/api/system.guid?view=net-5.0)にすることができます。<br><br>ソース接続からデータをストリーミングする場合、またはParquetファイルから直接取り込む場合は、プライマリID、タイムスタンプ、レコードタイプなど、レコードを一意にするフィールドの組み合わせを連結して、この値を生成する必要があります。連結された値は、`uri-reference`形式の文字列である必要があります。つまり、コロン文字は削除する必要があります。 その後、連結された値は、SHA-256または任意の別のアルゴリズムを使用してハッシュ化する必要があります。<br><br>このフィールドは、個人 **に関連するIDではなく、データ自体のレコードを表すという点を区別することが重要です**。個人に関するIDデータは、互換性のあるフィールドグループから提供される[IDフィールド](../schema/composition.md#identity)に置き換える必要があります。 |
| `createdByBatchID` | レコードが作成される原因となった取得済みバッチのID。 |
| `modifiedByBatchID` | レコードを更新した最後に取得したバッチのID。 |
| `personID` | このレコードが関連する個人の一意の識別子。 このフィールドは、[IDフィールド](../schema/composition.md#identity)とも指定されていない限り、必ずしも個人に関連するIDを表すわけではありません。 |
| `repositoryCreatedBy` | レコードを作成したユーザーのID。 |
| `repositoryLastModifiedBy` | レコードを最後に変更したユーザーのID。 |

{style=&quot;table-layout:auto&quot;}

## Compatible field groups {#field-groups}

>[!NOTE]
>
>The names of several field groups have changed. 詳しくは、[フィールドグループ名の更新](../field-groups/name-updates.md)のドキュメントを参照してください。

Adobeは、[!DNL XDM Individual Profile]クラスで使用するいくつかの標準フィールドグループを提供します。 The following is a list of some commonly used field groups for the class:

* [[!UICONTROL Demographic Details]](../field-groups/profile/demographic-details.md)
* [[!UICONTROL IdentityMap]](../field-groups/profile/identitymap.md)
* [[!UICONTROL ロイヤルティの詳細]](../field-groups/profile/loyalty-details.md)
* [[!UICONTROL 個人の連絡先の詳細]](../field-groups/profile/personal-contact-details.md)
* [[!UICONTROL Privacy/Personalization/Marketing Preferences (Consents)]](../field-groups/profile/consents.md)
* [[!UICONTROL Segment Membership Details]](../field-groups/profile/segmentation.md)
* [[!UICONTROL Work Contact Details]](../field-groups/profile/work-contact-details.md)

[!DNL XDM Individual Profile]の互換性のあるすべてのフィールドグループの完全なリストについては、[XDM GitHubリポジトリ](https://github.com/adobe/xdm/tree/master/components/fieldgroups/profile)を参照してください。
