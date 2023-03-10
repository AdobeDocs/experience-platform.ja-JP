---
keywords: Experience Platform；はじめに；content ai;commerce ai;content tagging
solution: Experience Platform
title: コンテンツタグ付けの概要
description: コンテンツのタグ付けはAdobe I/OAPI を利用します。 Adobe I/OAPI と I/O コンソール統合を呼び出すには、まず認証に関するチュートリアルを完了する必要があります。
exl-id: e7b0e9bb-a1f1-479c-9e9b-46991f2942e2
source-git-commit: b124ed97da8bde2a7fc4f10d350c81a47e096f29
workflow-type: tm+mt
source-wordcount: '567'
ht-degree: 15%

---

# コンテンツタグ付けの概要

[!DNL Content tagging] はAdobe I/OAPI を利用します。 Adobe I/OAPI と I/O コンソール統合を呼び出すには、まず [認証チュートリアル](https://experienceleague.adobe.com/docs/experience-platform/landing/platform-apis/api-authentication.html?lang=ja).

ただし、 **API を追加** 手順を実行すると、API は、次のスクリーンショットに示すように、Adobe Experience PlatformではなくCreative Cloudの下に配置されます。

![コンテンツタグ付けの追加](./images/add-api-updated.png)

認証に関するチュートリアルを完了すると、すべての認証 API 呼び出しで必要な各Adobe I/Oの値が次のようになります。

- `Authorization: Bearer {ACCESS_TOKEN}`
- `x-api-key: {API_KEY}`
- `x-gw-ims-org-id: {ORG_ID}`

## Postman環境の作成（オプション）

Adobe Developerコンソール内でプロジェクトと API を設定したら、Postmanの環境ファイルをダウンロードするオプションがあります。 の下 **[!UICONTROL API]** プロジェクトの左側のパネルで、「 **[!UICONTROL コンテンツのタグ付け]**. 新しいタブが開き、「[!DNL Try it out]&quot;. 選択 **Postman用のダウンロード** をクリックして、postman 環境の設定に使用する JSON ファイルをダウンロードします。

![postman 用にダウンロード](./images/add-to-postman-updated.png)

ファイルをダウンロードしたら、Postmanを開き、 **歯車アイコン** を右上に開いて **環境の管理** ダイアログ。

![歯車アイコン](./images/select-gear-icon.png)

次に、 **インポート** 内から **環境の管理** ダイアログ。

![インポート](./images/import-updated.png)

リダイレクトされ、コンピューターから環境ファイルを選択するように求められます。 前にダウンロードした JSON ファイルを選択し、「 」を選択します。 **開く** 環境を読み込みます。

![](./images/choose-your-file.png)

![](./images/click-open.png)

次のページにリダイレクトされます： *環境の管理* タブに新しい環境名が入力されます。 環境名を選択して、Postmanで使用可能な変数を表示および編集します。 引き続き、 `JWT_TOKEN` および `ACCESS_TOKEN`. これらの値は、 [認証チュートリアル](https://experienceleague.adobe.com/docs/experience-platform/landing/platform-apis/api-authentication.html?lang=ja).

![](./images/re-direct-updated.png)

完了した変数は、次のスクリーンショットのようになります。 選択 **更新** をクリックして、環境の設定を完了します。

![](./images/final-environment-updated.png)

右上隅のドロップダウンメニューから環境を選択し、保存した値を自動入力できるようになりました。 いつでも値を再編集して、すべての API 呼び出しを更新できます。

![例](./images/select-environment-updated.png)

Postmanを使用したAdobe I/OAPI の操作について詳しくは、 [Adobe I/Oでの JWT 認証にPostmanを使用](https://medium.com/adobetech/using-postman-for-jwt-authentication-on-adobe-i-o-7573428ffe7f).

## API 呼び出し例の読み取り

ここでは、リクエストの形式を説明するために API 呼び出しの例を示します。これには、パス、必須ヘッダー、適切な形式のリクエストペイロードが含まれます。また、API レスポンスで返されるサンプル JSON も示されています。API 呼び出し例のドキュメントで使用される表記について詳しくは、『Experience Platform トラブルシューティングガイド』の [API 呼び出し例の読み方](../../landing/troubleshooting.md)に関する節を参照してください。

## 次の手順 {#next-steps}

すべての資格情報を取得したら、次の用にカスタムワーカーを設定する準備が整います。 [!DNL Content tagging]. 次のドキュメントは、拡張フレームワークと環境の設定の理解に役立ちます。

拡張フレームワークの詳細については、まず [拡張機能の概要](https://experienceleague.adobe.com/docs/asset-compute/using/extend/understand-extensibility.html?lang=ja) 文書。 このドキュメントでは、前提条件とプロビジョニング要件の概要を説明します。

の環境の設定について詳しくは、以下を参照してください。 [!DNL Content tagging]を参照し、まず [開発環境の設定](https://experienceleague.adobe.com/docs/asset-compute/using/extend/setup-environment.html). このドキュメントでは、Service Service の開発に使用する設定手順をAsset computeします。
