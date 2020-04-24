---
keywords: Experience Platform;getting started;customer ai;popular topics
solution: Experience Platform
title: はじめに — お客様向けAI
topic: Getting started
translation-type: tm+mt
source-git-commit: f7c59ef097c00073fbf9f6522b6e70ed24cc8bf1

---


# はじめに — お客様向けAI

顧客AIのガイドでは、顧客AIの使用に関連する様々なプラットフォームサービスについて、実際に理解する必要があります。 開始を行う前に、次のドキュメントを確認します。

- [エクスペリエンスデータモデル(XDM)システム概要](../../xdm/home.md):XDMは、Experience Platformが提供するAdobe Experience Cloudが、適切なチャネルを適切なタイミングで適切な人に適切なメッセージを届けることを可能にする、基本的なフレームワークです。 Experience Platformが構築される方法、XDM Systemは、プラットフォームサービスで使用するエクスペリエンスデータモデルスキーマを運用します。
- [スキーマ構成の基本](../../xdm/schema/composition.md):このドキュメントでは、Experience Data Model(XDM)スキーマの概要と、Adobe Experience Platformで使用するスキーマを構成するための構成要素、原則およびベストプラクティスを紹介します。
- [スキーマ](../../xdm/tutorials/create-schema-ui.md):このチュートリアルでは、Experience Platform内でスキーマエディターを使用してスキーマを作成する手順を説明します。
- [リアルタイム顧客プロファイルの概要](../../rtcdp/overview.md):Adobe Experience Platformを基盤としたAdobe Real-time Customer Data Platform(Real-time CDP)は、会社が既知の未知のデータを集め、顧客の遍歴を通じてインテリジェントな判定を行い、顧客プロファイルをアクティブにするのに役立ちます。 Real-time CDP は、複数の大規模法人データソースを組み合わせて、1 対 1 のパーソナライズされた顧客体験をすべてのチャネルとデバイスにわたって提供するために使用できる、統合されたプロファイルを作成します。
- [Segmentation Serviceの概要](../../segmentation/home.md):セグメント化とは、マーケティング可能な人々のグループを顧客ベースと区別するために、プロファイルのサブセットによってプロファイルストアから共有される特定の属性や行動を定義するプロセスです。 例えば、「スニーカーを購入し忘れましたか？」という電子メールキャンペーンでは、過去 30 日間にランニングシューズを検索したが購入を完了しなかったすべてのユーザーのオーディエンスが必要な場合があります。様々なセグメントを使用すると、様々なオーディエンスに焦点を当て、よりカスタマイズされたマーケティングエクスペリエンスを提供できます。
- [セグメントビルダーユーザーガイド](../../segmentation/tutorials/create-a-segment.md):プラットフォームを使用すると、セグメントの作成とアクセスを簡単にし、異なる構築ブロックを使用してセグメントの特徴をさらに明確にすることができます。

## 顧客AIスコアのダウンロード

>[!NOTE] 生のスコアをダウンロードする必要がない場合は、この手順をスキップして設定ガイドに進むこと [ができます](./user-guide/configure.md)。

顧客AIスコアのダウンロードは、API呼び出しの組み合わせを通じて行われます。 プラットフォームAPIを呼び出すには、まず認証チュートリアルを完了する必要 [があります](../../tutorials/authentication.md)。 次に示すように、認証チュートリアルで、すべてのエクスペリエンスプラットフォームAPI呼び出しで必要な各ヘッダーの値を入力します。

- 認証：無記名 `{ACCESS_TOKEN}`
- x-api-key: `{API_KEY}`
- x-gw-ims-org-id: `{IMS_ORG}`

エクスペリエンスプラットフォームのすべてのリソースは、特定の仮想サンドボックスに分離されます。 プラットフォームAPIへのすべてのリクエストには、操作が行われるサンドボックスの名前を指定するヘッダーが必要です。

- x-sandbox-name: `{SANDBOX_NAME}`

>[!NOTE] プラットフォームのサンドボックスについて詳しくは、サンドボックスの概要ドキュメ [ントを参照してくださ](../../sandboxes/home.md)い。

### サンプルAPI呼び出しの読み取り

このガイドは、リクエストをフォーマットする方法を示すAPI呼び出しの例を提供します。 これには、パス、必須ヘッダー、適切にフォーマットされたリクエストペイロードが含まれます。 API応答で返されるサンプルJSONも提供されます。 サンプルAPI呼び出しのドキュメントで使用される表記について詳しくは、エクスペリエンスプラットフォームのトラブルシューテ [ィングガイドのAPI呼び出し例の読み方に関する節](../../landing/troubleshooting.md) （英語のみ）を参照してください。

## 次の手順

上記の手順を完了したら、 [Input and Output](./input-output.md) Documentationを参照してください。 このドキュメントでは、顧客AIで使用され、生成されるデータのタイプの概要を簡単に説明します。