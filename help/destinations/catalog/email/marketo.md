---
keywords: Marketo;marketo;Marketo拡張機能；marketo 拡張機能
title: Marketo 拡張機能
description: Marketo拡張機能は、Adobe Experience Platformの電子メールの宛先です。 拡張機能について詳しくは、Adobe Exchange の拡張機能のページを参照してください。
exl-id: 3841eb19-a17e-4c28-a101-7332d178af34
source-git-commit: b4e869f9bc29122db4fc66ccda752a50c7db729f
workflow-type: tm+mt
source-wordcount: '456'
ht-degree: 21%

---

# [!DNL Marketo] 拡張機能 {#marketo-extension}

## 概要 {#overview}

[!DNL Marketo]の強力なマーケティング自動化ソフトウェアは、マーケターがデジタルマーケティングの技術と科学を習得し、顧客や見込み客を惹きつけるのに役立ちます。

[!DNL Marketo] は、Adobe Experience Platformの電子メール拡張です。拡張機能について詳しくは、[Adobe Exchange の拡張機能ページ](https://exchange.adobe.com/experiencecloud.details.106250.bounteous-extension-for-adobe-launch-and-marketo.html)を参照してください。

この宛先はタグ拡張です。 Platform でのタグ拡張の動作について詳しくは、「[ タグ拡張の概要 ](../launch-extensions/overview.md)」を参照してください。

![Marketo 拡張機能](../../assets/catalog/email/marketo/catalog.png)

## 前提条件 {#prerequisites}

この拡張機能は、Platform を購入したすべてのお客様の [!DNL Destinations] カタログで利用できます。

この拡張機能を使用するには、Adobe Experience Platformのタグにアクセスする必要があります。 タグは、Adobe Experience Cloudのお客様に、付属の付加価値機能として提供されます。 組織の管理者に問い合わせてタグへのアクセス権を取得し、タグに拡張機能のインストールに必要な **[!UICONTROL manage_properties]** 権限を付与するよう依頼します。

## 拡張機能のインストール {#install-extension}

[!DNL Marketo] 拡張機能をインストールするには：

1. [Platform インターフェイス ](https://platform.adobe.com/) で、**[!UICONTROL 宛先]** / **[!UICONTROL カタログ]** に移動します。
2. カタログから拡張機能を選択するか、検索バーを使用します。
3. 宛先をクリックしてハイライトし、右側のレールで「**[!UICONTROL 設定]**」を選択します。 **[!UICONTROL Configure]** コントロールがグレー表示になっている場合は、**[!UICONTROL manage_properties]** 権限がありません。 [前提条件](#prerequisites)を確認してください。
4. 拡張機能をインストールするプロパティを選択します。 また、新しいプロパティを作成することもできます。 プロパティは、ルール、データ要素、設定された拡張機能、環境およびライブラリの集まりです。プロパティについては、タグのドキュメントの [ プロパティページの節 ](../../../tags/ui/administration/companies-and-properties.md#properties-page) を参照してください。
5. このワークフローでは、インストールを完了する手順を順を追って説明します。

拡張機能の設定オプションとインストールのサポートについて詳しくは、[Adobe Exchange の Marketo ページ](https://exchange.adobe.com/experiencecloud.details.106250.bounteous-extension-for-adobe-launch-and-marketo.html)を参照してください。

拡張機能は、[ データ収集 UI](https://experience.adobe.com/#/data-collection/) に直接インストールすることもできます。 詳しくは、タグのドキュメントの [ 新しい拡張子 ](../../../tags/ui/managing-resources/extensions/overview.md#add-a-new-extension) の追加に関する節を参照してください。

## 拡張機能の使用方法 {#how-to-use}

拡張機能をインストールしたら、ルールの設定を開始できます。

特定の状況でのみ拡張機能の宛先にイベントデータを送信するように、インストールした拡張機能のルールを設定できます。 拡張機能のルールの設定について詳しくは、[ タグのドキュメント ](../../../tags/ui/managing-resources/rules.md) を参照してください。

## 拡張機能の設定、アップグレード、削除 {#configure-upgrade-delete}

データ収集 UI で、拡張機能の設定、アップグレード、削除をおこなうことができます。

>[!TIP]
>
>拡張機能が既にいずれかのプロパティにインストールされている場合、Platform UI には、拡張機能の **[!UICONTROL Install]** が表示されます。 「[ 拡張機能のインストール ](#install-extension)」の説明に従って、インストールワークフローを開始し、拡張機能を設定または削除します。

拡張機能をアップグレードするには、タグのドキュメントの [ 拡張機能のアップグレードプロセス ](../../../tags/ui/managing-resources/extensions/extension-upgrade.md) のガイドを参照してください。
