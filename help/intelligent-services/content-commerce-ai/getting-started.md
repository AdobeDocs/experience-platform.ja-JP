---
keywords: Experience Platform；はじめに；コンテンツ；コンテンツのタグ付け
solution: Experience Platform
title: コンテンツのタグ付けの概要
description: コンテンツのタグ付けには、Adobe I/O API が使用されます。 Adobe I/OAPI と I/O コンソール統合を呼び出すには、まず認証に関するチュートリアルを完了する必要があります。
exl-id: e7b0e9bb-a1f1-479c-9e9b-46991f2942e2
source-git-commit: a42bb4af3ec0f752874827c5a9bf70a66beb6d91
workflow-type: tm+mt
source-wordcount: '541'
ht-degree: 12%

---

# コンテンツのタグ付けの概要

[!DNL Content tagging] はAdobe I/OAPI を使用します。 Adobe I/OAPI と I/O コンソール統合を呼び出すには、まず [ 認証チュートリアル ](https://experienceleague.adobe.com/docs/experience-platform/landing/platform-apis/api-authentication.html?lang=ja) を完了する必要があります。

ただし、**API を追加** ステップに進むと、次のスクリーンショットに示すように、API はAdobe Experience PlatformではなくCreative Cloudの下にあります。

![ コンテンツのタグ付けの追加 ](./images/add-api-updated.png)

次に示すように、すべての認証 API 呼び出しで必要な各ヘッダーの値はAdobe I/Oチュートリアルで説明されています。

- `Authorization: Bearer {ACCESS_TOKEN}`
- `x-api-key: {API_KEY}`
- `x-gw-ims-org-id: {ORG_ID}`

## Postman環境の作成（オプション）

Adobe Developer Console内でプロジェクトと API を設定したら、Postmanの環境ファイルをダウンロードするオプションがあります。 プロジェクトの左側のパネルにある **[!UICONTROL API]** の下で **[!UICONTROL コンテンツのタグ付け]** を選択します。 「[!DNL Try it out]」というラベルの付いたカードを含む新しいタブが開きます。 **Postman用にダウンロード** を選択して、Postman 環境の設定に使用する JSON ファイルをダウンロードします。

![postman 用にダウンロード ](./images/add-to-postman-updated.png)

ファイルをダウンロードしたら、Postmanを開き、右上の **歯車アイコン** を選択して **環境の管理** ダイアログを開きます。

![ 歯車アイコン ](./images/select-gear-icon.png)

次に、**環境の管理** ダイアログ内から **読み込み** を選択します。

![ 読み込み ](./images/import-updated.png)

リダイレクトされ、コンピューターから環境ファイルを選択するように求められます。 前にダウンロードした JSON ファイルを選択し、「**開く**」を選択して環境を読み込みます。

![](./images/choose-your-file.png)

![](./images/click-open.png)

新しい環境名が入力された状態で *環境の管理* タブにリダイレクトされます。 Postmanで使用可能な変数を表示および編集するには、環境名を選択します。 `JWT_TOKEN` と `ACCESS_TOKEN` を手動で入力する必要があります。 これらの値は、[ 認証チュートリアル ](https://experienceleague.adobe.com/docs/experience-platform/landing/platform-apis/api-authentication.html?lang=ja) を完了する際に取得する必要があります。

![](./images/re-direct-updated.png)

完了すると、変数は次のスクリーンショットのようになります。 「**アップデート**」を選択して、環境の設定を完了します。

![](./images/final-environment-updated.png)

右上隅のドロップダウンメニューから環境を選択し、保存した値を自動入力できるようになりました。 任意の時点で値を再編集するだけで、すべての API 呼び出しを更新できます。

![例](./images/select-environment-updated.png)

Postmanを使用したAdobe I/OAPI の操作について詳しくは、Mediumの投稿 [Adobe I/Oでの JWT 認証にPostmanを使用 ](https://medium.com/adobetech/using-postman-for-jwt-authentication-on-adobe-i-o-7573428ffe7f) を参照してください。

## API 呼び出し例の読み取り

ここでは、リクエストの形式を説明するために API 呼び出しの例を示します。これには、パス、必須ヘッダー、適切な形式のリクエストペイロードが含まれます。また、API レスポンスで返されるサンプル JSON も示されています。API 呼び出し例のドキュメントで使用される表記について詳しくは、『Experience Platform トラブルシューティングガイド』の [API 呼び出し例の読み方](../../landing/troubleshooting.md)に関する節を参照してください。

## 次の手順 {#next-steps}

すべての資格情報を取得したら、[!DNL Content tagging] 用のカスタムワーカーを設定する準備が整います。 拡張フレームワークと環境のセットアップについては、次のドキュメントを参照してください。

拡張フレームワークの詳細については、まず [ 拡張機能の概要 ](https://experienceleague.adobe.com/docs/asset-compute/using/extend/understand-extensibility.html?lang=ja) を参照してください。 このドキュメントでは、前提条件とプロビジョニング要件の概要を説明します。

デベロッパー環境のセットアップについて詳しくは、[!DNL Content tagging] ベロッパー環境のセットアップ [ に関するガイドを参照して開始し ](https://experienceleague.adobe.com/docs/asset-compute/using/extend/setup-environment.html?lang=ja) ください。 このドキュメントでは、Asset computeサービス向けの開発を可能にする設定手順を示します。
