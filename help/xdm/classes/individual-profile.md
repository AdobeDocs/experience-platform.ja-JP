---
keywords: Experience Platform;ホーム;人気のトピック;スキーマ;XDM;個人プロファイル;フィールド;identityMap;ID マップ;スキーマデザイン;マップ;結合スキーマ;和集合
solution: Experience Platform
title: XDM Individual Profile クラス
description: XDM Individual Profile クラスについて説明します。
exl-id: 83b22462-79ce-4024-aa50-a9bd800c0f81
source-git-commit: f129c215ebc5dc169b9a7ef9b3faa3463ab413f3
workflow-type: tm+mt
source-wordcount: '553'
ht-degree: 29%

---

# [!DNL XDM Individual Profile] クラス

[!DNL XDM Individual Profile] は、個人の単一の表現（または「プロファイル」）を形成する標準のエクスペリエンスデータモデル（XDM）クラスです。 特に、クラス（およびその互換性のあるフィールドグループ）は、ブランドとやり取りする、識別された個人と部分的に識別された個人の両方の属性と興味を取得します。

プロファイルは、匿名の行動信号（ブラウザー cookie など）から、名前、生年月日、場所、メールアドレスなどの詳細情報を含む識別されたプロファイルまで様々です。 プロファイルが大きくなると、個々の個人情報、ID、連絡先の詳細、コミュニケーション環境設定の堅牢なリポジトリになります。 Experience Platform エコシステムでのこのクラスの使用に関するハイレベルな情報については、[XDM の概要 ](../home.md#data-behaviors) を参照してください。

![XDM 個人プロファイルクラスのスキーマ図。](../images/classes/individual-profile.png)

| プロパティ | 説明 |
| --- | --- |
| `_repo` | 次の [!UICONTROL DateTime] フィールドを含むオブジェクト。 <ul><li>`createDate`：データが最初に取り込まれた日時など、データストア内でリソースが作成された日時。</li><li>`modifyDate`: リソースが最後に変更された日時。</li></ul> |
| `_id` | レコードの一意の文字列識別子。 このフィールドは、個々のレコードの一意性を追跡、データの重複を防ぎ、ダウンストリームのサービスでそのレコードを検索するために使用されます。 場合によっては、`_id` は、[ユニバーサル固有識別子（UUID）](https://tools.ietf.org/html/rfc4122) または [グローバル固有識別子（GUID）](https://docs.microsoft.com/ja-jp/dotnet/api/system.guid?view=net-5.0)とすることができます。<br><br> ソース接続からデータをストリーミングする場合、または Parquet ファイルから直接取り込む場合は、プライマリ ID、タイムスタンプ、レコードタイプなど、レコードを一意にするフィールドの特定の組み合わせを連結して、この値を生成する必要があります。 連結された値は、`uri-reference` 形式の文字列にする（コロン文字は削除する）必要があります。 その後、連結された値は、SHA-256 または選択した別のアルゴリズムを使用してハッシュ化する必要があります。<br><br>**このフィールドは、個人に関連する ID を表すものではなく**、データ記録そのものを表していることを見極めることが重要です。人物に関する ID データは、代わりに互換性のあるフィールドグループが提供する [ID フィールド](../schema/composition.md#identity)に降格させるべきです。 |
| `createdByBatchID` | レコードを作成する原因となった取り込まれたバッチの ID。 |
| `modifiedByBatchID` | レコードを更新する原因となった、最後に取り込まれたバッチの ID。 |
| `personID` | このレコードに関連する個人の一意の ID。 このフィールドは、[ID フィールド ](../schema/composition.md#identity) としても指定されていない限り、必ずしも人物に関連する ID を表すわけではありません。 |
| `repositoryCreatedBy` | レコードを作成したユーザーの ID。 |
| `repositoryLastModifiedBy` | レコードを最後に変更したユーザーの ID。 |

{style="table-layout:auto"}

## 互換性のあるフィールドグループ {#field-groups}

>[!NOTE]
>
>複数のフィールドグループの名前が変更されました。 詳しくは、[フィールドグループ名の更新](../field-groups/name-updates.md)のドキュメントを参照してください。

アドビでは、 [!DNL XDM Individual Profile] クラスで使用するためのいくつかの標準フィールドグループを提供しています。 このクラスで一般的に使用されるフィールドグループは次のとおりです。

* [[!UICONTROL &#x200B; 同意および環境設定 &#x200B;]](../field-groups/profile/consents.md)
* [[!UICONTROL &#x200B; 人口統計の詳細 &#x200B;]](../field-groups/profile/demographic-details.md)
* [[!UICONTROL IdentityMap]](../field-groups/profile/identitymap.md)
* [[!UICONTROL &#x200B; ロイヤルティの詳細 &#x200B;]](../field-groups/profile/loyalty-details.md)
* [[!UICONTROL &#x200B; 個人の連絡先の詳細 &#x200B;]](../field-groups/profile/personal-contact-details.md)
* [[!UICONTROL &#x200B; セグメントメンバーシップの詳細 &#x200B;]](../field-groups/profile/segmentation.md)
* [[!UICONTROL &#x200B; 電気通信の登録 &#x200B;]](../field-groups/profile/telecom-subscription.md)
* [[!UICONTROL &#x200B; 仕事用連絡先の詳細 &#x200B;]](../field-groups/profile/work-contact-details.md)
* [[!UICONTROL XDM ビジネスユーザーコンポーネント &#x200B;]](../field-groups/profile/business-person-components.md)\*
* [[!UICONTROL XDM ビジネス人物の詳細 &#x200B;]](../field-groups/profile/business-person-details.md)\*

*\*このフィールドグループは、Adobe Real-Time Customer Data PlatformのB2B editionにアクセスできる組織のみが使用できます。*

[!DNL XDM Individual Profile] と互換性のあるすべてのフィールドグループの完全なリストについては、[XDM GitHub リポジトリ ](https://github.com/adobe/xdm/tree/master/components/fieldgroups/profile) を参照してください。
