---
title: Adobe Experience Platform リリースノート
description: Adobe Experience Platform の 2023年6月 のリリースノート。
exl-id: f854f9e5-71be-4d56-a598-cfeb036716cb
source-git-commit: e56a6c2bac46778afcc24db8d51e77ec3700dd96
workflow-type: tm+mt
source-wordcount: '1606'
ht-degree: 32%

---

# Adobe Experience Platform リリースノート

**リリース日：2023年6月21日（PT）**

Adobe Experience Platform の既存の機能に対するアップデート：

- [Experience PlatformAPI に対する認証](#authentication-platform-apis)
- [データ収集](#data-collection)
- [宛先](#destinations)
- [エクスペリエンスデータモデル（XDM）](#xdm)
- [クエリサービス](#query-service)
- [ソース](#sources)

## Experience PlatformAPI に対する認証 {#authentication-platform-apis}

Experience PlatformAPI ユーザーの場合、認証および API エンドポイントの呼び出しをおこなうために必要なアクセストークンを取得する方法が簡略化されました。 アクセストークンを取得する JWT メソッドは廃止され、よりシンプルな OAuth サーバー間認証方法に置き換えられました。<p>![ハイライト表示されたアクセストークンを取得する新しい OAuth 認証方法。](/help/landing/images/api-authentication/oauth-authentication-method.png "ハイライト表示されたアクセストークンを取得する新しい OAuth 認証方法。"){width="100" zoomable="yes"}</p>

JWT 認証方式を使用した既存の API 統合は 2025 年 1 月 1 日まで引き続き機能しますが、Adobeでは、その日以前に既存の統合を新しい OAuth サーバー間方式に移行することを強くお勧めします。 次のガイドを読む： [サービスアカウント (JWT) 秘密鍵証明書から OAuth サーバー間秘密鍵証明書への移行](https://developer.adobe.com/developer-console/docs/guides/authentication/ServerToServerAuthentication/migration/).

更新された [Experience Platform認証のチュートリアル](/help/landing/api-authentication.md) を参照してください。

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

## 宛先 {#destinations}

[!DNL Destinations] は、Adobe Experience Platform からのデータの円滑なアクティベーションを可能にする、事前定義済みの出力先プラットフォームとの統合です。宛先を使用して、クロスチャネルマーケティングキャンペーン、電子メールキャンペーン、ターゲット広告、その他多くの使用事例に関する既知および不明なデータをアクティブ化できます。

**新規宛先または更新された宛先** {#new-updated-destinations}

| 宛先 | 説明 |
| ----------- | ----------- |
| [[!BADGE Beta]{type=Informative} [!DNL Amazon Ads] 接続](../../destinations/catalog/advertising/amazon-ads.md) | この [!DNL Amazon Ads] Adobe Experience Platformとの統合で、様々な [!DNL Amazon Ads] marketplaces. 詳しくは、 [宛先の変更ログ](../../destinations/catalog/advertising/amazon-ads.md#changelog). |

{style="table-layout:auto"}

**新機能または更新された機能** {#destinations-new-updated-functionality}

| 機能 | 説明 |
| ----------- | ----------- |
| Workspace でののサポート [Adobe Target](../../destinations/catalog/personalization/adobe-target-connection.md) 宛先。 | 新しいAdobe Targetの宛先接続を設定する際に、オーディエンスを共有するAdobe Targetワークスペースを選択できるようになりました。 詳しくは、 [接続パラメータ](../../destinations/catalog/personalization/adobe-target-connection.md#parameters) 」の節を参照してください。 さらに、 [ワークスペースの設定](https://experienceleague.adobe.com/docs/target-learn/tutorials/administration/set-up-workspaces.html?lang=en) Adobe Targetでを参照してください。 |

{style="table-layout:auto"}

<!--

**Fixes and enhancements** {#destinations-fixes-and-enhancements}

- Placeholder for fixes and enhancements

-->

宛先の一般的な情報については、[宛先の概要](../../destinations/home.md)を参照してください。

## エクスペリエンスデータモデル（XDM） {#xdm}

XDM は、Adobe Experience Platform に取り込むデータの共通構造および定義（スキーマ）を提供するオープンソース仕様です。XDM 標準規格に準拠しているので、すべての顧客体験データを共通の表現に反映させて、迅速かつ統合的な方法でインサイトを提供できます。顧客行動から有益なインサイトを得たり、セグメントを通じて顧客オーディエンスを定義したり、パーソナライゼーションのために顧客属性を使用したりできます。

**新しい XDM コンポーネント**

| コンポーネントのタイプ | 名前 | 説明 |
| --- | --- | --- |
| 拡張機能 (Prospect-Profile) | [[!UICONTROL Adobe統合プロファイルサービス見込み客 — プロファイル和集合拡張機能]](https://github.com/adobe/xdm/pull/1735/files) | 見込み客とプロファイルの和集合スキーマに必須フィールドを追加しました。 |
| 拡張機能 | [[!UICONTROL アセットの判定]](https://github.com/adobe/xdm/pull/1732/files) | 判定で使用されるアセットを表すデータ型を追加します。 [!UICONTROL アセットの判定] は、 `decisionItems`. |
| データタイプ | [[!UICONTROL コマース]](https://github.com/adobe/xdm/pull/1747/files) | [!UICONTROL コマース] は、購入および販売活動に関連するレコードを保存します。 |
| フィールドグループ | [[!UICONTROL プロファイルパートナーエンリッチメント（サンプル）]](https://github.com/adobe/xdm/pull/1747/files) | プロファイルパートナーエンリッチメント用にサンプルスキーマが追加されました。 |
| フィールドグループ | [[!UICONTROL パートナー見込み客の詳細（例）]](https://github.com/adobe/xdm/pull/1747/files) | データベンダー見込み客プロファイル拡張のサンプルスキーマが追加されました。 |
| データタイプ | [[!UICONTROL コマース範囲]](https://github.com/adobe/xdm/pull/1747/files) | [!UICONTROL コマース範囲] イベントが発生した場所を示します。 例えば、ストア表示、ストア、Web サイトなどでは、 |
| データタイプ | [[!UICONTROL 課金]](https://github.com/adobe/xdm/pull/1734/files) | 1 つ以上の支払いに関する請求情報が [!UICONTROL コマース] スキーマ。 |

{style="table-layout:auto"}

**更新された XDM コンポーネント**

| コンポーネントのタイプ | 名前 | 説明のアップデート |
| --- | --- | --- |
| フィールドグループ | [[!UICONTROL Media Analytics インタラクションの詳細]](https://github.com/adobe/xdm/pull/1736/files) | 変更済み `bitrateAverageBucket` 100 から「800-899」まで。 |
| データタイプ | [[!UICONTROL QoE データの詳細情報]](https://github.com/adobe/xdm/pull/1736/files) | 変更済み `bitrateAverageBucket` データ型を文字列に変換します。 |
| フィールドグループ | [[!UICONTROL セグメントメンバーシップの詳細]](https://github.com/adobe/xdm/pull/1735/files) | 見込み客プロファイルクラスに追加されました。 |
| スキーマ | [[!UICONTROL 計算属性システムスキーマ]](https://github.com/adobe/xdm/pull/1735/files) | ID マップが [!UICONTROL 計算済み属性システムスキーマ]. |
| データタイプ | [[!UICONTROL コンテンツ配信ネットワーク]](https://github.com/adobe/xdm/pull/1733/files) | フィールドの追加先 [!UICONTROL セッションの詳細情報] を参照して、使用するコンテンツ配信ネットワークを説明します。 |
| 拡張機能 | [[!UICONTROL Adobe 統合プロファイルサービスアカウント和集合拡張機能]](https://github.com/adobe/xdm/pull/1731/files) | ID マップが [!UICONTROL Adobe統合プロファイルサービスアカウント和集合拡張機能]. |
| データタイプ | [[!UICONTROL Order]](https://github.com/adobe/xdm/pull/1730/files) | `discountAmount` が [!UICONTROL 注文]. これは、通常の注文価格と特別価格の違いを表します。 個々の製品ではなく、注文全体に適用されます。 |
| スキーマ | [[!UICONTROL AEP 衛生操作リクエスト]](https://github.com/adobe/xdm/pull/1728/files) | この `targetServices` データの衛生操作を処理するサービスの名前を提供するためのフィールドが追加されました。 |
| データタイプ | [[!UICONTROL 送料]](https://github.com/adobe/xdm/pull/1727/files) | `currencyCode` が 1 つ以上の製品の配送先情報に追加されました。 製品の価格設定に使用される ISO 4217 アルファベットの通貨コードです。 |
| データタイプ | [[!UICONTROL アプリケーション]](https://github.com/adobe/xdm/pull/1726/files) | この `language` フィールドが追加され、ユーザーの言語的、地理的または文化的な好みがアプリケーションに提供されました。 |
| 拡張機能 | [[!UICONTROL AJO エンティティフィールド]](https://github.com/adobe/xdm/pull/1746/files) | [!UICONTROL AJO タイムスタンプエンティティ] が追加され、メッセージが最後に変更された日時を示します。 |
| データタイプ | （複数） | [複数のメディアの詳細を削除](https://github.com/adobe/xdm/pull/1739/files) 一貫性を保つために複数のデータタイプにまたがって |

{style="table-layout:auto"}

Platform での XDM について詳しくは、 [XDM システムの概要](../../xdm/home.md)

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
