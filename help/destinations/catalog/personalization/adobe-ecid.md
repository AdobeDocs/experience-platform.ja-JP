---
Keywords: ECID;ecid
title: Experience Cloud ID サービス拡張機能
description: Experience Cloud ID サービス拡張機能は、Adobe Experience Platformのパーソナライゼーションの宛先です。 拡張機能について詳しくは、Adobe Exchange の拡張機能のページを参照してください。
exl-id: 4cc49c14-66ec-43e0-a106-70d9c3646d87
source-git-commit: d946d3dbb09c1fe0163fba3a892b4c0f1b331f87
workflow-type: tm+mt
source-wordcount: '444'
ht-degree: 30%

---

# [!DNL Experience Cloud] ID サービス拡張機能 {#adobe-ecid-extension}

## 概要 {#overview}

この拡張機能は[!DNL Experience Cloud] ID サービスを実装しており、すべての[!DNL Experience Cloud] ソリューションの訪問者を識別します。

[!DNL Experience Cloud] ID サービスは、[!DNL Adobe Experience Platform]のパーソナライゼーション拡張機能です。 拡張機能の機能について詳しくは、タグドキュメントの[Experience Cloud ID サービス拡張機能ページ ](../../../tags/extensions/client/id-service/overview.md)を参照してください。

この宛先はタグ拡張機能です。 Experience Platformでのタグ拡張機能の仕組みについて詳しくは、[ タグ拡張機能の概要](../launch-extensions/overview.md)を参照してください。

![Adobe ECID 拡張機能](../../assets/catalog/personalization/adobe-ecid/catalog.png)

## 前提条件 {#prerequisites}

この拡張機能は、Experience Platformを購入したすべてのユーザーの宛先カタログで利用できます。

この拡張機能を使用するには、Experience Platformのタグにアクセスする必要があります。 タグは、含まれている付加価値機能として[!DNL Adobe Experience Cloud]のお客様に提供されます。 組織管理者に連絡して、データ収集UIへのアクセス権を取得し、拡張機能をインストールできるように&#x200B;**[!UICONTROL manage_properties]**&#x200B;権限の付与を依頼してください。

## 拡張機能のインストール {#install-extension}

[!DNL Experience Cloud] ID サービス拡張機能をインストールするには：

[Experience Platform インターフェイス ](https://platform.adobe.com/)で、**[!UICONTROL Destinations]** > **[!UICONTROL Catalog]**&#x200B;に移動します。

カタログから拡張機能を選択するか、検索バーを使用します。

宛先を選択し、右側のパネルで「**[!UICONTROL Configure]**」を選択します。 **[!UICONTROL Configure]** コントロールがグレー表示されている場合、**[!UICONTROL manage_properties]**&#x200B;権限がありません。 [前提条件](#prerequisites)を確認してください。

拡張機能をインストールするタグプロパティを選択します。 また、新しいプロパティを作成するオプションもあります。プロパティは、ルール、データ要素、設定された拡張機能、環境およびライブラリの集まりです。プロパティについて詳しくは、[ タグのドキュメント ](../../../tags/ui/administration/companies-and-properties.md)を参照してください。

このワークフローでは、データ収集UIに移動して、インストールを完了します。

拡張機能の設定オプションとインストールのサポートについて詳しくは、タグドキュメントの[Experience Cloud ID サービス拡張機能ページ ](../../../tags/extensions/client/id-service/overview.md)を参照してください。

拡張機能は、[データ収集 UI](https://experience.adobe.com/#/data-collection/) で直接インストールできます。詳しくは、[新しい拡張機能の追加](../../../tags/ui/managing-resources/extensions/overview.md#add-a-new-extension)に関するガイドを参照してください。

## 拡張機能の使用方法 {#how-to-use}

拡張機能をインストールしたら、ルールの設定を開始できます。 データ収集UIでは、特定の状況でのみ拡張機能の宛先にイベントデータを送信するように、インストール済みの拡張機能のルールを設定できます。 拡張機能のルールの設定について詳しくは、タグのドキュメントの[ ルール ](../../../tags/ui/managing-resources/rules.md)の概要を参照してください。

## 拡張機能の設定、アップグレード、削除 {#configure-upgrade-delete}

データ収集 UI で、拡張機能の設定、アップグレードおよび削除を行うことができます。

>[!TIP]
>
>拡張機能が既にいずれかのプロパティにインストールされている場合でも、UIには拡張機能の&#x200B;**[!UICONTROL Install]**&#x200B;が表示されます。 [拡張機能のインストール](#install-extension)の説明に従ってインストールワークフローを開始し、拡張機能を設定または削除します。

拡張機能をアップグレードするには、[拡張機能のアップグレードプロセス](../../../tags/ui/managing-resources/extensions/extension-upgrade.md) （タグドキュメント）のガイドを参照してください。
