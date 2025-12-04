---
title: Adobe Experience Platform Assuranceの使用
description: このガイドでは、インストールおよび実装されたAdobe Experience Platform Assuranceの使用方法について説明します。
exl-id: 872c83d1-82e8-40d8-9b66-3e51a91a955f
source-git-commit: f8576e7f7e1da7351f7860cba27d5d09d0161132
workflow-type: tm+mt
source-wordcount: '674'
ht-degree: 3%

---

# Adobe Experience Platform Assuranceの使用

このチュートリアルでは、Adobe Experience Platform Assuranceの使用方法を説明します。 Assurance拡張機能のインストールおよび実装方法については、[Adobe Experience Platform Assurance拡張機能の実装 &#x200B;](./implement-assurance.md) に関するチュートリアルを参照してください。

## セッションを作成

[Assurance UI](https://experience.adobe.com/assurance) にログインしたら、「**[!UICONTROL Create Session]**」を選択してセッションの作成を開始できます。

![&#x200B; 「セッションを作成」ボタンがハイライト表示され、セッションを作成できる場所が示されている様子。](./images/using-assurance/create-session.png)

**[!UICONTROL Create New Session]** ダイアログが開き、セッションを作成するための 2 つのオプションが表示されます。

### ディープリンク接続

このオプションを選択すると、一意のセッション URL、QR コードおよび PIN が生成されます。 QR コードをスキャンするか、アプリでセッションリンクを手動で開き、PIN を入力して接続を確立します。

「**[!UICONTROL Deep link connect]**」を選択し、「**[!UICONTROL Start]**」を選択して続行します。

![&#x200B; 「ディープリンク接続」オプションが選択されていることを示す「新しいセッションを作成」ダイアログ &#x200B;](./images/using-assurance/create-new-session-deep-link.png)

セッションを識別する名前を入力し、**[!UICONTROL Base URL]** （アプリのディープリンク URL）を入力できるようになりました。 これらの詳細を入力したら、「**[!UICONTROL Next]**」を選択します。

>[!INFO]
>
>ベース URL は、URL からアプリを起動するために使用されるルート定義です。 セッション URL が生成されます。この URL により、Assurance セッションを開始できます。 値の例は次のようになります。`myapp://default` 「**[!UICONTROL Base URL]**」フィールドに、アプリのベースのディープリンク定義を入力します。

![&#x200B; セッション名およびベース URL 入力フィールドが表示されます。](./images/using-assurance/create-session-form-deep-link.png)

### クイック接続

アプリから接続をトリガーすると、利用可能なデバイスのリストにデバイスが表示されます。 「**[!UICONTROL Quick connect]**」を選択して、Assurance セッションを作成します。

#### 前提条件

Quick Connect を使用する前に、アプリに必要なSDKのバージョンと実装があることを確認します。

**Android SDKの要件：**

- Mobile Core v3.1.0 以降
- Adobe Journey Optimizer v3.1.0 以降
- Adobe Experience Platform Assurance v3.0.4 以降

**iOS SDKの要件：**

- Mobile Core v5.2.0 以降
- Adobe Journey Optimizer v5.1.1 以降
- Adobe Experience Platform Assurance v5.0.0 以降

**実装：**

Assurance接続をトリガーするには [`startSession` アプリに &#x200B;](https://developer.adobe.com/client-sdks/home/base/assurance/api-reference/#startsession-quick-connect) API を実装する必要があります。 この API 呼び出しは、通常、アプリ内のアクションセットに含まれているか、トリガーされます。

#### クイック接続セッションの作成

![&#x200B; 「クイック接続」オプションが選択されていることを示す「新規セッションを作成」ダイアログ &#x200B;](./images/using-assurance/create-new-session-quick-connect.png)

**[!UICONTROL Quick connect]** を選択し、**[!UICONTROL Start]** を選択して続行すると、デバイスピッカーインターフェイスが表示されます。

1. **Assurance接続のトリガー** - モバイルアプリまたは実装で、`startSession` API を使用してAssurance接続を開始するアクションをトリガーします。 これにより、デバイスが検出可能になります。

2. **デバイスを選択して接続** – 使用可能なデバイスのリストにデバイスが表示されたら、そのデバイスを選択して「**[!UICONTROL Connect]**」をクリックします。

![&#x200B; 使用可能なデバイスを表示するクイック接続デバイスピッカーインターフェイス &#x200B;](./images/using-assurance/quick-connect-device-picker.png)

## セッションへの接続

接続する手順は、使用しているセッションタイプによって異なります。

### ディープリンク接続セッション

**[!UICONTROL Deep Link Connect]** で作成されたセッションの場合：

1. （既存のセッションの）セッション詳細ページに移動するか、セッションの作成を進めて、リンク、QR コード、PIN を確認します
2. デバイスのカメラアプリで QR コードをスキャンするか、リンクをコピーしてアプリで開きます
3. アプリが起動すると、PIN 入力画面がオーバーレイ表示されます。 PIN を入力し、**[!UICONTROL Connect]** キーを押します

![Assurance セッションへの接続オプションを示すダイアログが表示されます。](./images/using-assurance/deep-link-connection.png)

### クイック接続セッション

**[!UICONTROL Quick Connect]** で作成されたセッション（`adobeassurance://` で始まるセッション URL で識別できる）の場合、接続はデバイスピッカーインターフェイスを通じて自動的に行われます。

1. （既存のセッションの）セッション詳細ページに移動するか、セッションの作成から進みます
2. **[!UICONTROL Connect Device]** セクションには、デバイスピッカーインターフェイスが表示されます
3. デバイスを検出可能にするには、アプリ内のアクションセットをトリガーします
4. リストからデバイスを選択し、「**[!UICONTROL Connect]**」をクリックします

![&#x200B; 接続できるデバイスを表示するデバイスピッカーインターフェイス &#x200B;](./images/using-assurance/quick-connect-device-picker.png)

### 接続の確認

Adobe Experience Platform アイコン（赤いAdobe「A」）がアプリに表示されると、アプリがAssuranceに接続されていることを確認できます。

## セッションのエクスポート

Assurance セッションを書き出すには、アプリのセッションの詳細ページで、セッションの「**[!UICONTROL Export to JSON]**」を選択します。

![&#x200B; セッションのエクスポート &#x200B;](./images/using-assurance/export-session.png)

書き出しオプションは、検索フィルターの結果に従い、イベント表示に表示されるイベントのみを書き出します。 例えば、「track」イベントを検索してを選択した場合、「track」イベントの結果のみ **[!UICONTROL Export to JSON]** 書き出されます。