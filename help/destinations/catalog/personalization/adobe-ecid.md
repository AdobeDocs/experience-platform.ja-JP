---
Keywords: ECID;ecid
title: Experience Cloud ID サービス拡張機能
description: Experience CloudIDサービス拡張は、Adobe Experience Platformのパーソナライゼーションの宛先です。 拡張機能について詳しくは、Adobe Exchange の拡張機能のページを参照してください。
exl-id: 4cc49c14-66ec-43e0-a106-70d9c3646d87
source-git-commit: 7e27735697882065566ebdeccc36998ec368e404
workflow-type: tm+mt
source-wordcount: '462'
ht-degree: 14%

---

# [!DNL Experience Cloud] IDサービス拡張機能 {#adobe-ecid-extension}

## 概要 {#overview}

この拡張機能は、すべての[!DNL Experience Cloud]ソリューションをまたいで訪問者を識別する[!DNL Experience Cloud] IDサービスを実装します。

[!DNL Experience Cloud] IDサービスは、Adobe Experience Platformのパーソナライズ機能の拡張機能です。Experience Cloud機能について詳しくは、タグのドキュメントの[タグIDサービス拡張のページ](../../../tags/extensions/web/id-service/overview.md)を参照してください。

この宛先はタグ拡張です。 Platformでのタグ拡張の動作について詳しくは、「[タグ拡張の概要](../launch-extensions/overview.md)」を参照してください。

![Adobe ECID 拡張機能](../../assets/catalog/personalization/adobe-ecid/catalog.png)

## 前提条件 {#prerequisites}

この拡張機能は、Platformを購入したすべての顧客の宛先カタログで利用できます。

この拡張機能を使用するには、Platformのタグにアクセスする必要があります。 タグは、Adobe Experience Cloudのお客様に、付属の付加価値機能として提供されます。 組織の管理者に連絡してデータ収集UIへのアクセス権を取得し、拡張機能をインストールするための&#x200B;**[!UICONTROL manage_properties]**&#x200B;権限を付与するよう管理者に依頼します。

## 拡張機能のインストール {#install-extension}

[!DNL Experience Cloud] IDサービス拡張機能をインストールするには：

[Platformインターフェイス](http://platform.adobe.com/)で、**[!UICONTROL 宛先]**/**[!UICONTROL カタログ]**&#x200B;に移動します。

カタログから拡張機能を選択するか、検索バーを使用します。

宛先をクリックしてハイライトし、右側のレールで「**[!UICONTROL 設定]**」を選択します。 **[!UICONTROL Configure]**&#x200B;コントロールがグレー表示になっている場合は、**[!UICONTROL manage_properties]**&#x200B;権限がありません。 [前提条件](#prerequisites)を確認してください。

拡張機能をインストールするタグプロパティを選択します。 また、新しいプロパティを作成することもできます。 プロパティは、ルール、データ要素、設定された拡張機能、環境およびライブラリの集まりです。[タグのドキュメント](../../../tags/ui/administration/companies-and-properties.md)のプロパティについて説明します。

ワークフローに従って、データ収集UIが表示され、インストールが完了します。

拡張機能の設定オプションとインストールのサポートについて詳しくは、Experience Cloudドキュメントの[タグIDサービス拡張機能のページ](../../../tags/extensions/web/id-service/overview.md)を参照してください。

拡張機能は、[データ収集UI](https://experience.adobe.com/#/data-collection/)に直接インストールすることもできます。 詳しくは、[新しい拡張機能](../../../tags/ui/managing-resources/extensions/overview.md#add-a-new-extension)の追加に関するガイドを参照してください。

## 拡張機能の使用方法 {#how-to-use}

拡張機能をインストールしたら、ルールの設定を開始できます。 データ収集UIでは、特定の状況でのみ拡張機能の宛先にイベントデータを送信するように、インストール済みの拡張機能のルールを設定できます。 拡張機能のルールの設定について詳しくは、タグドキュメントの[rules](../../../tags/ui/managing-resources/rules.md)の概要を参照してください。

## 拡張機能の設定、アップグレード、削除 {#configure-upgrade-delete}

データ収集UIで、拡張機能の設定、アップグレード、削除をおこなうことができます。

>[!TIP]
>
>拡張機能が既にいずれかのプロパティにインストールされている場合、UIには拡張機能の&#x200B;**[!UICONTROL Install]**&#x200B;が表示されます。 「[拡張機能のインストール](#install-extension)」の説明に従って、拡張機能を設定または削除するインストールワークフローを開始します。

拡張機能をアップグレードするには、タグのドキュメントの[拡張機能のアップグレードプロセス](../../../tags/ui/managing-resources/extensions/extension-upgrade.md)のガイドを参照してください。
