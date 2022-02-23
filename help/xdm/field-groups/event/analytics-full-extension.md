---
title: Adobe Analytics ExperienceEvent Full 拡張スキーマフィールドグループ
description: このドキュメントでは、「 Adobe Analytics ExperienceEvent Full Extension 」スキーマフィールドグループの概要を説明します。
source-git-commit: bfdcee33fb2cbd28039633d1d981149c40aa1d68
workflow-type: tm+mt
source-wordcount: '894'
ht-degree: 8%

---

# [!UICONTROL Adobe Analytics ExperienceEvent Full 拡張機能] スキーマフィールドグループ

[!UICONTROL Adobe Analytics ExperienceEvent Full 拡張機能] は、 [[!DNL XDM ExperienceEvent] クラス](../../classes/experienceevent.md):Adobe Analyticsが収集する一般的な指標を取り込みます。

このドキュメントでは、Analytics 拡張機能フィールドグループの構造と使用例について説明します。

>[!NOTE]
>
>このフィールドグループ内で繰り返される要素のサイズと数により、このガイドに示す多くのフィールドが折りたたまれてスペースに保存されています。 このフィールドグループの完全な構造を見るには、次の操作を行います。 [Platform UI で参照する ](../../ui/explore.md) または [パブリック XDM リポジトリ](https://github.com/adobe/xdm/blob/master/extensions/adobe/experience/analytics/experienceevent-all.schema.json).

## フィールドグループ構造

フィールドグループには、 `_experience` オブジェクトをスキーマに追加します。スキーマ自体には単一の `analytics` オブジェクト。

![Analytics フィールドグループの最上位フィールド](../../images/field-groups/analytics-full-extension/full-schema.png)

| プロパティ | データタイプ | 説明 |
| --- | --- | --- |
| `customDimensions` | オブジェクト | Analytics で追跡されているカスタムディメンションをキャプチャします。 詳しくは、 [次の款](#custom-dimensions) を参照してください。 |
| `endUser` | オブジェクト | イベントをトリガーしたエンドユーザーの Web インタラクションの詳細をキャプチャします。 詳しくは、 [次の款](#end-user) を参照してください。 |
| `environment` | オブジェクト | イベントをトリガーしたブラウザーおよびオペレーティングシステムに関する情報をキャプチャします。 詳しくは、 [次の款](#environment) を参照してください。 |
| `event1to100`<br><br>`event101to200`<br><br>`event201to300`<br><br>`event301to400`<br><br>`event401to500`<br><br>`event501to100`<br><br>`event601to700`<br><br>`event701to800`<br><br>`event801to900`<br><br>`event901to1000` | オブジェクト | フィールドグループには、最大 1000 個のカスタムイベントを取り込むためのオブジェクトフィールドが用意されています。 詳しくは、 [次の款](#events) を参照してください。 |
| `session` | オブジェクト | イベントをトリガーしたセッションに関する情報をキャプチャします。 詳しくは、 [次の款](#session) を参照してください。 |

{style=&quot;table-layout:auto&quot;}

## `customDimensions` {#custom-dimensions}

`customDimensions` カスタムをキャプチャ [寸法](https://experienceleague.adobe.com/docs/analytics/components/dimensions/overview.html?lang=ja) Analytics で追跡される

![customDimensions フィールド](../../images/field-groups/analytics-full-extension/customDimensions.png)

| プロパティ | データタイプ | 説明 |
| --- | --- | --- |
| `eVars` | オブジェクト | 最大 250 個のコンバージョン変数 ([eVar](https://experienceleague.adobe.com/docs/analytics/components/dimensions/evar.html?lang=ja)) をクリックします。 このオブジェクトのプロパティはキー設定されています `eVar1` から `eVar250` およびは、そのデータ型に対する文字列のみを受け入れます。 |
| `hierarchies` | オブジェクト | 最大 5 つのカスタム階層変数 ([hiers](https://experienceleague.adobe.com/docs/analytics/implementation/vars/page-vars/hier.html?lang=ja)) をクリックします。 このオブジェクトのプロパティはキー設定されています `hier1` から `hier5`：これ自体は、次のサブプロパティを持つオブジェクトです。<ul><li>`delimiter`:の下に提供されるリストの生成に使用される元の区切り文字 `values`.</li><li>`values`:階層レベル名の区切りリスト。文字列として表されます。</li></ul> |
| `listProps` | オブジェクト | 最大 75 個を取り込むオブジェクト [リスト prop](https://experienceleague.adobe.com/docs/analytics/implementation/vars/page-vars/prop.html#list-props). このオブジェクトのプロパティはキー設定されています `prop1` から `prop75`：これ自体は、次のサブプロパティを持つオブジェクトです。<ul><li>`delimiter`:の下に提供されるリストの生成に使用される元の区切り文字 `values`.</li><li>`values`:prop の値の区切りリスト。文字列として表されます。</li></ul> |
| `lists` | オブジェクト | 最大 3 つまで取り込むオブジェクト [リスト](https://experienceleague.adobe.com/docs/analytics/implementation/vars/page-vars/list.html). このオブジェクトのプロパティはキー設定されています `list1` から `list3`. 各プロパティには、 `list` 配列 [[!UICONTROL キーと値のペア]](../../data-types/key-value-pair.md) データタイプ。 |
| `props` | オブジェクト | 最大 75 個を取り込むオブジェクト [prop](https://experienceleague.adobe.com/docs/analytics/implementation/vars/page-vars/prop.html). このオブジェクトのプロパティはキー設定されています `prop1` から `prop75` およびは、そのデータ型に対する文字列のみを受け入れます。 |
| `postalCode` | 文字列 | クライアントが指定した郵便番号。 |
| `stateProvince` | 文字列 | クライアントが指定した都道府県の場所。 |

{style=&quot;table-layout:auto&quot;}

## `endUser` {#end-user}

`endUser` は、イベントをトリガーしたエンドユーザーの web インタラクションの詳細をキャプチャします。

![endUser フィールド](../../images/field-groups/analytics-full-extension/endUser.png)

| プロパティ | データタイプ | 説明 |
| --- | --- | --- |
| `firstWeb` | [[!UICONTROL Web 情報]](../../data-types/web-information.md) | このエンドユーザーの最初のエクスペリエンスイベントからの Web ページ、リンクおよび参照元に関する情報。 |
| `firstTimestamp` | 整数 | このエンドユーザーの最初の ExperienceEvent の Unix タイムスタンプ。 |

## `environment` {#environment}

`environment` は、イベントをトリガーしたブラウザーとオペレーティングシステムに関する情報を取り込みます。

![環境フィールド](../../images/field-groups/analytics-full-extension/environment.png)

| プロパティ | データタイプ | 説明 |
| --- | --- | --- |
| `browserIDStr` | 文字列 | 使用するブラウザーのAdobe Analytics識別子 ( 別名： [ブラウザータイプディメンション](https://experienceleague.adobe.com/docs/analytics/components/dimensions/browser-type.html)) をクリックします。 |
| `operatingSystemIDStr` | 文字列 | 使用するオペレーティングシステムのAdobe Analytics識別子 ( 別名： [オペレーティングシステムタイプディメンション](https://experienceleague.adobe.com/docs/analytics/components/dimensions/operating-system-types.html)) をクリックします。 |

## カスタムイベントフィールド {#events}

Analytics 拡張機能フィールドグループには、最大 100 個のオブジェクトフィールドが用意されています [カスタムイベント指標](https://experienceleague.adobe.com/docs/analytics/components/metrics/custom-events.html) フィールドグループの合計 1,000 個です。

各最上位イベントオブジェクトには、それぞれの範囲に対応する個々のイベントオブジェクトが含まれます。 例： `event101to200` は、次のキーを持つイベントを含みます。 `event101` から `event200`.

各偶数オブジェクトは、 [[!UICONTROL 測定]](../../data-types/measure.md) 一意の識別子と定量化可能な値を提供するデータタイプ。

![カスタムイベントフィールド](../../images/field-groups/analytics-full-extension/event-vars.png)

## `session` {#session}

`session` は、イベントをトリガーしたセッションに関する情報を取得します。

![セッションフィールド](../../images/field-groups/analytics-full-extension/session.png)

| プロパティ | データタイプ | 説明 |
| --- | --- | --- |
| `search` | [[!UICONTROL 検索]](../../data-types/search.md) | セッションエントリの Web 検索またはモバイル検索に関する情報をキャプチャします。 |
| `web` | [[!UICONTROL Web 情報]](../../data-types/web-information.md) | セッションエントリのリンククリック数、Web ページの詳細、リファラー情報およびブラウザーの詳細に関する情報をキャプチャします。 |
| `depth` | 整数 | エンドユーザーの現在のセッションの深さ（ページ番号など）。 |
| `num` | 整数 | エンドユーザーの現在のセッション番号。 |
| `timestamp` | 整数 | セッションエントリの Unix タイムスタンプ。 |

## 次の手順

このドキュメントでは、Analytics 拡張機能フィールドグループの構造と使用例について説明しました。 フィールドグループ自体の詳細については、 [パブリック XDM リポジトリ](https://github.com/adobe/xdm/blob/master/extensions/adobe/experience/analytics/experienceevent-all.schema.json).

このフィールドグループを使用して、Adobe Experience Platform Web SDK を使用して Analytics データを収集する場合は、 [データストリームの設定](../../../edge/fundamentals/datastreams.md) サーバー側でデータを XDM にマッピングする方法を説明します。
