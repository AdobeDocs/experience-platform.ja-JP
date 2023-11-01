---
title: Adobe Experience Platform Debugger を使用した Adobe Target 実装のテスト
description: Adobe Experience Platform Debugger を使用して、Adobe Target が有効な web サイトのテストとデバッグを行う方法について説明します。
exl-id: f99548ff-c6f2-4e99-920b-eb981679de2d
source-git-commit: e300e57df998836a8c388511b446e90499185705
workflow-type: tm+mt
source-wordcount: '1209'
ht-degree: 98%

---

# Adobe Experience Platform Debugger を使用した Adobe Target 実装のテスト

Adobe Experience Platform Debugger は、Adobe Target の実装で作成された web サイトのテストとデバッグに役立つツールのスイートを提供します。このガイドでは、Target が有効な web サイトで Platform Debugger を使用する際の一般的なワークフローとベストプラクティスをいくつか説明します。

## 前提条件

Platform Debugger を Target に使用するには、web サイトで [at.js ライブラリ](https://developer.adobe.com/target/implement/client-side/atjs/how-atjs-works/)バージョン 1.x 以降を使用している必要があります。それより前のバージョンはサポートされていません。

## Platform Debugger の初期化

テストする web サイトをブラウザーで開き、Platform Debugger 拡張機能を開きます。

左側のナビゲーションの「**[!DNL Target]**」を選択します。 互換性のあるバージョンの at.js がサイトで実行されていることを Platform Debugger が検出した場合は、Adobe Target 実装の詳細が表示されます。

![Platform Debugger で選択された Target ビュー（現在表示されているブラウザーページで Adobe Target がアクティブであることを示します）](../images/solutions/target/target-initialized.png)

## グローバル設定情報

実装のグローバル設定に関する情報は、Platform Debugger の Target ビューの上部に表示されます。

![Platform Debugger 内で強調表示されている Target のグローバル設定情報](../images/solutions/target/global-config.png)

| 名前 | 説明 |
| --- | --- |
| クライアントコード | 組織を識別する一意の ID。 |
| バージョン | Web サイトに現在インストールされている Adobe Target ライブラリのバージョン。 |
| グローバルリクエスト名 | Target 実装の[グローバル mbox](https://developer.adobe.com/target/implement/client-side/atjs/global-mbox/global-mbox-overview/?) の名前（デフォルト名は `target-global-mbox`）。 |
| ページ読み込みイベント | [ページ読み込みイベント](https://developer.adobe.com/target/implement/client-side/atjs/how-atjs-works/how-atjs-works/#atjs-2x-diagrams)が発生したかどうかを示すブール値。ページ読み込みイベントは at.js 2.x でのみサポートされています。互換性のないバージョンの場合、この値はデフォルトで `None` になります。 |

{style="table-layout:auto"}

## [!DNL Network Requests] {#network}

「**[!DNL Network Requests]**」を選択すると、ページ上で行われた各ネットワークリクエストの概要情報が表示されます。

![Platform Debugger 内で選択された Target の「[!DNL Network Requests]」セクション](../images/solutions/target/network-requests.png)

ページ上でアクション（ページの再読み込みなど）を実行すると、新しい列がテーブルに自動的に追加されるので、アクションのシーケンスと、リクエスト間での値の変化を確認できます。

![Platform Debugger 内で選択された Target の「[!DNL Network Requests]」セクション](../images/solutions/target/new-request.png)

次の値が取り込まれます。

| 名前 | 説明 |
| --- | --- |
| [!DNL Page Title] | このリクエストを開始したページのタイトル。 |
| [!DNL Page URL] | リクエストを開始したページの URL。 |
| [!DNL URL] | リクエストの生の URL。 |
| [!DNL Method] | リクエストの HTTP メソッド。 |
| [!DNL Query String] | リクエストのクエリ文字列（URL から取得されたもの）。 |
| [!DNL POST Body] | リクエストの本文（POST リクエストの場合にのみ設定）。 |
| [!DNL Pathname] | リクエスト URL のパス名。 |
| [!DNL Hostname] | リクエスト URL のホスト名。 |
| [!DNL Domain] | リクエスト URL のドメイン。 |
| [!DNL Timestamp] | ブラウザーのタイムゾーン内でリクエスト（またはイベント）が発生した時点を示すタイムスタンプ。 |
| [!DNL Time Since Page Load] | リクエスト時にページが最初に読み込まれてからの経過時間。 |
| [!DNL Initiator] | リクエストのイニシエーター。つまり、誰がリクエストを実行したか。 |
| [!DNL clientCode] | Target で認識される、組織のアカウントの識別子。 |
| [!DNL requestType] | リクエストに使用された API。 at.js 1.x を使用している場合、値は `/json` です。at.js 2.x を使用している場合、値は `delivery` です。 |
| [!DNL Audience Manager Blob] | 「blob」と呼ばれる暗号化された Audience Manager メタデータに関する情報を提供します。 |
| [!DNL Audience Location Hint] | データ収集地域 ID。これは、特定の ID サービスデータセンターの地理的場所を示す数値識別子です。詳しくは、[DCS の地域 ID、場所、ホスト名](https://experienceleague.adobe.com/docs/audience-manager/user-guide/api-and-sdk-code/dcs/dcs-api-reference/dcs-regions.html?lang=ja)に関する Audience Manager のドキュメントと、[`getLocationHint`](https://experienceleague.adobe.com/docs/id-service/using/id-service-api/methods/getlocationhint.html#reference-a761030ff06c4439946bb56febf42d4c) に関する Experience Cloud ID サービスガイドを参照してください。 |
| [!DNL Browser Height] | ブラウザーの高さ（ピクセル単位）。 |
| [!DNL Browser Time Offset] | ブラウザーのタイムゾーンに関連付けられている、ブラウザーの時間オフセット。 |
| [!DNL Browser Width] | ブラウザーの幅（ピクセル単位）。 |
| [!DNL Color Depth] | 画面の色深度。 |
| [!DNL context] | 画面のサイズやクライアントプラットフォームなど、リクエストの実行に使用されるブラウザーに関するコンテキスト情報を含んだオブジェクト。 |
| [!DNL prefetch] | `prefetch` 処理中に使用されるパラメーター。 |
| [!DNL execute] | `execute` 処理中に使用されるパラメーター。 |
| [!DNL Experience Cloud Visitor ID] | 1 つ検出された場合は、現在のサイト訪問者に割り当てられている [Experience Cloud ID（ECID）](https://experienceleague.adobe.com/docs/id-service/using/intro/overview.html?lang=ja) に関する情報を提供します。 |
| [!DNL experienceCloud] | この特定のユーザーセッションの Experience Cloud ID を保持します：A4T [追加データ ID](https://experienceleague.adobe.com/docs/target/using/integrate/a4t/before-implement.html?lang=ja#section_2C1F745A2B7D41FE9E30915539226E3A) および[訪問者 ID（ECID）](https://experienceleague.adobe.com/docs/id-service/using/intro/overview.html?lang=ja)。 |
| [!DNL id] | 訪問者の [Target ID](https://developers.adobetarget.com/api/delivery-api/#section/Identifying-Visitors/Target-ID)。 |
| [!DNL Mbox Host] | Target リクエストの送信先となった[ホスト](https://experienceleague.adobe.com/docs/target/using/administer/hosts.html?lang=ja)。 |
| [!DNL Mbox PC] | [`mbox`](https://developer.adobe.com/target/implement/client-side/atjs/global-mbox/global-mbox-overview/) セッション ID と [Adobe Target Edge](https://experienceleague.adobe.com/docs/target/using/introduction/how-target-works.html?lang=ja#concept_0AE2ED8E9DE64288A8B30FCBF1040934) ロケーションヒントをカプセル化sした文字列。  この値は、セッションと Edge ロケーションを固定したままにするために at.js で使用されます。 |
| [!DNL Mbox Referrer] | 特定の [`mbox`](https://developer.adobe.com/target/implement/client-side/atjs/global-mbox/global-mbox-overview/) リクエストの URL リファラー。 |
| [!DNL Mbox URL] | [`mbox`](https://developer.adobe.com/target/implement/client-side/atjs/global-mbox/global-mbox-overview/) サーバーの URL。 |
| [!DNL Mbox Version] | 使用されている [`mbox`](https://developer.adobe.com/target/implement/client-side/atjs/global-mbox/global-mbox-overview/) のバージョン。 |
| [!DNL mbox3rdPartyId] | 現在の訪問者に割り当てられた [`mbox3rdPartyId`](https://experienceleague.adobe.com/docs/target/using/audiences/visitor-profiles/3rd-party-id.html?lang=ja)。 |
| [!DNL mboxRid] | [`mbox`](https://developer.adobe.com/target/implement/client-side/atjs/global-mbox/global-mbox-overview/) リクエスト ID。 |
| [!DNL requestId] | リクエストの一意の ID。 |
| [!DNL Screen Height] | 画面の高さ（ピクセル単位）。 |
| [!DNL Screen Width] | 画面の幅（ピクセル単位）。 |
| [!DNL Supplemental Data ID] | 訪問者を対応する Adobe Target および Adobe Analytics 呼び出しと照合するために使用されるシステム生成 ID。 詳しくは、[A4T トラブルシューティングガイド](https://experienceleague.adobe.com/docs/target/using/integrate/a4t/troubleshoot-a4t/a4t-troubleshooting.html?#section_75002584FA63456D8D9086172925DD8D)を参照してください。 |
| [!DNL vst] | [Experience Cloud ID サービス API の設定](https://experienceleague.adobe.com/docs/id-service/using/id-service-api/configurations/function-vars.html?lang=ja)。 |
| [!DNL webGLRenderer] | ページで使用される WebGL レンダラーに関する情報を提供します（該当する場合）。 |

{style="table-layout:auto"}

特定のネットワークイベントのパラメーターの詳細を表示するには、該当するテーブルセルを選択します。 ポップオーバーが表示され、説明や値など、パラメーターの詳細情報が表示されます。 値が JSON オブジェクトの場合、ダイアログには、オブジェクト構造の完全にナビゲーション可能なビューが含まれます。

![Platform Debugger 内で選択された Target の「[!DNL Network Requests]」セクション](../images/solutions/target/request-param-details.png)

## [!DNL Configuration]

「**[!DNL Configuration]**」を選択すると、Target の追加のデバッグツールの選択を有効または無効にすることができます。

![Platform Debugger 内で選択された Target の「[!DNL Configuration Requests]」セクション](../images/solutions/target/configuration.png)

| デバッグツール | 説明 |
| --- | --- |
| [!DNL Target Console Logging] | 有効にすると、ブラウザーのコンソールタブで at.js ログにアクセスできます。 この機能は、ブラウザーの URL に `mboxDebug` クエリパラメーター（任意の値）を追加することによっても有効にできます。 |
| [!DNL Target Diable] | 有効にすると、ページ上で Target のすべての機能が無効になります。 これを使用すると、Target 固有のオファーがページ上での問題の原因であるかどうかを判断できます。 |
| [!DNL Target Trace] | **メモ**：この機能を有効にするには、ログインする必要があります。<br><br>有効にした場合、クエストのたびにトラッキングトークンが送信され、各応答でトレースオブジェクトが返されます。`at.js` は応答 `window.__targetTraces` を解析します。各トレースオブジェクトには、次の追加事項に加えて「[!DNL Network Requests]」タブと同じ情報が含まれます。<ul><li>プロファイルスナップショット（リクエストの前後で属性を確認できるようになります）。</li><li>一致した／一致しなかった[アクティビティ](https://experienceleague.adobe.com/docs/target/using/activities/target-activities-guide.html?lang=ja)（現在のプロファイルが特定のアクティビティの対象として認定された／認定されなかった理由を示します）。<ul><li>これは、特定の時点でプロファイルの対象となるオーディエンスとその理由を特定するのに役立ちます。</li><li>様々なアクティビティタイプに関する詳細は、Target ドキュメントに記載されています</li></ul></li></ul> |

{style="table-layout:auto"}
