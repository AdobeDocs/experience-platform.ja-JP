---
title: XDM 個別見込み客プロファイルクラス
description: このドキュメントでは、エクスペリエンスデータモデル (XDM) の XDM 個別見込み客プロファイルクラスの概要を説明します。
badgeBeta: label="ベータ版" type="Informative"
hide: true
hidefromtoc: true
source-git-commit: 83f01c58e21bc42a53e2e7a08bd7a60ef90b41e6
workflow-type: tm+mt
source-wordcount: '374'
ht-degree: 16%

---

# [!UICONTROL XDM 個別見込み客プロファイル] クラス

>[!IMPORTANT]
>
>The [!UICONTROL XDM 個別見込み客プロファイル] クラスはベータ版で、すべてのユーザーが使用できるわけではありません。 ドキュメントと機能は変更される場合があります。

エクスペリエンスデータモデル (XDM) では、 [!UICONTROL XDM 個別見込み客プロファイル] クラスは、通常、ファネルの上位にある顧客獲得の使用例に関して、データパートナーから提供される見込み客プロファイルをキャプチャします。

![XDM Prospect クラスのスキーマ図。](../images/classes/individual-prospect-profile.png)

| プロパティ | データタイプ | 説明 |
| --- | --- | --- |
| `_repo` | オブジェクト | このクラスを使用すると、データベンダーから提供される見込み客プロファイルを取り込み、顧客獲得の使用例をファネルできます。 |
| `_repo.createDate` | [!UICONTROL 日時] | リソースがリポジトリで作成されたサーバーの日時。 作成時間は、アセットファイルが最初にアップロードされたときや、サーバーによって新しいアセットの親としてディレクトリが作成されたときに設定される場合があります。 datetime プロパティは、ISO 8601 標準に準拠している必要があります。 この形式の例は、「2004-10-23T12」です。:00:00～06:00&quot; |
| `_repo.modifyDate` | [!UICONTROL 日時] | リポジトリでリソースが最後に変更された日時（新しいバージョンのアセットがアップロードされた場合や、ディレクトリの子リソースが追加または削除された場合など）。datetime プロパティは、ISO 8601 標準に準拠している必要があります。 例えば、「2004-10-23T12:00:00-06:00」のような形式があります。 |
| `_id` | [!UICONTROL 文字列] | レコードの、システムで生成される一意の文字列識別子。 このフィールドは、個々のレコードの一意性を追跡し、データの重複を防ぎ、ダウンストリームサービスでそのレコードを検索するために使用します。<br><br>このフィールドはシステムで生成されるので、データの取り込み中に明示的な値は指定されません。 ただし、必要に応じて独自の一意の ID 値を指定することもできます。 |
| `createdByBatchID` | [!UICONTROL 文字列] | レコードが作成される原因となった取得済みバッチの ID。 |
| `modifiedByBatchID` | [!UICONTROL 文字列] | レコードを更新した最後に取得したバッチの ID。 |
| `partnerID` | [!UICONTROL 文字列] | 通常、個々の見込み客を識別する一意の偽名の識別子。 次のドキュメントを参照してください： [ID タイプ](../../identity-service/namespaces.md#identity-types) を参照して、Adobe Experience Platform内で使用できるパートナー ID およびその他の id タイプについて確認してください。 |
| `repositoryCreatedBy` | [!UICONTROL 文字列] | レコードを作成したユーザーの ID。 |
| `repositoryLastModifiedBy` | [!UICONTROL 文字列] | レコードを最後に変更したユーザーの ID。 レコードが作成されると、 `modifiedByUser` の値が `createdByUser` の値です。 |

{style="table-layout:auto"}
