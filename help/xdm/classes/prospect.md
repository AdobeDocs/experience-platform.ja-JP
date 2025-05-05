---
title: XDM 個人見込み客プロファイルクラス
description: エクスペリエンスデータモデル（XDM）の XDM 個人見込み客プロファイルクラスについて説明します。
exl-id: 10fd9d16-4123-4ad4-971f-b715231ee6a9
source-git-commit: f4ddcf14de7a5cec42b5ebc521203cfdd1498a9f
workflow-type: tm+mt
source-wordcount: '390'
ht-degree: 16%

---

# [!UICONTROL XDM 個人見込み客プロファイル &#x200B;] クラス

エクスペリエンスデータモデル（XDM）の [!UICONTROL XDM 個人見込み客プロファイル &#x200B;] クラスは、ファネルの最上位にある顧客獲得のユースケースに対して、データパートナーから通常得られる見込み客プロファイルを取得します。

>[!NOTE]
>
>XDM 個人見込み客プロファイルのフィールドをインデックスとして設定するには、まず 1 つ以上のパートナー ID 名前空間を作成する必要があります。 パートナー ID 名前空間について詳しくは、[ID タイプの節](../../identity-service/features/namespaces.md)を参照してください。

![XDM 見込み客クラスのスキーマ図。](../images/classes/individual-prospect-profile.png)

| プロパティ | データタイプ | 説明 |
| --- | --- | --- |
| `_repo` | オブジェクト | このクラスを使用すると、データベンダーをソースとする見込み客プロファイルを、顧客獲得のユースケースに取り込むことができます。 |
| `_repo.createDate` | [!UICONTROL &#x200B; 日時 &#x200B;] | リポジトリでリソースが作成されたサーバーの日時。 作成時間は、アセットファイルが最初にアップロードされたとき、またはサーバーによって新しいアセットの親としてディレクトリが作成されたときです。 datetime プロパティは、ISO 8601 標準に準拠する必要があります。 この形式の例は、「2004-10-23T12:00:00-06:00」です。 |
| `_repo.modifyDate` | [!UICONTROL &#x200B; 日時 &#x200B;] | リポジトリでリソースが最後に変更された日時（新しいバージョンのアセットがアップロードされた場合や、ディレクトリの子リソースが追加または削除された場合など）。datetime プロパティは、ISO 8601 標準に準拠する必要があります。 例えば、「2004-10-23T12:00:00-06:00」のような形式があります。 |
| `_id` | [!UICONTROL 文字列] | レコードに対してシステムで生成された一意の文字列識別子。 このフィールドは、個々のレコードの一意性を追跡、データの重複を防ぎ、ダウンストリームのサービスでそのレコードを検索するために使用されます。<br><br> このフィールドはシステムで生成されるので、データ取り込み時に明示的な値を提供しません。 ただし、必要に応じて、独自の一意の ID 値を指定することもできます。 |
| `createdByBatchID` | [!UICONTROL 文字列] | レコードを作成する原因となった取り込まれたバッチの ID。 |
| `modifiedByBatchID` | [!UICONTROL 文字列] | レコードを更新する原因となった、最後に取り込まれたバッチの ID。 |
| `partnerID` | [!UICONTROL 文字列] | 通常、個々の見込み客を識別する一意の偽名 ID。 パートナー ID とAdobe Experience Platform内で使用可能な他の ID 型について詳しくは、[ID 型 ](../../identity-service/features/namespaces.md#identity-type) に関するドキュメントを参照してください。 |
| `repositoryCreatedBy` | [!UICONTROL 文字列] | レコードを作成したユーザーの ID。 |
| `repositoryLastModifiedBy` | [!UICONTROL 文字列] | レコードを最後に変更したユーザーの ID。 レコードが作成されると、`modifiedByUser` の値が `createdByUser` の値として設定されます。 |

{style="table-layout:auto"}
