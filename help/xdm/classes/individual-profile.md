---
keywords: Experience Platform；ホーム；人気のあるトピック；スキーマ;スキーマ;XDM；個々のプロファイル；フィールド；スキーマ;スキーマ;identityMap；アイデンティティマップ；スキーマ設計；マップ；和集合スキーマ;和集合
solution: Experience Platform
title: XDM個別プロファイルクラス
topic-legacy: overview
description: このドキュメントでは、XDM Individualプロファイルクラスの概要を説明します。
exl-id: 83b22462-79ce-4024-aa50-a9bd800c0f81
translation-type: tm+mt
source-git-commit: 612917b23d1841556a71f6378497e1d033bc3b62
workflow-type: tm+mt
source-wordcount: '433'
ht-degree: 0%

---

# [!DNL XDM Individual Profile] class

[!DNL XDM Individual Profile] は、識別された個人と部分的に識別された個人の両方の属性と利害の単数表現(すなわち「プロファイル」)を形成する標準的なXDMクラスです。

プロファイルには、匿名の行動シグナル（ブラウザーのcookieなど）から、名前、生年月日、場所、電子メールアドレスなどの詳細情報を含む高度に識別されるプロファイルまで様々な種類があります。 プロファイルが増えるにつれて、個人情報、個人情報、連絡先の詳細、コミュニケーションに関する個人の好みの強力なリポジトリとなります。 プラットフォームエコシステムでのこのクラスの使用に関する詳細は、[XDMの概要](../home.md#data-behaviors)を参照してください。

[!DNL XDM Individual Profile]クラス自体は、データの取り込み時に自動的に設定されるシステム生成値をいくつか提供します。それ以外のすべてのフィールドは、[互換性のあるスキーマフィールドグループ](#field-groups)を使用して追加する必要があります。

![](../images/classes/individual-profile.png)

| プロパティ | 説明 |
| --- | --- |
| `_repo` | 次の[!UICONTROL DateTime]フィールドを含むオブジェクト： <ul><li>`createDate`:データが最初に取り込まれた日時など、データストアでリソースが作成された日時。</li><li>`modifyDate`:リソースが最後に変更された日時。</li></ul> |
| `_id` | レコードの一意の識別子。 このフィールドは、個々のレコードの一意性の追跡、データの重複の防止およびダウンストリームサービスでのそのレコードの検索に使用します。<br><br>このフィールドは、個々の人に関連するアイデンティティ **ではなく、データの記録自体を表しているのではな** いことを区別することが重要です。個人に関するIDデータは、代わりに[IDフィールド](../schema/composition.md#identity)に左右する必要があります。 |
| `createdByBatchID` | レコードを作成した、取り込んだバッチのID。 |
| `modifiedByBatchID` | レコードを更新した最後に取り込んだバッチのID。 |
| `personID` | このレコードが関連付けられる個人の一意の識別子。 [IDフィールド](../schema/composition.md#identity)としても指定されていない限り、このフィールドは必ずしも個人に関連するIDを表すものではありません。 |
| `repositoryCreatedBy` | レコードを作成したユーザーのID。 |
| `repositoryLastModifiedBy` | レコードを最後に変更したユーザーのID。 |

## 互換性のあるフィールドグループ{#field-groups}

>[!NOTE]
>
>複数のフィールドグループの名前が変更されました。 詳しくは、[フィールドグループ名の更新](../field-groups/name-updates.md)のドキュメントを参照してください。

Adobeは、[!DNL XDM Individual Profile]クラスで使用するいくつかの標準フィールドグループを提供します。 次に、このクラスでよく使用されるフィールドグループのリストを示します。

* [[!UICONTROL IdentityMap]](../field-groups/profile/identitymap.md)
* [[!UICONTROL 人口統計の詳細]](../field-groups/profile/demographic-details.md)
* [[!UICONTROL 個人の連絡先の詳細]](../field-groups/profile/personal-contact-details.md)
* [[!UICONTROL 勤務先担当者の詳細]](../field-groups/profile/work-contact-details.md)
* [[!UICONTROL セグメントのメンバーシップの詳細]](../field-groups/profile/segmentation.md)

[!DNL XDM Individual Profile]の互換性のあるすべてのフィールドグループの完全なリストについては、[XDM GitHub repo](https://github.com/adobe/xdm/tree/master/components/mixins/profile)を参照してください。
