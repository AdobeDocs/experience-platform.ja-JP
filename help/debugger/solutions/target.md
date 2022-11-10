---
title: Adobe Experience Platform Debugger を使用したAdobe Target実装のテスト
description: Adobe Experience Platform Debugger を使用して、Adobe Targetで有効な Web サイトのテストとデバッグをおこなう方法について説明します。
source-git-commit: 1ce7ac78936040d76faa3a58b92333a737fbeb66
workflow-type: tm+mt
source-wordcount: '1220'
ht-degree: 6%

---

# Adobe Experience Platform Debugger を使用したAdobe Target実装のテスト

Adobe Experience Platform Debugger は、Adobe Targetの実装でツールされた Web サイトのテストとデバッグに役立つツールのスイートを提供します。 このガイドでは、Target が有効な Web サイトで Platform Debugger を使用する際の一般的なワークフローとベストプラクティスを説明します。

## 前提条件

Platform Debugger を Target で使用するには、Web サイトで [at.js ライブラリ](https://developer.adobe.com/target/implement/client-side/atjs/how-atjs-works/) バージョン 1.x 以降。 以前のバージョンはサポートされていません。

## Platform Debugger の初期化

テストする Web サイトをブラウザーで開き、Platform Debugger 拡張機能を開きます。

左側のナビゲーションの「**[!DNL Target]**」を選択します。 Platform Debugger が互換性のあるバージョンの at.js がサイトで実行されていることを検出した場合は、Adobe Targetの実装の詳細が表示されます。

![Platform Debugger で選択された Target ビュー。現在表示されているブラウザーページでAdobe Targetがアクティブであることを示します。](../images/solutions/target/target-initialized.png)

## グローバル設定情報

実装のグローバル設定に関する情報は、Platform Debugger の Target ビューの上部に表示されます。

![Platform Debugger 内で強調表示されている Target のグローバル設定情報](../images/solutions/target/global-config.png)

| 名前 | 説明 |
| --- | --- |
| クライアントコード | 組織を識別する一意の ID。 |
| バージョン | Web サイトに現在インストールされているAdobe Targetライブラリのバージョン。 |
| グローバルリクエスト名 | の名前 [グローバル mbox](https://developer.adobe.com/target/implement/client-side/atjs/global-mbox/global-mbox-overview/?) Target 実装の場合、デフォルト名は `target-global-mbox`. |
| ページ読み込みイベント | 次の値を示す boolean 値。 [ページ読み込みイベント](https://developer.adobe.com/target/implement/client-side/atjs/how-atjs-works/how-atjs-works/#atjs-2x-diagrams) が発生しました。 ページ読み込みイベントは at.js 2.x でのみサポートされています。互換性のないバージョンの場合、この値のデフォルトはです。 `None`. |

{style=&quot;table-layout:auto&quot;}

## [!DNL Network Requests] {#network}

選択 **[!DNL Network Requests]** ページ上で行われた各ネットワークリクエストの概要情報を表示します。

![この [!DNL Network Requests] Platform Debugger 内で選択された Target のセクション](../images/solutions/target/network-requests.png)

ページ上でアクションを実行する（ページの再読み込みを含む）と、新しい列が自動的にテーブルに追加され、アクションの順序と、各リクエスト間での値の変更方法を表示できます。

![この [!DNL Network Requests] Platform Debugger 内で選択された Target のセクション](../images/solutions/target/new-request.png)

次の値が取り込まれます。

| 名前 | 説明 |
| --- | --- |
| [!DNL Page Title] | このリクエストを開始したページのタイトル。 |
| [!DNL Page URL] | リクエストを開始したページの URL。 |
| [!DNL URL] | リクエストの生の URL。 |
| [!DNL Method] | リクエストの HTTP メソッド。 |
| [!DNL Query String] | URL から取得されたリクエストのクエリ文字列。 |
| [!DNL POST Body] | リクエストの本文 (POSTリクエストに対してのみ設定 )。 |
| [!DNL Pathname] | リクエスト URL のパス名。 |
| [!DNL Hostname] | リクエスト URL のホスト名。 |
| [!DNL Domain] | リクエスト URL のドメイン。 |
| [!DNL Timestamp] | リクエスト（またはイベント）が発生した時点を示すタイムスタンプ（ブラウザーのタイムゾーン内）。 |
| [!DNL Time Since Page Load] | ページがリクエストの時点で最初に読み込まれてからの経過時間。 |
| [!DNL Initiator] | リクエストのイニシエーター。 つまり誰がリクエストをしたのか？ |
| [!DNL clientCode] | Target が認識する、組織のアカウントの識別子。 |
| [!DNL requestType] | リクエストに使用された API。 at.js 1.x を使用している場合、値は `/json`. at.js 2.x を使用している場合、値は `delivery`. |
| [!DNL Audience Manager Blob] | 「blob」と呼ばれる暗号化されたAudience Managerメタデータに関する情報を提供します。 |
| [!DNL Audience Location Hint] | データ収集地域 ID です。これは、特定の ID サービスデータセンターの地理的場所を示す数値識別子です。詳しくは、 [DCS の地域 ID、場所、ホスト名](https://experienceleague.adobe.com/docs/audience-manager/user-guide/api-and-sdk-code/dcs/dcs-api-reference/dcs-regions.html?lang=ja) と、 [`getLocationHint`](https://experienceleague.adobe.com/docs/id-service/using/id-service-api/methods/getlocationhint.html?lang=en#reference-a761030ff06c4439946bb56febf42d4c). |
| [!DNL Browser Height] | ブラウザーの高さ（ピクセル単位）。 |
| [!DNL Browser Time Offset] | タイムゾーンに関連付けられているブラウザーの時間オフセット。 |
| [!DNL Browser Width] | ブラウザーの幅（ピクセル単位）。 |
| [!DNL Color Depth] | 画面の色深度。 |
| [!DNL context] | 画面のサイズやクライアントプラットフォームなど、リクエストの作成に使用されるブラウザーに関するコンテキスト情報を含むオブジェクトです。 |
| [!DNL prefetch] | の間に使用されるパラメーター `prefetch` 処理中。 |
| [!DNL execute] | 次の期間に使用されるパラメーター `execute` 処理中。 |
| [!DNL Experience Cloud Visitor ID] | 1 つが検出された場合は、 [Experience CloudID (ECID)](https://experienceleague.adobe.com/docs/id-service/using/intro/overview.html?lang=ja) 現在のサイト訪問者に割り当てられている |
| [!DNL experienceCloud] | この特定のExperience Cloudセッションのユーザー ID を保持します。A4T [追加データ ID](https://experienceleague.adobe.com/docs/target/using/integrate/a4t/before-implement.html?#section_2C1F745A2B7D41FE9E30915539226E3A)、および [訪問者 ID (ECID)](https://experienceleague.adobe.com/docs/id-service/using/intro/overview.html). |
| [!DNL id] | この [ターゲット ID](https://developers.adobetarget.com/api/delivery-api/#section/Identifying-Visitors/Target-ID) 訪問者に対して。 |
| [!DNL Mbox Host] | この [ホスト](https://experienceleague.adobe.com/docs/target/using/administer/hosts.html?lang=ja) Target リクエストがに対して実行されたこと |
| [!DNL Mbox PC] | をカプセル化する文字列 [`mbox`](https://developer.adobe.com/target/implement/client-side/atjs/global-mbox/global-mbox-overview/) セッション ID と [Adobe Target Edge](https://experienceleague.adobe.com/docs/target/using/introduction/how-target-works.html#concept_0AE2ED8E9DE64288A8B30FCBF1040934) 場所のヒント。 この値は、セッションと Edge の場所を固定したままにするために at.js で使用されます。 |
| [!DNL Mbox Referrer] | 特定の [`mbox`](https://developer.adobe.com/target/implement/client-side/atjs/global-mbox/global-mbox-overview/) リクエスト。 |
| [!DNL Mbox URL] | の URL [`mbox`](https://developer.adobe.com/target/implement/client-side/atjs/global-mbox/global-mbox-overview/) サーバー。 |
| [!DNL Mbox Version] | のバージョン。 [`mbox`](https://developer.adobe.com/target/implement/client-side/atjs/global-mbox/global-mbox-overview/) 使用されています。 |
| [!DNL mbox3rdPartyId] | この [`mbox3rdPartyId`](https://experienceleague.adobe.com/docs/target/using/audiences/visitor-profiles/3rd-party-id.html) 現在の訪問者に割り当てられました。 |
| [!DNL mboxRid] | この [`mbox`](https://developer.adobe.com/target/implement/client-side/atjs/global-mbox/global-mbox-overview/) リクエスト ID。 |
| [!DNL requestId] | リクエストの一意の ID。 |
| [!DNL Screen Height] | 画面の高さをピクセル単位で指定します。 |
| [!DNL Screen Width] | 画面の幅をピクセル単位で指定します。 |
| [!DNL Supplemental Data ID] | 訪問者を対応するAdobe TargetおよびAdobe Analytics呼び出しと照合するために使用される、システム生成 ID。 詳しくは、 [A4T トラブルシューティングガイド](https://experienceleague.adobe.com/docs/target/using/integrate/a4t/troubleshoot-a4t/a4t-troubleshooting.html?#section_75002584FA63456D8D9086172925DD8D) を参照してください。 |
| [!DNL vst] | この [Experience CloudID サービス API の設定](https://experienceleague.adobe.com/docs/id-service/using/id-service-api/configurations/function-vars.html). |
| [!DNL webGLRenderer] | ページで使用される WebGL レンダラーに関する情報を提供します（該当する場合）。 |

{style=&quot;table-layout:auto&quot;}

特定のネットワークイベントのパラメータの詳細を表示するには、該当するテーブルセルを選択します。 ポップオーバーが表示され、説明や値など、パラメーターの詳細情報が示されます。 値が JSON オブジェクトの場合、ダイアログにはオブジェクトの構造の完全にナビゲーション可能なビューが含まれます。

![この [!DNL Network Requests] Platform Debugger 内で選択された Target のセクション](../images/solutions/target/request-param-details.png)

## [!DNL Configuration]

選択 **[!DNL Configuration]** :Target の追加のデバッグツールの選択を有効または無効にします。

![この [!DNL Configuration Requests] Platform Debugger 内で選択された Target のセクション](../images/solutions/target/configuration.png)

| デバッグツール | 説明 |
| --- | --- |
| [!DNL Target Console Logging] | 有効にすると、ブラウザーのコンソールタブで at.js ログにアクセスできます。 この機能は、 `mboxDebug` クエリパラメーター（任意の値）をブラウザー URL に追加します。 |
| [!DNL Target Diable] | 有効にすると、ページ上で Target のすべての機能が無効になります。 これを使用して、Target 固有のオファーがページ上で問題の原因であるかどうかを判断できます。 |
| [!DNL Target Trace] | **注意**:この機能を有効にするには、ログインする必要があります。<br><br>有効にした場合、トラッキングトークンはクエストごとに送信され、各応答でトレースオブジェクトが返されます。 `at.js` 応答を解析する `window.__targetTraces`. 各トレースオブジェクトには、[[!DNL Network Requests] タブ ] に次の追加が含まれます。<ul><li>プロファイルスナップショット。リクエストの前後の属性を確認できます。</li><li>一致するものと一致しないもの [アクティビティ](https://experienceleague.adobe.com/docs/target/using/activities/target-activities-guide.html)：現在のプロファイルが特定のアクティビティの対象として認定された理由を示します。<ul><li>これは、特定の時点でプロファイルの対象となるオーディエンスとその理由を特定するのに役立ちます。</li><li>Target ドキュメントには、様々なアクティビティタイプに関する詳細情報が含まれています</li></ul></li></ul> |

{style=&quot;table-layout:auto&quot;}
