---
keywords: Experience Platform；ホーム；人気の高いトピック；Adobe Experience Platform;api ガイド；platform api ガイド；platform の概要；開発者ガイド
solution: Experience Platform
title: Adobe Experience PlatformのPostman
topic-legacy: api guide
description: このドキュメントでは、Postman環境の設定方法、Postmanコレクションの読み込み方法、および各 Platform サービスで使用可能なコレクションのリストの概要を説明する手順について説明します。
exl-id: a09b3875-97f5-47f1-a562-52decbce67b1
source-git-commit: 9f00bff31f9e7d2da1294d3d1f24cba7870a4614
workflow-type: tm+mt
source-wordcount: '550'
ht-degree: 1%

---

# Adobe Experience PlatformのPostman

Postmanは、API 開発のコラボレーションプラットフォームで、プリセット変数を使用して環境を設定し、API コレクションを共有し、CRUD リクエストを合理化するなどのことができます。 ほとんどの Platform API サービスにはPostmanコレクションがあり、API 呼び出しの実行に役立ちます。

## Experience Platform用の Postman 環境の設定方法

次のビデオガイドでは、Postman環境の作成と設定の概要を説明します。 Postman環境には、以下に示す様々なコレクションに対して API 呼び出しをおこなう必要のあるすべてのヘッダーが含まれています。 設定後は、値の有効期限が切れるたびに ( `ACCESS_TOKEN`) 環境内の現在の値を更新できます。この新しい値は、すべてのコレクションで使用されます。

>[!VIDEO](https://video.tv.adobe.com/v/28832)

## Postman コレクション {#collections}

使用可能なすべてのPostmanコレクションを含むフォルダーは、次の URL にアクセスすることで、 [Experience PlatformPostmanサンプル GitHub リポジトリ](https://github.com/adobe/experience-platform-postman-samples/tree/master/apis/experience-platform). または、Postmanのコレクションリンクは、個々の Swagger ファイル ( [API リファレンスドキュメント](https://www.adobe.com/go/platform-api-reference-en) Adobe I/O

Postmanコレクションをダウンロードするには、 **[!DNL Raw]** GitHub ページから、生の JSON ファイルを新しいタブに読み込みます。 次に、右クリックして「 」を選択します。 **[!DNL Save as]** をクリックして、任意のローカルの宛先にファイルを保存します。

![生の JSON](./images/api-guide/raw-collection.PNG)

## Postmanコレクションの読み込み {#import}

を利用するために [Postman コレクション](#collections)を設定するには、環境を設定する必要があります。 環境の設定が完了したら、 **[!DNL Manage Environments]** セレクターを使用できます。

![環境セレクターを管理](./images/api-guide/environment-selector.png)

ポップオーバーが表示され、現在の環境がすべて表示されます。 コレクションを読み込むには、「 **[!DNL import]** .

![「インポート」ボタン](./images/api-guide/import-collection.png)

インポートするファイルを選択するよう求められます。 読み込むPostmanコレクションファイルを選択します。 選択すると、左側のレールの「コレクション」タブの下にコレクションが設定されます。

![入力されたコレクション](./images/api-guide/imported-collection.png)

各コレクションには異なるキーと値のペアがあり、CRUD 操作を正常に実行するために必要になる場合があります。 サービスの [API 開発者ガイド](api-guide.md#api-guides) 必要な値、ヒント、および例を参照してください。

Postman UI と使用可能な機能について詳しくは、 [Postmanドキュメント](https://learning.postman.com/docs/getting-started/navigating-postman/).

### 実稼動以外で使用するためのPostmanでのアクセストークンの生成

>[!WARNING]
>
>Adobe I/Oアクセストークン生成のPostmanコレクションで述べたように、指定された生成方法が **非実稼動用**. ローカル署名は、サードパーティのホストから JavaScript ライブラリを読み込み、リモート署名は、Adobeが所有および操作する Web サービスに秘密鍵を送信します。 Adobeはこの秘密鍵を保存しませんが、実稼働鍵は誰とも共有しないでください。

以下のビデオでは、 [Adobe I/Oアクセストークン生成コレクション](https://github.com/adobe/experience-platform-postman-samples/blob/master/apis/ims/Adobe%20IO%20Access%20Token%20Generation.postman_collection.json) 公開 GitHub リポジトリからダウンロードできます。

>[!VIDEO](https://video.tv.adobe.com/v/29698/?quality=12&learn=on)

## 次の手順

このドキュメントでは、Postman環境、コレクション、コレクションの読み込み方法について説明しました。 Postmanの準備が整ったら、 [Platform 入門ガイド](api-guide.md) を参照してください。 [API ガイド](api-guide.md#api-guides) は、各 Platform サービスで使用できます。
