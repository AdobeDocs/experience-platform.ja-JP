---
title: Adobe Experience Platform リリースノート（2026年2月）
description: Adobe Experience Platform の 2026年2月のリリースノートです。
exl-id: f854f9e5-71be-4d56-a598-cfeb036716cb
source-git-commit: a11c00c218ffbbd5618616f401613a604c35859a
workflow-type: tm+mt
source-wordcount: '606'
ht-degree: 33%

---

# Adobe Experience Platform リリースノート

>[!TIP]
>
>その他のAdobe Experience Platform アプリケーションのリリースノートについては、次のドキュメントを参照してください。
>
>- [Adobe Journey Optimizer](https://experienceleague.adobe.com/ja/docs/journey-optimizer/using/whats-new/release-notes)
>- [Adobe Journey Optimizer B2B](https://experienceleague.adobe.com/ja/docs/journey-optimizer-b2b/user/release-notes)
>- [Customer Journey Analytics](https://experienceleague.adobe.com/ja/docs/analytics-platform/using/releases/latest)
>- [連合オーディエンス構成](https://experienceleague.adobe.com/ja/docs/federated-audience-composition/using/release-notes)
>- [Real-Time CDP Collaboration](https://experienceleague.adobe.com/ja/docs/real-time-cdp-collaboration/using/latest)

**リリース日：2026年2月17日（PT）**

Adobe Experience Platformの既存の機能に対する新機能とアップデート：

- [アラート](#alerts)
- [宛先](#destinations)
- [ソース](#sources)
- [エクスペリエンスデータモデル（XDM）](#xdm)

## アラート {#alerts}

Experience Platformでは、様々なExperience Platform アクティビティに関するイベントベースのアラートを登録できます。 Experience Platform ユーザーインターフェイスの「[!UICONTROL Alerts]」タブを使用して、様々なアラートルールを購読し、UI 内またはメール通知を通じてアラートメッセージを受け取るように選択できます。

**新機能または更新された機能**

| 機能 | 説明 |
| --- | --- |
| 顧客向けアラートの [!DNL Slack] 統合 | 顧客向けのアラートを [!DNL Slack] に配信できるようになりました。 [&#x200B; ステップバイステップのチュートリアル &#x200B;](../../observability/alerts/slack-integration.md) に従って [!DNL Slack] 統合を設定し、[!DNL Slack] ワークスペースで直接アラート通知を受け取ります。 |

{style="table-layout:auto"}

詳しくは、[[!DNL Observability Insights] 概要](../../observability/home.md)を参照してください。

## 宛先 {#destinations}

Experience Platformから [!DNL Destinations] データの円滑なアクティベーションを可能にする、事前定義済みの出力先プラットフォームとの統合です。 宛先を使用して、クロスチャネルマーケティングキャンペーン、メールキャンペーン、ターゲット広告、その他多くの使用事例に関する既知および不明なデータをアクティブ化できます。

**新規宛先または更新された宛先**

| 宛先 | 説明 |
| --- | --- |
| [[!DNL Snowflake]  バッチ &#x200B;](../../destinations/catalog/warehouses/snowflake-batch.md) 一般公開 | [!DNL Snowflake] バッチ宛先の一般提供が開始されました。 世界中のReal-Time CDP ユーザーは、このコネクタを使用して、データを物理的にコピーしなくても、Snowflake アカウントに対してデータをアクティブ化できるようになりました。 限定リリースのすべての制限が解除されました（米国のみのお客様に提供され、デフォルトの結合ポリシーに属するオーディエンスにのみ対応）。 |

{style="table-layout:auto"}

**修正点および改善点**

| 修正 | 説明 |
| --- | --- |
| Activation Failed Rate Exceeded アラート | アクティブ化に失敗した割合を超えた宛先アラートで、アラートの評価および送信時に設定したしきい値が正しく使用されるようになりました。 以前は、設定した割合に関係なく、アラートは 1% の失敗率でトリガーされていました。 このアラートの詳細については、[&#x200B; 標準アラートルール &#x200B;](../../observability/alerts/rules.md#destinations) を参照してください。 |
| Google Customer Match で除外された ID レポート | Google Customer Match の宛先に対して、除外されたプロファイル数の水増しが表示される、スキップされたレコードのカウントロジックのバグを修正しました。 アクティベーションと書き出しの動作は影響を受けません。報告された数値のみが正しくありません。 |

{style="table-layout:auto"}

詳しくは、[&#x200B; 宛先の概要 &#x200B;](../../destinations/home.md) を参照してください。

## ソース {#sources}

Experience Platform は、様々なデータプロバイダーのソース接続を簡単に設定できる RESTful API とインタラクティブ UI を備えています。これらのソース接続を使用すると、外部ストレージシステムおよび CRM サービスの認証と接続、取得実行時間の設定、データ取得スループットの管理を行うことができます。

**新規または更新されたソース**

| ソース | 説明 |
| --- | --- |
| ソースコネクタでの Unity カタログ [!DNL Databricks] サポート | [!DNL Databricks] ソースコネクタで Unity カタログがサポートされるようになりました。 ソース接続を設定する際に Unity カタログを使用する方法については、更新された [[!DNL Databricks]](../../sources/connectors/databases/databricks.md) ドキュメントをお読みください。 |

{style="table-layout:auto"}

詳しくは、[ソースの概要](../../sources/home.md)を参照してください。

## エクスペリエンスデータモデル（XDM） {#xdm}

XDM は、Experience Platformに取り込むデータの共通の構造と定義（スキーマ）を提供するオープンソース仕様です。 XDM 標準規格に準拠しているので、すべての顧客体験データを共通の表現に反映させて、迅速かつ統合的な方法でインサイトを提供できます。顧客アクションから有益なインサイトを得たり、セグメントを通じて顧客オーディエンスを定義したり、パーソナライズ機能のために顧客属性を使用したりできます。

| 機能 | 説明 |
| --- | --- |
| データセットを含むスキーマの制限付き編集 | スキーマにデータセットが存在する場合、重大な変更を引き起こす編集操作が制限されるようになりました。 データセットを関連付けると、フィールドの名前変更や削除、フィールドのデータタイプや形式の変更、ID 記述子の変更、関連するフィールドの管理による既存のフィールドの削除、割り当てられたクラスの変更ができなくなります。追加の変更とフィールドの非推奨（廃止予定）は引き続きサポートされます。 |

詳しくは、[XDM の概要 &#x200B;](../../xdm/home.md) を参照してください。

