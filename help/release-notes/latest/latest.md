---
title: Adobe Experience Platform リリースノート
description: Adobe Experience Platform の 2023年7月のリリースノート。
exl-id: f854f9e5-71be-4d56-a598-cfeb036716cb
source-git-commit: 134c18822350a0032bb9957e6e0d1ab888c6b289
workflow-type: tm+mt
source-wordcount: '659'
ht-degree: 35%

---

# Adobe Experience Platform リリースノート

**リリース日：2023年7月26日（PT）**

Adobe Experience Platform の既存の機能に対するアップデート：

- [データ収集](#data-collection)
- [データ準備](#data-prep)
- [セグメント化サービス](#segmentation)
- [ソース](#sources)

## データ収集 {#data-collection}

Adobe Experience Platform では、クライアントサイドのカスタマーエクスペリエンスデータを収集し、Adobe Experience Platform Edge Network に送信できます。そこでデータを補強して変換し、アドビまたはアドビ以外の宛先に配信できます。

**新機能または更新された機能**

| タイプ | 機能 | 説明 |
| --- | --- | --- |
| タグとイベント転送 | データ収集監査ログ | アクションが実行された日時、およびタグとイベントの転送をまたいでこのアクションを実行したユーザーを確認できるようになりました。 これにより、製品のトラブルシューティング、適切なガバナンス、内部監査アクティビティを容易におこなえます。 この監査データは、クイックアクションやリソースステータスの更新を含む、コンテキスト内のスライドアウトメニューから表示されます。 このデータは、次の画面のタグとイベント転送 UI にわたって表示されます。<br><ul><li>[プロパティの概要](../../tags/ui/event-forwarding/overview.md#properties)</li><li>[ルール](../../tags/ui/event-forwarding/overview.md#rules)</li><li>[データ要素](../../tags/ui/event-forwarding/overview.md#data-elements)</li><li>[拡張機能](../../tags/ui/event-forwarding/overview.md#extensions)</li><li>[ライブラリレビュー](https://experienceleague.adobe.com/docs/platform-learn/data-collection/tags/build-and-publish-a-library.html)</li><li>[ライブラリの最終ビルドと公開](https://experienceleague.adobe.com/docs/platform-learn/data-collection/tags/build-and-publish-a-library.html)</li></ul> |
| データストリーム | [地域ルックアップ](../../datastreams/configure.md#advanced-options) | データストリームの位置情報とネットワーク検索を設定して、次のような情報を含めることができるようになりました。 <ul><li>国</li><li>郵便番号</li><li>都道府県</li><li>DMA</li><li>市区町村</li><li>緯度 </li><li>経度</li><li>通信事業者</li><li>ドメイン</li><li>ISP</li></ul> お客様は、適用される法令に基づき、正確な位置情報を含む個人データの収集、処理、送信に必要なすべての権限、同意、明確性、および承認を取得したことを確認する責任を負います。 <br> IP アドレスの難読化の選択は、IP アドレスから派生し、設定したAdobeソリューションに送信される位置情報のレベルには影響しません。 位置情報の参照は、別々に制限するか無効にする必要があります。 <br> 詳しくは、 [datastreams に関するドキュメント](../../datastreams/configure.md#advanced-options) を参照してください。 |

{style="table-layout:auto"}

データ収集の詳細については、 [データコレクションの概要](../../tags/home.md).

## データ準備 {#data-prep}

データ準備を使用すると、データエンジニアはエクスペリエンスデータモデル（XDM）との間でデータのマッピング、変換および検証を行うことができます。

**新機能または更新された機能**

| 機能 | 説明 |
| --- | --- |
| 新しいマッパー関数 | Data Prep でオブジェクトをマッピングする際に、次の関数を使用できるようになりました。 <ul><li>`map_get_values`</li><li>`map_has_keys`</li><li>`add_to_map`</li></ul> これらの関数について詳しくは、 [データ準備関数ガイド](../../data-prep/functions.md#hierarchies---objects). |

{style="table-layout:auto"}

データ準備について詳しくは、[データ準備の概要](../../data-prep/home.md)を参照してください。

## セグメント化サービス {#segmentation}

[!DNL Segmentation Service] では、次に保存されているデータをセグメント化できます。 [!DNL Experience Platform] オーディエンスに組み込まれた個人（顧客、見込み客、ユーザー、組織など）に関連する オーディエンスは、セグメント定義や、 [!DNL Real-Time Customer Profile] データ。 これらのオーディエンスは、 [!DNL Platform]を使用し、任意のAdobeソリューションから簡単にアクセスできます。

**新機能または更新された機能**

| 機能 | 説明 |
| ------- | ----------- |
| Audience Portal | Audience Portal は、Adobe Experience Platform内のオーディエンスにアクセスし、作成し、管理するための新しい閲覧操作を提供します。 Audience Portal 内では、Platform で生成されたオーディエンスと外部で生成されたオーディエンスを表示し、フィルタリング、フォルダー、タグを使用して作業効率を向上させ、Platform で生成されたオーディエンスを作成し、CSV ファイルを使用して外部で生成されたオーディエンスをインポートできます。 Audience Portal の詳細については、 [セグメント化サービス UI ガイド](../../segmentation/ui/overview.md). |
| オーディエンス構成 | オーディエンス構成は、様々なアクションを表すために使用されるブロックを使用して、オーディエンスを作成および編集するための使いやすいワークスペースを提供します。 オーディエンスの構成について詳しくは、 [Audience Composition UI ガイド](../../segmentation/ui/audience-composition.md). |

詳しくは、 [!DNL Segmentation Service]を読んでください。 [セグメント化の概要](../../segmentation/home.md).

## ソース {#sources}

Experience Platform は、様々なデータプロバイダーのソース接続を簡単に設定できる RESTful API とインタラクティブ UI を備えています。これらのソース接続を使用すると、外部ストレージシステムおよび CRM サービスの認証と接続、取得実行時間の設定、データ取得スループットの管理を行うことができます。

**新機能または更新された機能**

| 機能 | 説明 |
| --- | --- |
| [!BADGE Beta]{type=Informative}[!DNL SAP Commerce] | これで、 [[!DNL SAP Commerce] ソース](../../sources/connectors/ecommerce/sap-commerce.md) サブスクリプションの請求データを [!DNL SAP Commerce] アカウントからExperience Platformへ。 |
| [!DNL Phoenix] のサポート | これで、 [[!DNL Phoenix] ソース](../../sources/connectors/databases/phoenix.md) データを取り込む [!DNL Phoenix] データベースからExperience Platformへ。 |
| 認証の更新先 [!DNL Salesforce] および [!DNL Salesforce Service Cloud] | これで、 [[!DNL Salesforce]](../../sources/connectors/crm/salesforce.md) および [[!DNL Salesforce Service Cloud]](../../sources/connectors/customer-success/salesforce-service-cloud.md) Experience PlatformUI または [!DNL Flow Service] API. |

{style="table-layout:auto"}

ソースについて詳しくは、[ソースの概要](../../sources/home.md)を参照してください。
