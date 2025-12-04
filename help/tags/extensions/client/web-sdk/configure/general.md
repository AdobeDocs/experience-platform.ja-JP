---
title: SDK インスタンスの設定
description: Web SDK インスタンスの一般設定を指定します。
source-git-commit: 09799847c61d82ed5b7cd372d92aa436697d54f3
workflow-type: tm+mt
source-wordcount: '250'
ht-degree: 3%

---

# SDK インスタンスの設定

この設定セクションでは、Web SDK インスタンス名、適用先の IMS 組織およびデータの送信先を管理します。 デフォルトでは、インスタンスの名前は `alloy` です。

1. Adobe IDの資格情報を使用して [experience.adobe.com](https://experience.adobe.com) にログインします。
1. **[!UICONTROL Data Collection]**／**[!UICONTROL Tags]**&#x200B;に移動します。
1. 目的のタグプロパティを選択します。
1. **[!UICONTROL Extensions]** に移動し、**[!UICONTROL Configure]** カードで [!UICONTROL Adobe Experience Platform Web SDK] を選択します。
1. 展開された [!UICONTROL SDK instances] アコーディオンのすぐ下にあるインスタンス名を見つけます。

![&#x200B; タグ UI の web SDK タグ拡張機能の一般設定を示す画像 &#x200B;](../assets/web-sdk-ext-general.png)

次のオプションがあります。

## [!UICONTROL Name]

Adobe Experience Platform Web SDK タグ拡張機能は、ページ上の複数のインスタンスをサポートします。 この名前は、Web SDKのタグライブラリを複製せずに、複数の組織にデータを送信する場合に使用します。 インスタンス名を任意の有効なJavaScript オブジェクト名に変更できます。

## [!UICONTROL IMS organization ID]

Adobeでデータを送信する組織の ID。 ほとんどの場合、自動入力されるデフォルト値を使用します。 ページ上に複数のインスタンスがある場合、データの送信先の 2 番目の組織の値をこのフィールドに入力します。

## [!UICONTROL Edge domain]

拡張機能がデータを送受信するドメイン。 `edge.adobedc.net` のデフォルト値は機能しますが、Adobeでは、ほとんどの場合、ファーストパーティドメインを使用することをお勧めします。 データ収集に適したファーストパーティドメインのセットアップ方法については [&#128279;](https://experienceleague.adobe.com/en/docs/core-services/interface/data-collection/adobe-managed-cert)Adobe管理の証明書プログラム &rbrace; を参照してください。 この値の設定に関するガイダンスについては、JavaScript ライブラリのドキュメントの [`edgeDomain`](/help/collection/js/commands/configure/edgedomain.md) も参照してください。
