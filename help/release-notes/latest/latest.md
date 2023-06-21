---
title: Adobe Experience Platform リリースノート
description: Adobe Experience Platform の 2023年6月 のリリースノート。
exl-id: f854f9e5-71be-4d56-a598-cfeb036716cb
source-git-commit: b9d78cd726430b0c7690fdb814d0888aaad832f6
workflow-type: tm+mt
source-wordcount: '768'
ht-degree: 24%

---

# Adobe Experience Platform リリースノート

**リリース日：2023年6月21日（PT）**

Adobe Experience Platform の既存の機能に対するアップデート：

- [データ収集](#data-collection)
- [クエリサービス](#query-service)
- [ソース](#sources)

## データ収集 {#data-collection}

Adobe Experience Platform では、クライアントサイドのカスタマーエクスペリエンスデータを収集し、Adobe Experience Platform Edge Network に送信できます。そこでデータを補強して変換し、アドビまたはアドビ以外の宛先に配信できます。

**新機能または更新された機能**

| タイプ | 機能 | 説明 |
| --- | --- | --- |
| 拡張機能 | [!DNL Google Cloud Platform] イベント転送拡張機能 | この [[!DNL Google Cloud Platform]](../../tags/extensions/server/google-cloud-platform/overview.md) イベント転送拡張機能を使用すると、イベントデータをGoogleに転送し、 [!DNL Google Pub/Sub]. |
| 拡張機能 | [!DNL Cloud connector for Google Analytics 4 (ga4)] 拡張機能 | この [[!DNL Cloud connector for Google Analytics 4 (ga4)]](https://partners.adobe.com/exchangeprogram/experiencecloud/exchange.details.109820.html) イベント転送拡張機能を使用すると、新しい [!DNL Google Analytics 4 (ga4)] 標準。 |
| 秘密鍵 | OAuth 2 JWT 秘密鍵 | この [OAuth 2 JWT 秘密鍵](../../tags/ui/event-forwarding/secrets.md) では、Adobeと [!DNL Google] イベント転送でのサーバーとサーバーのやり取りをサポートするサービストークン。 |
| タグとイベントの転送 | ユーザー属性 | ユーザーアトリビューションは、 [タグ](../../tags/home.md) および [イベント転送](../../tags/ui/event-forwarding/overview.md) UI<br><br>データには、誰が何を変更し、何時に変更を加えたかが含まれます。 このデータは、次の画面のタグとイベント転送 UI に表示されます。<br><ul><li> プロパティの概要</li><li> インストール済みの拡張機能</li><li>ルールの比較と確認</li><li>データ要素の比較レビュー</li><li>拡張機能の比較レビュー</li><li>ライブラリリソースの変更</li><li>ライブラリの最終ビルドと公開</li></ul> |

{style="table-layout:auto"}

データ収集について詳しくは、 [データ収集の概要](../../tags/home.md).

## クエリサービス {#query-service}

クエリサービスを使用すると、標準 SQL を使用して、Adobe Experience Platformデータレイクのデータに対してクエリを実行できます。 データレイクの任意のデータセットを結合し、クエリ結果を新しいデータセットとして取り込んで、レポートや Data Science Workspace で使用したり、リアルタイム顧客プロファイルに取り込んだりできます。&#x200B;
**更新された機能**
&#x200B; |機能 |説明 | | — | — | |インラインテ&#x200B;ンプレート |クエリサービスで、SQL 内の他のテンプレートを参照するテンプレートの使用がサポートされるようになりました。 クエリでインラインテンプレートを活用することで、ワークロードを削減し、エラーを回避します。 SQL をより柔軟に使用できるよう、ステートメントや条件を再利用し、ネストされたテンプレートを参照できます。 テンプレートとして保存できるクエリのサイズ、または元のクエリから参照できるテンプレートの数に制限はありません。 詳しくは、 [インラインテンプレートガイド](../../query-service/essential-concepts/inline-templates.md). | |スケジュールされたクエリ UI の更新 | [[!UICONTROL 「予定クエリ」タブ]](../../query-service/ui/monitor-queries.md#inline-actions). この [!UICONTROL 予定クエリ] インラインクエリアクションと新しいクエリステータス列が追加され、UI が改善されました。 最近の追加機能には、スケジュールを有効、無効、削除する機能や、今後のクエリ実行に関するアラートを [!UICONTROL 予定クエリ] 表示 <p>![インラインアクションで [!UICONTROL 予定クエリ] 表示](../../query-service/images/ui/monitor-queries/disable-inline.png "インラインアクションで [!UICONTROL 予定クエリ] 表示"){width="100" zoomable="yes"}</p> |

{style="table-layout:auto"}
ク&#x200B;エリサービスについて詳しくは、 [クエリサービスの概要](../../query-service/home.md).

## ソース {#sources}

Adobe Experience Platform では、外部ソースからデータを取り込むことができ、Platform サービスを使用してそのデータの構造化、ラベル付け、および拡張を行うことができます。Adobeアプリケーション、クラウドベースのストレージ、サードパーティのソフトウェア、CRM システムなど、様々なソースからデータを取り込むことができます。

Experience Platform は、様々なデータプロバイダーのソース接続を簡単に設定できる RESTful API とインタラクティブ UI を備えています。これらのソース接続を使用すると、外部ストレージシステムおよび CRM サービスの認証と接続、取得実行時間の設定、データ取得スループットの管理を行うことができます。

**更新された機能**

| 機能 | 説明 |
| --- | --- |
| Adobe Analytics分類ソースのデータフロー削除のサポート | Adobe Analytics分類をソースとして使用するソースデータフローを削除できるようになりました。 の下 **[!UICONTROL ソース]** > **[!UICONTROL データフロー]**」、目的のデータフローを選択し、「削除」を選択します。 詳しくは、 [Adobe Analytics分類データのソース接続の作成](../../sources/tutorials/ui/create/adobe-applications/classifications.md). |
| のフィルターサポート [!DNL Microsoft Dynamics] API の使用 | 論理演算子と比較演算子を使用して、 [[!DNL Microsoft Dynamics]](../../sources/connectors/crm/ms-dynamics.md) ソース。 詳しくは、 [API を使用したソースのデータのフィルタリング](../../sources/tutorials/api/filter.md). |
| [!BADGE ベータ]{type=Informative}[!DNL RainFocus] | これで、 [!DNL RainFocus] ソースの統合により、 [!DNL RainFocus] アカウントからExperience Platformへ。 詳しくは、 [[!DNL RainFocus] ソースの概要](../../sources/connectors/analytics/rainfocus.md). |
| Adobe Commerce のサポート | Adobe Commerceソース統合を使用して、Adobe CommerceアカウントからExperience Platformにデータを取り込めるようになりました。 詳しくは、 [Adobe Commerceソースの概要](../../sources/connectors/adobe-applications/commerce.md). |
| [!DNL Mixpanel] のサポート | これで、 [!DNL Mixpanel] ソースの統合により、分析データを [!DNL Mixpanel] API またはユーザーインターフェイスを使用してExperience Platformにアカウントする。 詳しくは、 [[!DNL Mixpanel] ソースの概要](../../sources/connectors/analytics/mixpanel.md). |
| [!DNL Zendesk] のサポート | これで、 [!DNL Zendesk] ソース統合により、 [!DNL Zendesk] API またはユーザーインターフェイスを使用してExperience Platformにアカウントする。 詳しくは、 [[!DNL Zendesk] ソースの概要](../../sources/connectors/customer-success/zendesk.md). |

{style="table-layout:auto"}

ソースについて詳しくは、[ソースの概要](../../sources/home.md)を参照してください。
