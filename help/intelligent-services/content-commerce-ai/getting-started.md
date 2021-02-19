---
keywords: Experience Platform；はじめに；コンテンツai；コマースai；コンテンツとコマースai
solution: Experience Platform, Intelligent Services
title: コンテンツとコマースの使用の手引きAI
topic: Getting started
description: コンテンツ&コマースAIはAdobe I/OAPIを活用。 Adobe I/OAPIとI/Oコンソール統合を呼び出すには、まず認証のチュートリアルを完了する必要があります。
translation-type: tm+mt
source-git-commit: eb163949f91b0d1e9cc23180bb372b6f94fc951f
workflow-type: tm+mt
source-wordcount: '596'
ht-degree: 13%

---


# コンテンツとコマースのAI使用の手引き

>[!NOTE]
>
>コンテンツとコマースAIはベータ版です。 このドキュメントは変更されることがあります。

[!DNL Content and Commerce AI] Adobe I/OAPIを利用。Adobe I/OAPIとI/Oコンソール統合を呼び出すには、まず[認証チュートリアル](https://www.adobe.com/go/platform-api-authentication-en)を完了する必要があります。

ただし、**追加 API**&#x200B;の手順に進むと、次のスクリーンショットに示すように、APIはAdobe Experience PlatformではなくExperience Cloudの下に配置されます。

![コンテンツおよびコマースAIの追加](./images/add-api.png)

次に示すように、認証チュートリアルで、すべてのAdobe I/OAPI呼び出しに必要な各ヘッダーの値を指定します。

- `Authorization: Bearer {ACCESS_TOKEN}`
- `x-api-key: {API_KEY}`
- `x-gw-ims-org-id: {IMS_ORG}`

## ポストマン環境の作成（オプション）

Adobeデベロッパーコンソール内でプロジェクトとAPIを設定したら、Postman用の環境ファイルをダウンロードできます。 プロジェクトの左側のレールの&#x200B;**[!UICONTROL API]**&#x200B;の下で、「**[!UICONTROL コンテンツとコマースのAI]**」を選択します。 新しいタブが開き、「[!DNL Try it out]」というラベルの付いたカードが表示されます。 「**Postman**&#x200B;にダウンロード」を選択して、postman環境の設定に使用するJSONファイルをダウンロードします。

![郵便配達人にダウンロードする](./images/add-to-postman.png)

ファイルをダウンロードしたら、Postmanを開き、右上の&#x200B;**ギアアイコン**&#x200B;を選択して、「**環境を管理**」ダイアログを開きます。

![ギアアイコン](./images/select-gear-icon.png)

次に、**環境の管理**&#x200B;ダイアログ内で、「**読み込み**」を選択します。

![インポート](./images/import.png)

リダイレクトされ、コンピューターから環境ファイルを選択するように求められます。 前にダウンロードしたJSONファイルを選択し、「**開く**」を選択して環境を読み込みます。

![](./images/choose-your-file.png)

![](./images/click-open.png)

新しい環境名が入力され、「*環境の管理*」タブに戻ります。 表示する環境名を選択し、Postmanで使用可能な変数を編集します。 引き続き、`JWT_TOKEN`と`ACCESS_TOKEN`を手動で入力する必要があります。 これらの値は、[認証チュートリアル](https://www.adobe.com/go/platform-api-authentication-en)の完了時に取得する必要があります。

![](./images/re-direct.png)

完了すると、変数は下のスクリーンショットのようになります。 「**更新**」を選択して、環境の設定を終了します。

![](./images/final-environment.png)

右上隅のドロップダウンメニューから環境を選択して、保存した値を自動入力できるようになりました。 値をいつでも再編集して、すべてのAPI呼び出しを更新するだけです。

![example](./images/select-environment.png)

Postmanを使用したAdobe I/OAPIの操作について詳しくは、[Adobe I/OでのJWT認証にPostmanを使用した場合のMediumの投稿](https://medium.com/adobetech/using-postman-for-jwt-authentication-on-adobe-i-o-7573428ffe7f)を参照してください。

## API 呼び出し例の読み取り

ここでは、リクエストの形式を説明するために API 呼び出しの例を示します。これには、パス、必須ヘッダー、適切に書式設定されたリクエストペイロードが含まれます。また、API レスポンスで返されるサンプル JSON も示されています。ドキュメントで使用される API 呼び出し例の表記について詳しくは、Experience Platform トラブルシューテングガイドの[API 呼び出し例の読み方](../../landing/troubleshooting.md)に関する節を参照してください。

## 次の手順 {#next-steps}

資格情報をすべて取得したら、[!DNL Content and Commerce AI]のカスタムワーカーを設定する準備が整います。 次のドキュメントは、拡張機能のフレームワークと環境の設定を理解するのに役立ちます。

拡張機能のフレームワークについて詳しくは、[拡張機能](https://docs.adobe.com/content/help/ja-JP/asset-compute/using/extend/understand-extensibility.html)のドキュメントの概要を読んで開始してください。 このドキュメントでは、前提条件とプロビジョニング要件について概説します。

[!DNL Content and Commerce AI]用の環境の設定についての詳細は、[開発者環境](https://docs.adobe.com/content/help/en/asset-compute/using/extend/setup-environment.html)の設定ガイドを読んで開始を参照してください。 このドキュメントでは、Asset computeサービスの開発を可能にする設定手順を説明します。