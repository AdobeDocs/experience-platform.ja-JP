---
keywords: Experience Platform;getting started;customer ai;popular topics
solution: Experience Platform
title: お客様向けAIはじめに
topic: Getting started
translation-type: tm+mt
source-git-commit: bd9884a24c5301121f30090946ab24d9c394db1b
workflow-type: tm+mt
source-wordcount: '532'
ht-degree: 15%

---


# お客様向けAIはじめに

お客様向けAIのガイドでは、お客様向けAIの使用に関連する様々なPlatformサービスについて十分に理解している必要があります。 開始を行う前に、次のドキュメントを確認してください。

- [Experience Data Model(XDM)システム概要](../../xdm/home.md): XDMは、Experience Platformを利用し [!DNL Adobe Experience Cloud]て適切なメッセージを適切な人に、適切なチャネルに、ちょうど適切なタイミングで配信できる基本的なフレームワークです。 Experience Platformを構築する方法論、XDM Systemは、Platformサービスで使用するExperience Data Modelスキーマを運用します。
- [スキーマ構成の基本](../../xdm/schema/composition.md): このドキュメントでは、Experience Data Model(XDM)スキーマと、で使用するスキーマを構成するための構成要素、原則およびベストプラクティスを紹介 [!DNL Adobe Experience Platform]します。
- [スキーマの作成](../../xdm/tutorials/create-schema-ui.md): このチュートリアルでは、Experience Platform内でスキーマエディタを使用してスキーマを作成する手順を説明します。
- [リアルタイム顧客プロファイルの概要](../../rtcdp/overview.md): 基盤として構築され [!DNL Adobe Experience Platform]た [!DNL Adobe Real-time Customer Data Platform] （リアルタイムCDP）により、会社は既知の未知のデータを統合し、お客様の遍歴を通じてインテリジェントな判定を行い、お客様のプロファイルをアクティブ化できます。 Real-time CDP は、複数の大規模法人データソースを組み合わせて、1 対 1 のパーソナライズされた顧客体験をすべてのチャネルとデバイスにわたって提供するために使用できる、統合されたプロファイルを作成します。
- [Segmentation Serviceの概要](../../segmentation/home.md): 分類とは、マーケティング可能な人々のグループを顧客ベースから区別するために、プロファイルのサブセットによってプロファイルストアから共有される特定の属性や行動を定義するプロセスです。 例えば、「スニーカーを購入し忘れましたか？」という電子メールキャンペーンでは、過去 30 日間にランニングシューズを検索したが購入を完了しなかったすべてのユーザーのオーディエンスが必要な場合があります。様々なセグメントを使用すると、様々なオーディエンスに焦点を当て、よりカスタマイズされたマーケティングエクスペリエンスを提供できます。
- [セグメントビルダーユーザーガイド](../../segmentation/tutorials/create-a-segment.md): Platformを使用すると、セグメントを簡単に作成してアクセスでき、異なる構築ブロックを使用してセグメントの特徴をさらに明確にできます。

## 顧客AIスコアのダウンロード

>[!NOTE]
>
>生のスコアをダウンロードする必要がない場合は、この手順をスキップして [設定ガイドに進み](./user-guide/configure.md)ます。

顧客AIスコアのダウンロードは、API呼び出しの組み合わせによって行われます。 PlatformAPIを呼び出すには、まず [認証チュートリアルを完了する必要があります](../../tutorials/authentication.md)。 次に示すように、Experience PlatformAPIのすべての呼び出しに必要な各ヘッダーの値を認証チュートリアルで説明します。

- 認証： 無記名 `{ACCESS_TOKEN}`
- x-api-key: `{API_KEY}`
- x-gw-ims-org-id: `{IMS_ORG}`

Experience Platform内のすべてのリソースは、特定の仮想サンドボックスに分離されます。 PlatformAPIへのすべてのリクエストには、操作が実行されるサンドボックスの名前を指定するヘッダーが必要です。

- x-sandbox-name: `{SANDBOX_NAME}`

>[!NOTE]
>
>Platform内のサンドボックスについて詳しくは、「 [Sandboxの概要に関するドキュメント](../../sandboxes/home.md)」を参照してください。

### サンプルAPI呼び出しの読み取り

このガイドは、リクエストをフォーマットする方法を示すAPI呼び出しの例を提供します。 例えば、パス、必須のヘッダー、適切にフォーマットされた要求ペイロードなどです。 API応答で返されるサンプルJSONも提供されます。 サンプルAPI呼び出しのドキュメントで使用される規則について詳しくは、Experience PlatformトラブルシューティングガイドのAPI呼び出し例 [の読み方に関する節](../../landing/troubleshooting.md) を参照してください。

## 次の手順

上記のドキュメントで概要を説明した手順を完了したら、 [Input and Output](./input-output.md) Documentationを参照してください。 このドキュメントでは、Customer AIで使用され、生成されるデータのタイプについて簡単に説明します。