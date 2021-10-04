---
keywords: Experience Platform；ホーム；人気のあるトピック；Adobe Experience Platform;api ガイド；プラットフォーム api ガイド；プラットフォームの概要；開発者ガイド
solution: Experience Platform
title: Adobe Experience Platformの郵便配達員
topic-legacy: api guide
description: このドキュメントでは、Postman 環境の設定方法、Postman コレクションの読み込み方法、および各 Platform サービスで使用可能なコレクションのリストを説明する手順を説明します。
exl-id: a09b3875-97f5-47f1-a562-52decbce67b1
source-git-commit: a0f4e49192a54075ce7c48620c9729e61ecdfdac
workflow-type: tm+mt
source-wordcount: '550'
ht-degree: 1%

---

# Adobe Experience Platformの郵便配達員

Postman は、API 開発のコラボレーションプラットフォームで、事前に設定された変数を使用して環境を設定し、API コレクションを共有し、CRUD リクエストを合理化するなどを可能にします。 ほとんどの Platform API サービスには、API 呼び出しの実行に役立つ Postman コレクションがあります。

## Experience Platform用の Postman 環境の設定方法

次のビデオガイドでは、Postman 環境の作成と設定の概要を説明します。 Postman 環境には、以下に示す様々なコレクションに対して API 呼び出しをおこなう必要のあるすべてのヘッダーが含まれています。 設定が完了すると、値の有効期限が切れるたびに（`ACCESS_TOKEN` など）環境の現在の値を更新でき、この新しい値がすべてのコレクションで使用されます。

>[!VIDEO](https://video.tv.adobe.com/v/28832)

## Postman コレクション {#collections}

使用可能なすべての Postman コレクションを含むExperience Platformーは、 [ フォルダー Postman サンプル GitHub リポジトリ ](https://github.com/adobe/experience-platform-postman-samples/tree/master/apis/experience-platform) にアクセスして、で見つかります。 または、Adobe I/Oの [API リファレンスドキュメント ](https://www.adobe.com/go/platform-api-reference-en) の各 Swagger ファイルに Postman コレクションリンクが含まれています。

Postman コレクションをダウンロードするには、GitHub ページから **[!DNL Raw]** を選択して、新しいタブに生の JSON ファイルを読み込みます。 次に、右クリックして **[!DNL Save as]** を選択し、選択したローカルの宛先にファイルを保存します。

![生の JSON](./images/api-guide/raw-collection.PNG)

## Postman コレクションの読み込み {#import}

[Postman コレクション ](#collections) を利用するには、環境を設定する必要があります。 環境の設定が完了したら、右上隅の **[!DNL Manage Environments]** セレクターを選択します。

![環境セレクターの管理](./images/api-guide/environment-selector.png)

ポップオーバーが表示され、現在の環境がすべて表示されます。 コレクションを読み込むには、「 **[!DNL import]** 」を選択します。

![読み込みボタン](./images/api-guide/import-collection.png)

インポートするファイルを選択するよう求められます。 読み込む Postman コレクションファイルを選択します。 選択すると、左側のレールの「コレクション」タブにコレクションが設定されます。

![収集](./images/api-guide/imported-collection.png)

各コレクションには、CRUD 操作を正常に実行するために必要なキーと値のペアが異なる場合があります。 必要な値、ヒント、および例については、サービスの [API 開発者ガイド ](api-guide.md#api-guides) を参照してください。

Postman UI と利用可能な機能について詳しくは、[Postman のドキュメント ](https://learning.postman.com/docs/getting-started/navigating-postman/) を参照してください。

### 非実稼動用の Postman でのアクセストークンの生成

>[!WARNING]
>
>Adobe I/Oアクセストークン生成の Postman コレクションで述べたように、指定された生成方法は、**非実稼動環境での使用** に適しています。 ローカル署名は、サードパーティのホストから JavaScript ライブラリを読み込み、リモート署名は、Adobeが所有および操作する Web サービスに秘密鍵を送信します。 Adobeはこの秘密鍵を保存しませんが、プロダクションキーは誰とも共有しないでください。

以下のビデオでは、[Adobe I/Oアクセストークン生成コレクション ](https://github.com/adobe/experience-platform-postman-samples/blob/master/apis/ims/Adobe%20IO%20Access%20Token%20Generation.postman_collection.json) を使用しています。このコレクションは、公開 GitHub リポジトリからダウンロードできます。

>[!VIDEO](https://video.tv.adobe.com/v/29698/?quality=12&learn=on)

## 次の手順

このドキュメントでは、Postman 環境、コレクション、コレクションの読み込み方法について説明しました。 Postman の準備が整ったら、[Platform 入門ガイド ](api-guide.md) を参照して、必要なヘッダー、例、各 Platform サービスで使用できる [API ガイド ](api-guide.md#api-guides) のリストを確認してください。
