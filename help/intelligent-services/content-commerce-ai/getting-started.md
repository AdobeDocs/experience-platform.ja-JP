---
keywords: Experience Platform；はじめに；コンテンツai；コマースai；コンテンツとコマースai
solution: Experience Platform, Intelligent Services
title: コンテンツおよびコマースAIの概要
topic-legacy: Getting started
description: コンテンツとコマースAIはAdobe I/OAPIを利用します。 Adobe I/OAPIとI/Oコンソール統合を呼び出すには、まず認証に関するチュートリアルを完了する必要があります。
exl-id: e7b0e9bb-a1f1-479c-9e9b-46991f2942e2
source-git-commit: c3d66e50f647c2203fcdd5ad36ad86ed223733e3
workflow-type: tm+mt
source-wordcount: '592'
ht-degree: 14%

---

# コンテンツおよびコマースAIの概要

>[!NOTE]
>
>コンテンツおよびコマースAIはベータ版です。 このドキュメントは変更される場合があります。

[!DNL Content and Commerce AI] では、Adobe I/OAPIを利用します。Adobe I/OAPIとI/Oコンソール統合を呼び出すには、まず[認証に関するチュートリアル](https://experienceleague.adobe.com/docs/experience-platform/landing/platform-apis/api-authentication.html?lang=ja#platform-apis)を完了する必要があります。

ただし、**API**&#x200B;の追加の手順に進むと、次のスクリーンショットに示すように、APIはAdobe Experience PlatformではなくExperience Cloudの下に配置されます。

![コンテンツおよびコマースAIの追加](./images/add-api.png)

認証に関するチュートリアルを完了すると、以下に示すように、すべてのAdobe I/OAPI呼び出しで必要な各ヘッダーの値が提供されます。

- `Authorization: Bearer {ACCESS_TOKEN}`
- `x-api-key: {API_KEY}`
- `x-gw-ims-org-id: {IMS_ORG}`

## Postman環境の作成（オプション）

Adobe開発者コンソール内でプロジェクトとAPIを設定したら、Postman用の環境ファイルをダウンロードできます。 プロジェクトの左側のレールの「**[!UICONTROL API]**」で、「**[!UICONTROL コンテンツとコマースのAI]**」を選択します。 新しいタブが開き、「[!DNL Try it out]」というラベルのカードが表示されます。 Postman環境の設定に使用するJSONファイルをダウンロードするには、「**Postman**&#x200B;のダウンロード」を選択します。

![郵便人にダウンロードする](./images/add-to-postman.png)

ファイルをダウンロードしたら、Postmanを開き、右上の&#x200B;**ギアアイコン**&#x200B;を選択して、**環境を管理**&#x200B;ダイアログを開きます。

![歯車アイコン](./images/select-gear-icon.png)

次に、**環境の管理**&#x200B;ダイアログで「**読み込み**」を選択します。

![import](./images/import.png)

リダイレクトされ、コンピューターから環境ファイルを選択するように求められます。 前にダウンロードしたJSONファイルを選択し、「**開く**」を選択して環境を読み込みます。

![](./images/choose-your-file.png)

![](./images/click-open.png)

新しい環境名が入力され、「*環境を管理*」タブに戻ります。 環境名を選択して、Postmanで使用可能な変数を表示および編集します。 引き続き、`JWT_TOKEN`と`ACCESS_TOKEN`に手動で値を設定する必要があります。 これらの値は、[認証に関するチュートリアル](https://www.adobe.com/go/platform-api-authentication-en)の完了時に取得された値です。

![](./images/re-direct.png)

完了したら、変数は次のスクリーンショットのようになります。 **「**&#x200B;を更新」を選択して、環境の設定を完了します。

![](./images/final-environment.png)

右上隅のドロップダウンメニューから環境を選択し、保存した値を自動入力できるようになりました。 値をいつでも再編集して、すべてのAPI呼び出しを更新できます。

![例](./images/select-environment.png)

Postmanを使用したAdobe I/OAPIの操作について詳しくは、](https://medium.com/adobetech/using-postman-for-jwt-authentication-on-adobe-i-o-7573428ffe7f)Adobe I/OでのJWT認証にPostmanを使用した[のMediumの投稿を参照してください。

## API 呼び出し例の読み取り

ここでは、リクエストの形式を説明するために API 呼び出しの例を示します。これには、パス、必須ヘッダー、適切に書式設定されたリクエストペイロードが含まれます。また、API レスポンスで返されるサンプル JSON も示されています。ドキュメントで使用される API 呼び出し例の表記について詳しくは、Experience Platform トラブルシューテングガイドの[API 呼び出し例の読み方](../../landing/troubleshooting.md)に関する節を参照してください。

## 次の手順 {#next-steps}

すべての資格情報を取得したら、[!DNL Content and Commerce AI]のカスタムワーカーを設定する準備が整います。 次のドキュメントは、拡張機能フレームワークと環境の設定について理解するのに役立ちます。

拡張機能フレームワークの詳細については、まず「[拡張機能](https://experienceleague.adobe.com/docs/asset-compute/using/extend/understand-extensibility.html?lang=ja)の概要」ドキュメントを読んでください。 このドキュメントでは、前提条件とプロビジョニング要件について説明します。

[!DNL Content and Commerce AI]用の環境の設定について詳しくは、まず[開発環境の設定](https://experienceleague.adobe.com/docs/asset-compute/using/extend/setup-environment.html)のガイドを参照してください。 このドキュメントでは、Service用の開発を行うための設定手順をAsset computeします。
