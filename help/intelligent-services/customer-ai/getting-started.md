---
keywords: Experience Platform；はじめに；顧客 ai；人気のトピック
solution: Experience Platform, Real-Time Customer Data Platform
feature: Customer AI
title: 顧客 AI の基本を学ぶ
description: ここでは、リクエストの形式を説明するために API 呼び出しの例を示します。これには、パス、必須ヘッダー、適切な形式のリクエストペイロードが含まれます。
exl-id: 90c9a83a-8e66-4239-b2d6-2049a6319b25
source-git-commit: e52eb90b64ae9142e714a46017cfd14156c78f8b
workflow-type: tm+mt
source-wordcount: '570'
ht-degree: 58%

---

# はじめに — 顧客 AI

顧客 AI のガイドでは、顧客 AI の使用に関連する様々な Platform サービスに関する十分な知識が必要です。開始する前に、次のドキュメントを確認してください。

- [エクスペリエンスデータモデル（XDM）システムの概要](../../xdm/home.md):XDM は、次のことを可能にする基本フレームワークです [!DNL Adobe Experience Cloud]Experience Platformを活用し、適切なタイミング、適切なチャネルで適切な人物に適切なメッセージを配信します。 Experience Platform を構築する方法論である XDM システムによって、Platform サービスでエクスペリエンスデータモデルスキーマを操作できるようになります。
- [スキーマ構成の基本](../../xdm/schema/composition.md)：このドキュメントでは、エクスペリエンスデータモデル（XDM）スキーマの概要と、で使用するスキーマを構成するための構成要素、原則およびベストプラクティスを紹介します [!DNL Adobe Experience Platform].
- [スキーマの構築](../../xdm/tutorials/create-schema-ui.md)：このチュートリアルでは、Experience Platform 内でスキーマエディターを使用してスキーマを作成する手順を説明します。
- [リアルタイム顧客プロファイルの概要](../../rtcdp/overview.md)：にビルド [!DNL Adobe Experience Platform]Adobe Real-time Customer Data Platform（Real-Time CDP）は、企業が既知および未知のデータを統合し、カスタマージャーニーを通じてインテリジェントな意思決定によりカスタマープロファイルをアクティブ化するのに役立ちます。 Real-Time CDPでは、複数のエンタープライズデータソースを組み合わせて、統合されたプロファイルをリアルタイムで作成します。このプロファイルを使用して、すべてのチャネルとデバイスにわたって 1 対 1 のパーソナライズされたカスタマーエクスペリエンスを提供できます。
- [セグメント化サービスの概要](../../segmentation/home.md)：セグメント化は、プロファイルストアからのプロファイルのサブセットで共有される特定の属性や行動を定義して、マーケティング可能なユーザーグループを顧客ベースと区別するプロセスです。 例えば、「スニーカーを購入し忘れましたか？」というメールキャンペーンでは、過去 30 日間にランニングシューズを検索したが購入を完了しなかったすべてのユーザーのオーディエンスが必要な場合があります。様々なセグメントを使用すると、様々なオーディエンスに焦点を当て、よりカスタマイズされたマーケティングエクスペリエンスを提供できます。
- [セグメントビルダーユーザガイド](../../segmentation/tutorials/create-a-segment.md)：Platform では、セグメントを簡単に作成してアクセスでき、様々な構成要素を使用してセグメントをさらに特徴付けることができます。

## 顧客 AI スコアのダウンロード

>[!NOTE]
>
>生のスコアをダウンロードする必要がない場合は、この手順をスキップして、に進んでください [設定ガイド](./user-guide/configure.md).

顧客 AI スコアのダウンロードは、API 呼び出しの組み合わせを通じて実行されます。Platform API への呼び出しを実行する前に、[認証に関するチュートリアル](https://experienceleague.adobe.com/docs/experience-platform/landing/platform-apis/api-authentication.html?lang=ja)を完了する必要があります。認証に関するチュートリアルを完了すると、すべての Experience Platform API 呼び出しで使用する、以下のような各必須ヘッダーの値が提供されます。

- Authorization: Bearer `{ACCESS_TOKEN}`
- x-api-key： `{API_KEY}`
- x-gw-ims-org-id: `{ORG_ID}`

Experience Platform のすべてのリソースは、特定の仮想サンドボックスに分離されています。Platform API へのすべてのリクエストには、操作がおこなわれるサンドボックスの名前を指定するヘッダーが必要です。

- x-sandbox-name: `{SANDBOX_NAME}`

>[!NOTE]
>
>Platform のサンドボックスについて詳しくは、[サンドボックスの概要に関するドキュメント](../../sandboxes/home.md)を参照してください。

### API 呼び出し例の読み取り

ここでは、リクエストの形式を説明するために API 呼び出しの例を示します。これには、パス、必須ヘッダー、適切な形式のリクエストペイロードが含まれます。また、API レスポンスで返されるサンプル JSON も示されています。API 呼び出し例のドキュメントで使用される表記について詳しくは、『Experience Platform トラブルシューティングガイド』の [API 呼び出し例の読み方](../../landing/troubleshooting.md)に関する節を参照してください。

## 次の手順

上記のドキュメントに記載されている手順を完了したら、 [入出力](./data-requirements.md) ドキュメント。 このドキュメントでは、顧客 AI でどのようなタイプのデータが使用および生成されるかについて簡単に説明します。
