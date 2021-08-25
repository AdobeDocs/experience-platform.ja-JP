---
keywords: Experience Platform；ホーム；人気のあるトピック；Adobe Experience Platform;apiガイド；プラットフォームapiガイド；プラットフォームの概要；開発者ガイド
solution: Experience Platform
title: Adobe Experience Platformの郵便配達人
topic-legacy: api guide
description: このドキュメントでは、Postman環境の設定方法、Postmanコレクションの読み込み方法、および各Platformサービスで使用可能なコレクションのリストを説明する手順を説明します。
exl-id: a09b3875-97f5-47f1-a562-52decbce67b1
source-git-commit: a0f4e49192a54075ce7c48620c9729e61ecdfdac
workflow-type: tm+mt
source-wordcount: '550'
ht-degree: 1%

---

# Adobe Experience Platformの郵便配達人

Postmanは、API開発の共同作業プラットフォームで、プリセット変数を使用して環境を設定したり、APIコレクションを共有したり、CRUDリクエストを合理化したりできます。 ほとんどのPlatform APIサービスには、API呼び出しの実行に役立つPostmanコレクションがあります。

## Experience Platform用のPostman環境の設定方法

次のビデオガイドでは、Postman環境の作成と設定の概要を説明します。 Postman環境には、以下に示す様々なコレクションに対してAPI呼び出しをおこなう必要のあるすべてのヘッダーが含まれています。 設定が完了したら、いつでも値の有効期限が切れる（`ACCESS_TOKEN`など）ようになれば、環境内の現在の値を更新でき、この新しい値がすべてのコレクションで使用されます。

>[!VIDEO](https://video.tv.adobe.com/v/28832)

## Postmanコレクション {#collections}

使用可能なすべてのPostmanExperience Platformを含むフォルダーは、 [PostmanサンプルGitHubリポジトリ](https://github.com/adobe/experience-platform-postman-samples/tree/master/apis/experience-platform)にアクセスして、で見つけることができます。 または、Adobe I/O上の[APIリファレンスドキュメント](https://www.adobe.com/go/platform-api-reference-en)の各SwaggerファイルにPostmanコレクションリンクが含まれています。

Postmanコレクションをダウンロードするには、GitHubページから&#x200B;**[!DNL Raw]**&#x200B;を選択し、新しいタブに生のJSONファイルを読み込みます。 次に、右クリックして&#x200B;**[!DNL Save as]**&#x200B;を選択し、選択したローカルの宛先にファイルを保存します。

![生のJSON](./images/api-guide/raw-collection.PNG)

## Postmanコレクションの読み込み {#import}

[Postmanコレクション](#collections)を利用するには、環境を設定する必要があります。 環境の設定が完了したら、右上隅の&#x200B;**[!DNL Manage Environments]**&#x200B;セレクターを選択します。

![環境セレクターの管理](./images/api-guide/environment-selector.png)

ポップオーバーが表示され、現在の環境がすべて表示されます。 コレクションを読み込むには、「 **[!DNL import]** 」を選択します。

![インポートボタン](./images/api-guide/import-collection.png)

インポートするファイルを選択するよう求められます。 読み込むPostmanコレクションファイルを選択します。 選択すると、左側のレールの「コレクション」タブにコレクションが設定されます。

![収集](./images/api-guide/imported-collection.png)

各コレクションには、CRUD操作を正常に実行するために必要なキーと値のペアが異なる場合があります。 必要な値、ヒント、および例については、サービスの[API開発者ガイド](api-guide.md#api-guides)を参照してください。

Postman UIと使用可能な機能について詳しくは、[Postmanのドキュメント](https://learning.postman.com/docs/getting-started/navigating-postman/)を参照してください。

### 非実稼動用のPostmanでのアクセストークンの生成

>[!WARNING]
>
>Adobe I/Oアクセストークン生成のPostmanコレクションで述べたように、指定された生成方法は、**非実稼動環境での使用**&#x200B;に適しています。 ローカル署名は、サードパーティのホストからJavaScriptライブラリを読み込み、リモート署名は、Adobeが所有および操作するWebサービスに秘密鍵を送信します。 Adobeはこの秘密鍵を保存しませんが、プロダクションキーは誰とも共有しないでください。

以下のビデオでは、パブリックGitHubリポジトリからダウンロードできる[Adobe I/Oアクセストークン生成コレクション](https://github.com/adobe/experience-platform-postman-samples/blob/master/apis/ims/Adobe%20IO%20Access%20Token%20Generation.postman_collection.json)を使用しています。

>[!VIDEO](https://video.tv.adobe.com/v/29698/?quality=12&learn=on)

## 次の手順

このドキュメントでは、Postman環境、コレクション、コレクションの読み込み方法について説明しました。 Postmanの準備が整ったので、必要なヘッダー、例、各Platformサービスで使用できる[APIガイド](api-guide.md#api-guides)のリストについては、[Platform入門ガイド](api-guide.md)を参照してください。
