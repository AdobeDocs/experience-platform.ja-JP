---
title: Adobe Experience Platform リリースノート
description: Adobe Experience Platform の 2023年9月のリリースノート。
exl-id: f854f9e5-71be-4d56-a598-cfeb036716cb
source-git-commit: 2492651fb03e0bfc5c9f68a9b063689a1b9001a3
workflow-type: tm+mt
source-wordcount: '907'
ht-degree: 34%

---

# Adobe Experience Platform リリースノート

**リリース日：2023年9月28日（PT）**

Adobe Experience Platform の新機能：

- [計算済み属性](#computed-attributes)

 Experience Platform の既存の機能に対するアップデート：

- [アラート](#alerts)
- [データ収集](#data-collection)
- [ID サービス](#identity-service)
- [セグメント化サービス](#segmentation)
- [ソース](#sources)

## 計算済み属性 {#computed-attributes}

計算済み属性を使用すると、直感的な UI でイベントデータを簡単にプロファイル属性に要約し、動作ベースのセグメント化、パーソナライゼーション、アクティベーションを強化できます。 この機能を使用すると、計算済み属性をセルフサービス方式で作成し、管理し、セグメント化、Real-Time CDP宛先、Adobe Journey Optimizerで使用できます。 さらに、計算済み属性は、セグメント化とジャーニーワークフローを簡素化し、関連するエクスペリエンスをシームレスに配信できるようにします。 計算済み属性の詳細については、 [計算済み属性の概要](../../profile/computed-attributes/overview.md).

## アラート {#alerts}

Experience Platform では、様々な Platform アクティビティに関するイベントベースのアラートを登録できます。Platform ユーザーインターフェイスの「[!UICONTROL アラート]」タブを使用して、様々なアラートルールを購読し、UI 内または電子メール通知を通じてアラートメッセージを受け取るように選択できます。

**新機能または更新された機能**

| 機能 | 説明 |
| --- | --- |
| 「アラート履歴」タブ | アラート [!UICONTROL 履歴] タブに、遅延、開始、成功、失敗を含むすべてのイベントが含まれるようになりました。 詳しくは、 [アラート UI ドキュメント](../../observability/alerts/ui.md) 「履歴」タブの詳細を参照してください。 |

{style="table-layout:auto"}

アラートの詳細については、 [[!DNL Observability Insights] 概要](../../observability/home.md).

## データ収集 {#data-collection}

Adobe Experience Platform では、クライアントサイドのカスタマーエクスペリエンスデータを収集し、Adobe Experience Platform Edge Network に送信できます。そこでデータを補強して変換し、アドビまたはアドビ以外の宛先に配信できます。

**新機能または更新された機能**

| タイプ | 機能 | 説明 |
| --- | --- | --- |
| データストリーム | デバイス参照のサポート | データストリームを設定する際に、収集するデバイス参照情報のレベルを選択できるようになりました。 デバイス参照情報には、ページとのやり取りに使用されるデバイス、ハードウェア、オペレーティングシステム、ブラウザーに関するデータが含まれます。 <br>  ユーザーエージェントやクライアントヒントと共に、デバイス参照情報を収集することはできません。 デバイス情報の収集を選択すると、ユーザーエージェントとクライアントヒントの収集が無効になり、その逆も無効になります。 すべてのデバイス参照情報は、 `xdm:device` フィールドグループを使用します。 詳しくは、 [データ・ストリームの構成](../../datastreams/configure.md#geolocation-device-lookup). |
| 拡張機能 | [!DNL TikTok] web イベント API 拡張機能 | The [[!DNL TikTok] ウェブイベント API](https://exchange.adobe.com/apps/ec/109834/tiktok-web-events-api) 拡張機能を使用すると、 Adobe Experience Platform Edge Network で取得したデータを活用し、に送信できます。 [!DNL TikTok] を使用して、サーバー側のイベントの形式で [!DNL TikTok] ウェブイベント API。 |

{style="table-layout:auto"}

データ収集の詳細については、 [データ収集の概要](../../tags/home.md).

## ID サービス {#identity-service}

Adobe Experience Platform ID サービスを利用すると、デバイスやシステム間で ID を橋渡しすることで、顧客とその行動を包括的に把握し、インパクトのある個人的なデジタルエクスペリエンスをリアルタイムで提供できます。

**新機能または更新された機能**

| 機能 | 説明 |
| --- | --- |
| ID サービス UI の強化 | Experience PlatformUI の改善されたカスタム名前空間作成ツールを使用して、カスタム名前空間と、対応する ID タイプをより適切に管理できます。 拡張された ID サービス UI には、次の機能が備わっています。 <ul><li>コンテキストエクスペリエンス： ID 名前空間と ID タイプに対する視覚的な手掛かり、明確性、コンテキスト。</li><li>精度：エラー処理の改善。ID 名の重複がなくなりました。</li><li>検出性：製品内ダイアログ内からドキュメントにアクセスできます。</li></ul> 詳しくは、 [カスタム名前空間の作成](../../identity-service/namespaces.md#create-namespaces). |
| ID グラフの制限の変更 | ID グラフの上限が 150 ID から 50 ID に変更されました。 新しい ID がフルグラフに取り込まれると、取り込みタイムスタンプと ID タイプに基づく最も古い ID が削除されます。 Cookie の ID タイプは削除用に優先されます。 実稼動用サンドボックスに次の情報が含まれている場合は、Adobeアカウントチームに連絡して、ID タイプの変更をリクエストしてください。 <ul><li>ユーザー識別子（CRM ID など）を cookie/デバイス id タイプとして設定するカスタム名前空間。</li><li>cookie/device 識別子がクロスデバイス id タイプとして設定されるカスタム名前空間。</li></ul> Adobeエンジニアリングがこれらのリクエストを手動で処理します。 詳しくは、 [ID サービスデータのガードレール](../../identity-service/guardrails.md). |

{style="table-layout:auto"}

ID サービスの詳細については、 [ID サービスの概要](../../identity-service/home.md).

## セグメント化サービス {#segmentation}

[!DNL Segmentation Service] を使用すると、[!DNL Experience Platform] に保存されている、個人（顧客、見込み客、ユーザー、組織など）に関連するデータをオーディエンスにセグメント化できます。オーディエンスは、セグメント定義または [!DNL Real-Time Customer Profile] データの他のソースを通じて作成できます。これらのオーディエンスは [!DNL Platform] で一元的に設定および管理されており、Adobe ソリューションから簡単にアクセスできます。

**新機能または更新された機能**

| 機能 | 説明 |
| ------- | ----------- |
| カスタマイズ可能な列 | サイズを変更できる列を使用して、Audience Portal のレイアウトをカスタマイズできるようになりました。 この機能の詳細については、 [セグメント化 UI ガイド](../../segmentation/ui/overview.md#customize). |
| 頻度の分類を更新 | これで、組織内のオーディエンスの更新頻度の分類を表示できます。 この機能の詳細については、 [セグメント化 UI ガイド](../../segmentation/ui/overview.md#browse). |

ソースの詳細については、 [ソースの概要](../../sources/home.md).

セグメント化サービスの詳細については、「[セグメント化サービスの概要](../../segmentation/home.md)」を参照してください。

## ソース {#sources}

Experience Platform は、様々なデータプロバイダーのソース接続を簡単に設定できる RESTful API とインタラクティブ UI を備えています。これらのソース接続を使用すると、外部ストレージシステムおよび CRM サービスの認証と接続、取得実行時間の設定、データ取得スループットの管理を行うことができます。

**新機能または更新された機能**

| 機能 | 説明 |
| --- | --- |
| 次の新しいパラメーターが追加されました： `offset` セルフサービスソースでのページネーション（バッチ SDK） | 次に、 `endConditionName` および `endConditionValue` を使用している場合は `offset` ページネーション。 これらのパラメーターを使用すると、次の HTTP リクエストでページネーションループを終了する条件を指定できます。 詳しくは、 [セルフサービスソース（バッチ SDK）のページネーションガイド](../../sources/sources-sdk/config/sourcespec.md#pagination). |

{style="table-layout:auto"}

ソースの詳細については、 [ソースの概要](../../sources/home.md).