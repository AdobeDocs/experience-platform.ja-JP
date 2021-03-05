---
keywords: Experience Platform；ホーム；人気の高いトピック；Adobe Experience Platform;apiガイド；プラットフォームapiガイド；プラットフォームの紹介；開発者ガイド
solution: Experience Platform
title: Adobe Experience Platformの郵便配達人
topic: apiガイド
description: このドキュメントでは、Postman環境の設定、Postmanコレクションの読み込み、各Platformサービスで使用可能なコレクションのリストの手順を説明します。
translation-type: tm+mt
source-git-commit: effc8fef666ffbf62c2e0874d048245f19c12111
workflow-type: tm+mt
source-wordcount: '552'
ht-degree: 0%

---


# Adobe Experience Platformの郵便配達人

PostmanはAPI開発のコラボレーションプラットフォームで、プリセット変数を使用した環境の設定、APIコレクションの共有、CRUDリクエストの合理化などを可能にします。 ほとんどのプラットフォームAPIサービスにはPostmanコレクションがあり、API呼び出しの作成に役立てるのに使用できます。

## Experience Platform用にポストマン環境を設定する方法

次のビデオガイドは、Postman環境の作成と設定の概要を説明しています。 Postman環境には、以下に示す様々なコレクションに対してAPI呼び出しを行うのに必要なすべてのヘッダーが含まれています。 設定が完了すると、値の有効期限（`ACCESS_TOKEN`など）が切れるたびに、環境内の現在の値を更新でき、この新しい値がすべてのコレクションで使用されます。

>[!VIDEO](https://video.tv.adobe.com/v/28832)

## ポストマンコレクション{#collections}

[Experience PlatformポストマンサンプルGitHubリポジトリ](https://github.com/adobe/experience-platform-postman-samples/tree/master/apis/experience-platform)にアクセスすると、利用可能なすべてのPostmanコレクションを含むフォルダーが見つかります。 または、Adobe I/O上の[APIリファレンスドキュメント](http://www.adobe.com/go/platform-api-reference-en)内の個々のSwaggerファイルにPostmanコレクションリンクがあります。

Postmanコレクションをダウンロードするには、GitHubページから&#x200B;**[!DNL Raw]**&#x200B;を選択して、新しいタブに生のJSONファイルを読み込みます。 次に、右クリックし、**[!DNL Save as]**&#x200B;を選択して、選択したローカルの保存先にファイルを保存します。

![raw JSON](./images/api-guide/raw-collection.PNG)

## Postmanコレクションを読み込む{#import}

[Postmanコレクション](#collections)を利用するには、環境を設定する必要があります。 環境の設定が完了したら、右上隅の&#x200B;**[!DNL Manage Environments]**&#x200B;セレクターを選択します。

![環境選択の管理](./images/api-guide/environment-selector.png)

ポーバーが表示され、現在の環境がすべて表示されます。 コレクションを読み込むには、**[!DNL import]**&#x200B;を選択します。

![読み込みボタン](./images/api-guide/import-collection.png)

読み込むファイルを選択するよう求められます。 読み込むPostmanコレクションファイルを選択します。 選択すると、左側のナビゲーションバーの「コレクション」タブの下にコレクションが設定されます。

![埋め込みコレクション](./images/api-guide/imported-collection.png)

コレクションごとに、CRUD操作を正常に実行するために必要なキーと値のペアが異なります。 必要な値、ヒント、および例を参照するには、サービスの[API開発者ガイド](api-guide.md#api-guides)を参照してください。

Postman UIと使用可能な機能について詳しくは、[Postmanドキュメント](https://learning.postman.com/docs/getting-started/navigating-postman/)を参照してください。

### 非実稼働用のPostmanを使用したアクセストークンの生成

>[!WARNING]
>
>Adobe I/Oアクセストークン生成ポストマンコレクションで述べられているように、示す生成方式は、**非実稼働での使用**&#x200B;に適しています。 ローカル署名では、サードパーティのホストからJavaScriptライブラリが読み込まれ、リモート署名では、Adobeが所有および操作するWebサービスに秘密鍵が送信されます。 Adobeはこの秘密鍵を保存しませんが、プロダクションキーを他のユーザーと共有することは避けてください。

以下のビデオでは、パブリックGitHubリポジトリからダウンロードできる[Adobe I/Oアクセストークン生成コレクション](https://github.com/adobe/experience-platform-postman-samples/blob/master/apis/ims/Adobe%20IO%20Access%20Token%20Generation.postman_collection.json)を使用しています。

>[!VIDEO](https://video.tv.adobe.com/v/29698/?quality=12&learn=on)

## 次の手順

このドキュメントでは、Postman環境、コレクション、およびコレクションの読み込み方法が導入されました。 Postmanの準備ができたので、[Platform getting started guide](api-guide.md)にアクセスし、必要なヘッダー、例、および各プラットフォームサービスで利用可能な[APIガイド](api-guide.md#api-guides)のリストを確認してください。