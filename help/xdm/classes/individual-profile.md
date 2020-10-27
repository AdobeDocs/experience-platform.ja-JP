---
keywords: Experience Platform;home;popular topics;schema;Schema;XDM;individual profile;fields;schemas;Schemas;identityMap;identity map;Identity map;Schema design;map;Map;union schema;union
solution: Experience Platform
title: XDM個別プロファイルクラス
topic: overview
description: このドキュメントでは、XDM Individualプロファイルクラスの概要を説明します。
translation-type: tm+mt
source-git-commit: f9d8021643e72e3fbb5315b54a19815dcdaaa702
workflow-type: tm+mt
source-wordcount: '363'
ht-degree: 0%

---


# [!DNL XDM Individual Profile] class

[!DNL XDM Individual Profile] は、識別された個人と部分的に識別された個人の両方の属性と利害の単数表現(すなわち「プロファイル」)を形成する標準的なXDMクラスです。

プロファイルには、匿名の行動シグナル（ブラウザーのcookieなど）から、名前、生年月日、場所、電子メールアドレスなどの詳細情報を含む高度に識別されるプロファイルまで様々な種類があります。 プロファイルが増えるにつれて、個人情報、個人情報、連絡先の詳細、コミュニケーションに関する個人の好みの強力なリポジトリとなります。 プラットフォームエコシステムでのこのクラスの使用に関する詳細は、 [XDMの概要を参照してください](../home.md#data-behaviors)。

この [!DNL XDM Individual Profile] クラス自体は、データの取り込み時に自動的に設定されるシステム生成値をいくつか提供しますが、他のすべてのフィールドは、 [互換性のあるミックスインを使用して追加する必要があります](#mixins)。

![](../images/classes/individual-profile.png)

| プロパティ | 説明 |
| --- | --- |
| `_repo` | 次のDateTime [!UICONTROL フィールドを含むオブジェクト] 。 <ul><li>`createDate`:データが最初に取り込まれた日時など、データストアでリソースが作成された日時。</li><li>`modifyDate`:リソースが最後に変更された日時。</li></ul> |
| `_id` | レコードに対する、システム生成の一意の文字列識別子。 このフィールドは、個々のレコードの一意性の追跡、データの重複の防止およびダウンストリームサービスでのそのレコードの検索に使用します。 このフィールドはシステム生成なので、データ取り込み中に明示的な値を指定しないでください。<br><br>このフィールドは、個人に関連するIDではなく、データ **** 自体のレコードを表すものであると区別することが重要です。 個人に関連するIDデータは、 [IDフィールドに左右する必要があります](../schema/composition.md#identity) 。 |
| `createdByBatchID` | レコードを作成した、取り込んだバッチのID。 |
| `modifiedByBatchID` | レコードを更新した最後に取り込んだバッチのID。 |
| `repositoryCreatedBy` | レコードを作成したユーザーのID。 |
| `repositoryLastModifiedBy` | レコードを最後に変更したユーザーのID。 |

## 互換性のあるミックスイン {#mixins}

>[!NOTE]
>
>いくつかのミックスインの名前が変更されました。 詳しくは、 [mixin名の更新に関するドキュメントを参照してください](../mixins/name-updates.md) 。

Adobeは、クラスで使用するいくつかの標準ミックスインを提供し [!DNL XDM Individual Profile] ます。 以下は、このクラスで最も一般的に使用されるミックスインのリストです。

* [[!UICONTROL IdentityMap]](../mixins/profile/identitymap.md)
* [[!UICONTROL 人口統計の詳細]](../mixins/profile/person-details.md)
* [[!UICONTROL 個人の連絡先の詳細]](../mixins/profile/personal-details.md)
* [[!UICONTROL 勤務先担当者の詳細]](../mixins/profile/work-details.md)
* [[!UICONTROL セグメントのメンバーシップの詳細]](../mixins/profile/segmentation.md)