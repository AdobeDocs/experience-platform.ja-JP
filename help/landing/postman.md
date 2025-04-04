---
keywords: Experience Platform；ホーム；人気のトピック；Adobe Experience Platform;api ガイド；platform api ガイド；platform の概要；開発者ガイド
solution: Experience Platform
title: Adobe Experience PlatformのPostman
description: このドキュメントには、Experience Platform環境のセットアップ、Postman コレクションのインポート、および各Postman サービスで使用可能なコレクションのリストを概要を示す手順が含まれています。
role: Developer
feature: API
exl-id: a09b3875-97f5-47f1-a562-52decbce67b1
source-git-commit: f129c215ebc5dc169b9a7ef9b3faa3463ab413f3
workflow-type: tm+mt
source-wordcount: '524'
ht-degree: 0%

---

# Adobe Experience PlatformのPostman

Postmanは、API 開発のコラボレーションプラットフォームで、プリセット変数の設定、API コレクションの共有、CRUD リクエストの効率化などを行うことができます。 ほとんどのExperience Platform API サービスには、API 呼び出しの実行を支援するために使用できるPostman コレクションがあります。

## Experience Platform用のPostman環境の設定方法

次のビデオガイドでは、Postman環境の作成とセットアップの概要を説明します。 Postman環境には、以下に提供される様々なコレクションに対する API 呼び出しを行うために必要なすべての必須ヘッダーが含まれています。 設定すると、値の有効期限（`ACCESS_TOKEN` など）が切れるたびに、環境の現在の値を更新でき、この新しい値がすべてのコレクションで使用されます。

>[!VIDEO](https://video.tv.adobe.com/v/28832)

## Postman コレクション {#collections}

使用可能なすべてのPostman コレクションを含んだフォルダーは、[Experience Platform Postman サンプル GitHub リポジトリ ](https://github.com/adobe/experience-platform-postman-samples/tree/master/apis/experience-platform) にアクセスすることで見つけることができます。 または、Adobe I/Oの [API リファレンスドキュメント ](https://www.adobe.com/go/platform-api-reference-en) に、個々の Swagger ファイルにPostman コレクションリンクが記載されています。

Postman コレクションをダウンロードするには、GitHub ページの **[!DNL Raw]** を選択して、生の JSON ファイルを新しいタブに読み込みます。 次に、右クリックして「**[!DNL Save as]**」を選択し、選択したローカルの保存先にファイルを保存します。

![ 生の JSON](./images/api-guide/raw-collection.PNG)

## Postman コレクションの読み込み {#import}

[Postman コレクション ](#collections) を利用するには、環境を設定する必要があります。 環境の設定が完了したら、右上隅の **[!DNL Manage Environments]** セレクターを選択します。

![ 環境セレクターを管理 ](./images/api-guide/environment-selector.png)

ポップオーバーが表示され、現在の環境がすべて表示されます。 コレクションを読み込むには、「**[!DNL import]**」を選択します。

![ 読み込みボタン ](./images/api-guide/import-collection.png)

読み込むファイルを選択するよう求められます。 読み込むPostman コレクションファイルを選択します。 選択すると、コレクションが「コレクション」タブの左パネルに表示されます。

![ 入力されたコレクション ](./images/api-guide/imported-collection.png)

各コレクションには、CRUD 操作を正常に実行するために必要になる可能性のある異なるキーと値のペアがあります。 必要な値、ヒント、例については、サービスの [API 開発者ガイド ](api-guide.md#api-guides) を参照してください。

Postman UI と使用可能な機能について詳しくは、[Postman ドキュメント ](https://learning.postman.com/docs/getting-started/navigating-postman/) を参照してください。

### 実稼動以外で使用するためのPostmanによるアクセストークンの生成

>[!WARNING]
>
>Identity Management サービス（IMS）のPostman コレクションで説明されているように、表記されている生成方法は **実稼動以外での使用** に適しています。 ローカル署名は、サードパーティホストからJavaScript ライブラリを読み込み、リモート署名は、Adobeが所有および運用する web サービスに秘密鍵を送信します。 Adobeはこの秘密鍵を保存しませんが、実稼働キーは誰とも共有しないでください。

以下のビデオでは、[Identity Management サービス（IMS）のPostman コレクションを使用しています。このコレクションは ](https://github.com/adobe/experience-platform-postman-samples/blob/master/apis/ims/Identity%20Management%20Service.postman_collection.json) 公開されている GitHub リポジトリからダウンロードできます。

>[!VIDEO](https://video.tv.adobe.com/v/29698/?quality=12&learn=on)

## 次の手順

このドキュメントでは、Postman環境、コレクションおよびコレクションの読み込み方法を紹介しました。 Postmanの準備が整ったら、[Experience Platform入門ガイドにアクセスして、必要なヘッダー、例 ](api-guide.md) および各Experience Platform サービスで使用可能な [API ガイド ](api-guide.md#api-guides) のリストを確認してください。
