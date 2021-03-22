---
keywords: Audience ManagerDIL拡張子；宛先オーディエンスマネージャ；DIL拡張子
title: Audience Manager DIL 拡張機能
description: Audience ManagerDILの拡張は、Adobe Experience Platformのデータ管理プラットフォーム(DMP)の宛先です。 拡張機能について詳しくは、Adobe Exchange の拡張機能のページを参照してください。
translation-type: tm+mt
source-git-commit: 709908196bb5df665c7e7df10dc58ee9f3b0edbf
workflow-type: tm+mt
source-wordcount: '560'
ht-degree: 43%

---


# Audience Manager DIL 拡張機能 {#aam-dil-extension}

## 概要 {#overview}

これは、Adobe Audience Manager データ統合ライブラリ拡張機能（クライアントサイド実装）です。メモ：この拡張機能は、Adobe Analytics データのサーバーサイド転送（SSF）で使用するためのものではありません。SSF の場合は、Adobe Analytics 拡張機能を使用します。重要：バージョン8.0以降では、DILは[!DNL Experience Cloud] IDサービス、バージョン3.3以降に強い依存関係を持ちます。 [!DNL Experience Cloud] IDサービスとDILの両方を実装し、[!DNL Audience Manager]の完全なデータ統合機能を実現してください。

[!DNL Audience Manager] DILは、Adobe Experience Platformのデータ管理プラットフォーム(DMP)拡張です。拡張機能について詳しくは、Experience Platform Launch ドキュメントの [Audience Manager 拡張機能ページ](https://experienceleague.adobe.com/docs/launch/using/extensions-ref/adobe-extension/adobe-audience-manager-extension.html)を参照してください。

この宛先は[!DNL Experience Platform Launch]拡張子です。 Launch拡張機能がプラットフォームでどのように機能するかについて詳しくは、[Experience Platform Launch拡張機能の概要](../launch-extensions/overview.md)を参照してください。

![Audience Manager DIL 拡張機能](../../assets/catalog/data-management-platform/aam-dil-extension/configure.png)

## 前提条件 {#prerequisites}

この拡張機能は、[!DNL Destinations]カタログにあり、プラットフォームを購入したすべてのお客様に提供されます。

この拡張機能を使用するには、[!DNL Adobe Experience Platform Launch]にアクセスする必要があります。 [!DNL Platform Launch] は、Adobe Experience Cloud に付属の付加価値機能として提供されています。組織の管理者に連絡して、[!DNL Platform Launch]へのアクセス権を取得し、**[!UICONTROL manage_properties]**&#x200B;の権限を与えて、拡張機能をインストールするように依頼してください。

## 拡張機能のインストール {#install-extension}

[!DNL Audience Manager]DIL拡張をインストールするには：

[プラットフォームインターフェイス](http://platform.adobe.com/)で、**[!UICONTROL 宛先]**/**[!UICONTROL カタログ]**&#x200B;に移動します。

カタログから拡張機能を選択するか、検索バーを使用します。

リンク先をクリックしてハイライトし、右側のパネルで「**[!UICONTROL 設定]**」を選択します。 **[!UICONTROL Configure]**&#x200B;コントロールがグレー表示になっている場合は、**[!UICONTROL manage_properties]**&#x200B;権限がありません。 [前提条件](#prerequisites)を確認してください。

**[!UICONTROL 使用可能な プロパティを選択]**&#x200B;ウィンドウで、拡張機能をインストールする Launch プロパティを選択します。[!DNL Launch]また、Launch で新しいプロパティを作成することもできます。プロパティは、ルール、データ要素、設定された拡張機能、環境およびライブラリの集まりです。プロパティについては、 ドキュメントの[「プロパティ」ページに関する節](https://experienceleague.adobe.com/docs/launch/using/reference/admin/companies-and-properties.html#properties-page)を参照してください。[!DNL Launch]

このワークフローにより、[!DNL Launch]に移動してインストールを完了します。

拡張機能の設定オプションについて詳しくは、 ドキュメントの [Audience Manager 拡張機能のページ](https://experienceleague.adobe.com/docs/launch/using/extensions-ref/adobe-extension/adobe-audience-manager-extension.html)を参照してください。[!DNL Experience Launch]

[Adobe Experience Platform Launchインターフェイス](https://launch.adobe.com/)に直接インストールすることもできます。  ドキュメントの[新しい拡張機能の追加](https://experienceleague.adobe.com/docs/launch/using/reference/manage-resources/extensions/overview.html?lang=en#add-a-new-extension)に関するページを参照してください。[!DNL Platform Launch]

## 拡張機能の使用方法 {#how-to-use}

拡張機能をインストールすると、[!DNL Platform Launch]に直接ルールを設定する開始を作成できます。

[!DNL Platform Launch]では、特定の状況でのみイベントデータを拡張の宛先に送信するように、インストール済みの拡張機能のルールを設定できます。 拡張機能のルールの設定について詳しくは、[ルールに関するドキュメント](https://experienceleague.adobe.com/docs/launch/using/reference/manage-resources/rules.html)を参照してください。

## 拡張機能の設定、アップグレード、削除 {#configure-upgrade-delete}

[!DNL Platform Launch]インターフェイスでは、拡張機能の設定、アップグレード、削除が可能です。

>[!TIP]
>
>拡張機能が既にプロパティの1つにインストールされている場合、プラットフォームUIには、拡張機能の&#x200B;**[!UICONTROL Install]**&#x200B;が表示されます。 「[拡張機能のインストール](#install-extension)」の説明にしたがってインストールワークフローを開始し、 に移動して拡張機能を設定または削除します。[!DNL Platform Launch]

拡張機能をアップグレードするには、 ドキュメントの「[拡張機能のアップグレード](https://experienceleague.adobe.com/docs/launch/using/reference/manage-resources/extensions/extension-upgrade.html)」を参照してください。[!DNL Platform Launch]



