---
title: Adobe Content Analytics拡張機能の概要
description: Adobe Experience PlatformのAdobe Content Analytics タグ拡張機能について説明します。
exl-id: fcc46c86-e765-4bc7-bfdf-b8b10e8afacc
source-git-commit: 34f50c6e92cb1bde4e5f27a4378058615fb0cdff
workflow-type: tm+mt
source-wordcount: '545'
ht-degree: 0%

---

# Adobe Content Analytics拡張機能の概要

[!DNL Adobe Content Analytics] タグ拡張機能を使用すると、web サイト上のコンテンツ関連のイベントをトラッキングできます。 拡張機能は、web プロパティからExperience Platform Edge Networkを通じて、コンテンツデータ（エクスペリエンスとアセット）をAdobe Experience Cloudのデータストリームに送信します。

拡張機能を使用すると、特定のコンテンツに関連するイベントデータをExperience Platformにストリーミングでき、そのデータをCustomer Journey Analyticsのコンテンツ分析レポートで使用できます。

このドキュメントでは、タグ UI でタグ拡張機能を設定する方法について説明します。

## Adobe Content Analytics タグ拡張機能のインストール {#install}

Adobe Content Analytics タグ拡張機能は、[Content Analytics ガイド付き設定ウィザード ](https://experienceleague.adobe.com/en/docs/analytics-platform/using/content-analytics/configuration/guided) を使用すると自動的に作成されるタグプロパティの一部として自動的にインストールされます。

<!--
### Manual installation

In case of a manual configuration, the Adobe Content Analytics tag extension needs a property to be installed on. If you have not done so already, see the documentation on [creating a tag property](https://experienceleague.adobe.com/en/docs/platform-learn/implement-in-websites/configure-tags/create-a-property).

After you have created a property or when you select the property created using the [Content Analytics guided configuration wizard](https://experienceleague.adobe.com/en/docs/analytics-platform/using/content-analytics/configuration/guided), open the property and select the **[!UICONTROL Extensions]** tab on the left side bar.

Select the **[!UICONTROL Catalog]** tab. From the list of available extensions, find the **[!DNL Adobe Content Analytics]** extension and select **[!UICONTROL Install]**.

![Image showing the Tags UI with the Web SDK extension selected](assets/aca-tag-install.png)

After selecting **[!UICONTROL Install]**, you must configure the Adobe Content Analytics tag extension and save the configuration.
-->

<!--
## Configure schema

The [Content Analytics guided configuration wizard](https://experienceleague.adobe.com/en/docs/analytics-platform/using/content-analytics/configuration/guided) automatically populates the proper value for the **[!UICONTROL Tenant Schema Name]**. 

![Image that shows the Schema configuration of the Adobe Content Analytics tag extension in the Tags UI](assets/aca-tag-schema.png)

>[!WARNING]
>
>Do not modify the value for **[!UICONTROL Tenant Schema Name]**.

-->

## データストリームの設定

[Content Analyticsのガイド付き設定ウィザード ](https://experienceleague.adobe.com/en/docs/analytics-platform/using/content-analytics/configuration/guided) は、**[!UICONTROL Sandbox]** および **[!UICONTROL 実稼動データストリーム]** の適切な値を自動的に選択します。 オプションで、追加の **[!UICONTROL ステージングデータストリーム]** および **[!UICONTROL 開発データストリーム]** を設定できます。

![ タグ UI のAdobe Content Analytics タグ拡張機能のデータストリーム設定を示す画像 ](assets/aca-tag-datastreams.png)

別のサンドボックスで別のデータストリームとともにContent Analyticsを使用する場合は、**[!UICONTROL Sandbox]** および **[!UICONTROL 実稼動データストリーム]** の自動選択値を上書きできます。 その際は、使用可能なドロップダウンメニューからサンドボックスとデータストリームを選択するか、**[!UICONTROL 値を入力]** を選択して、各環境のカスタムデータストリーム ID を入力します。

>[!IMPORTANT]
>
>別のサンドボックスとデータストリームを設定する場合、次のことを確認します
>
>* 選択したサンドボックスは、別のContent Analytics設定にまだ関連付けられていません。
>* 選択したデータストリームでは、有効なExperience Platform エクスペリエンスイベントデータセットでContent Analytics サービスが設定されています。

データストリームの設定方法については、[ データストリーム ](../../../../datastreams/overview.md) に関するガイドを参照してください。

## エクスペリエンスのキャプチャと定義の設定

「**[!UICONTROL エクスペリエンスのキャプチャと定義]**」セクションでは、Content Analyticsのデータを収集する際にエクスペリエンスを含める **[!UICONTROL エクスペリエンスを含める]** ことを有効にできます。

![ 拡張機能の「エクスペリエンスキャプチャと定義」セクションを示す画像 ](assets/aca-tag-experiencecapture.png)

1. **[!UICONTROL エクスペリエンスを含める]** を有効にします。
1. オプション。 web サイトでのコンテンツのレンダリング方法をパラメーターで指定します。 パラメーターは、0 個以上の **[!UICONTROL ドメイン正規表現]** と **[!UICONTROL クエリパラメーター]** の組み合わせです。
   1. **[!UICONTROL ドメイン正規表現]** を入力します（例：`^(?!.*\b(store|help|admin)\b)`）。
   1. **[!UICONTROL クエリパラメーター]** のコンマ区切りリスト（例：`outdoors, patio, kitchen`）を指定します。
![ 閉じる ](./assets/CrossSize300.svg) を使用して個々のパラメータを削除するか、**[!UICONTROL すべてクリア]** を使用してすべてのパラメータを削除します。
1. ドメイン正規表現とクエリパラメーターの組み合わせを削除する場合は、「**[!UICONTROL 削除]**」を選択します。
1. 正規表現とクエリパラメーターの別の組み合わせを追加する場合は、「**[!UICONTROL 正規表現を追加]**」を選択します。

## イベントフィルタリングの設定

Content Analyticsのデータを収集する際に、「**[!UICONTROL イベントフィルタリング]**」セクションで正規表現を変更して **[!UICONTROL ページ URL]** および **[!UICONTROL Assets URL]** をフィルタリングできます。 [Content Analytics ガイド付き設定ウィザードで定義した正規表現は ](https://experienceleague.adobe.com/en/docs/analytics-platform/using/content-analytics/configuration/guided) 自動的に入力されます。

![ タグ UI のAdobe Content Analytics タグ拡張機能のイベントフィルタリング設定を示す画像 ](assets/aca-tag-eventfiltering.png)


### 例

* すべてのドキュメントページをContent Analyticsから除外する場合。<br/> 次の正規表現を使用します。`^(?!.*documentation).*`
* Content Analyticsからすべてのロゴ JPEGとSVG画像を除外する場合。<br/> 次の正規表現を使用します。`^(?!.*(logo\.jpg|)).*$`

**[!UICONTROL 正規表現をテスト]** を使用して、**[!UICONTROL 正規表現テスター]** で正規表現をテストできます。

![ タグ UI のAdobe Content Analytics タグ拡張機能の正規表現テスターを示す画像 ](assets/aca-tag-regextester.png)

