---
keywords: Experience Platform；はじめに；お客様向けai；人気の高いトピック
solution: Experience Platform, Intelligent Services, Real-time Customer Data Platform
title: お客様向けAIはじめに
topic-legacy: Getting started
description: ここでは、リクエストの形式を説明するために API 呼び出しの例を示します。この中には、パス、必須ヘッダー、適切な形式のリクエストペイロードが含まれます。
exl-id: 90c9a83a-8e66-4239-b2d6-2049a6319b25
translation-type: tm+mt
source-git-commit: 5d449c1ca174cafcca988e9487940eb7550bd5cf
workflow-type: tm+mt
source-wordcount: '571'
ht-degree: 70%

---

# はじめに — 顧客 AI

顧客 AI のガイドでは、顧客 AI の使用に関連する様々な Platform サービスに関する十分な知識が必要です。開始する前に、次のドキュメントを確認してください。

- [Experience Data Model(XDM)システム概要](../../xdm/home.md):XDMは、Experience Platformを利用し [!DNL Adobe Experience Cloud]て適切なメッセージを適切な人に、適切なチャネルに、ちょうど適切なタイミングで配信できる基本的なフレームワークです。Experience Platform を構築する方法論である XDM システムによって、Platform サービスでエクスペリエンスデータモデルスキーマを操作できるようになります。
- [スキーマ構成の基本](../../xdm/schema/composition.md):このドキュメントでは、Experience Data Model(XDM)スキーマと、で使用するスキーマを構成するための構成要素、原則およびベストプラクティスを紹介 [!DNL Adobe Experience Platform]します。
- [スキーマの構築](../../xdm/tutorials/create-schema-ui.md)：このチュートリアルでは、Experience Platform 内でスキーマエディターを使用してスキーマを作成する手順を説明します。
- [リアルタイム顧客プロファイルの概要](../../rtcdp/overview.md):Real-time Customer Data Platform(Real-time CDP)を基盤とし [!DNL Adobe Experience Platform]て構築され、会社は既知の未知のデータを集約し、お客様のジャーニー全体でインテリジェントな判定を使用して顧客のプロファイルをアクティブ化できます。リアルタイム CDP は、複数の大規模法人データソースを組み合わせて、1 対 1 のパーソナライズされた顧客体験をすべてのチャネルとデバイスにわたって提供するために使用できる、統合されたプロファイルを作成します。
- [セグメント化サービスの概要](../../segmentation/home.md)：セグメント化とは、プロファイルストアにあるプロファイルのサブセットによって共有される特定の属性や行動を定義し、マーケティング可能な人々のグループを顧客ベースから選別するプロセスです。例えば、「スニーカーを購入し忘れましたか？」という電子メールキャンペーンでは、過去 30 日間にランニングシューズを検索したが購入を完了しなかったすべてのユーザーのオーディエンスが必要な場合があります。様々なセグメントを使用すると、様々なオーディエンスに焦点を当て、よりカスタマイズされたマーケティングエクスペリエンスを提供できます。
- [セグメントビルダーユーザガイド](../../segmentation/tutorials/create-a-segment.md)：Platform では、セグメントを簡単に作成してアクセスでき、様々な構成要素を使用してセグメントをさらに特徴付けることができます。

## 顧客 AI スコアのダウンロード

>[!NOTE]
>
>生のスコアをダウンロードする必要がない場合は、この手順をスキップして[設定ガイド](./user-guide/configure.md)に進みます。

顧客 AI スコアのダウンロードは、API 呼び出しの組み合わせを通じて実行されます。Platform API への呼び出しを実行する前に、[認証に関するチュートリアル](https://www.adobe.com/go/platform-api-authentication-en)を完了する必要があります。認証に関するチュートリアルを完了すると、すべての Experience Platform API 呼び出しで使用する、以下のような各必須ヘッダーの値が提供されます。

- Authorization: Bearer `{ACCESS_TOKEN}`
- x-api-key: `{API_KEY}`
- x-gw-ims-org-id: `{IMS_ORG}`

Experience Platform のすべてのリソースは、特定の仮想サンドボックスに分離されています。Platform API へのすべてのリクエストには、操作がおこなわれるサンドボックスの名前を指定するヘッダーが必要です。

- x-sandbox-name: `{SANDBOX_NAME}`

>[!NOTE]
>
>Platform のサンドボックスについて詳しくは、[サンドボックスの概要に関するドキュメント](../../sandboxes/home.md)を参照してください。

### API 呼び出し例の読み取り

ここでは、リクエストの形式を説明するために API 呼び出しの例を示します。これには、パス、必須ヘッダー、適切に書式設定されたリクエストペイロードが含まれます。また、API レスポンスで返されるサンプル JSON も示されています。API 呼び出し例のドキュメントで使用される表記について詳しくは、『Experience Platform トラブルシューティングガイド』の [API 呼び出し例の読み方](../../landing/troubleshooting.md)に関する節を参照してください。

## 次の手順

上記のドキュメントで概要を説明した手順を完了したら、[入出力](./input-output.md)のドキュメントを参照してください。 このドキュメントでは、Customer AIで使用され、生成されるデータのタイプについて簡単に説明します。
