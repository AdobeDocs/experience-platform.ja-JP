---
keywords: Experience Platform；はじめに；attribution ai；人気の高いトピック
feature: Attribution AI
title: はじめに —Attribution AI
description: 以下のガイドでは、Attribution AI の使用に関連する様々な Adobe Experience Platform サービスについて理解している必要があります。このチュートリアルを始める前に、次のドキュメントを確認してください。
exl-id: ab269c24-97ac-4da9-9b6c-7d2dde61f0dc
source-git-commit: e4e30fb80be43d811921214094cf94331cbc0d38
workflow-type: tm+mt
source-wordcount: '619'
ht-degree: 64%

---

# Attribution AI の概要

以下のガイドでは、 [!DNL Adobe Experience Platform] Attribution AIの使用に関連するサービス。 このチュートリアルを始める前に、次のドキュメントを確認してください。

- [エクスペリエンスデータモデル (XDM) システムの概要](../../xdm/home.md):XDM は、次を可能にする基本的なフレームワークです [!DNL Adobe Experience Cloud]Experience Platformを活用して、適切な人に、適切なチャネル、正確なタイミングで、適切なメッセージを配信します。 Experience Platform を構築する方法論である XDM システムによって、Platform サービスでエクスペリエンスデータモデルスキーマを操作できるようになります。
- [スキーマ構成の基本](../../xdm/schema/composition.md):このドキュメントでは、Experience Data Model(XDM) スキーマの概要と、で使用するスキーマを構成するための構成要素、原則およびベストプラクティスを説明します [!DNL Adobe Experience Platform].
- [スキーマの構築](../../xdm/tutorials/create-schema-ui.md)：このチュートリアルでは、Experience Platform 内でスキーマエディターを使用してスキーマを作成する手順を説明します。

Attribution AIは、データセットが消費者エクスペリエンスイベント (CEE) スキーマ ( ) に準拠している必要があります [エクスペリエンスデータモデル (XDM)](../../xdm/home.md) スキーマフィールドグループを使用します。 このデータを実装または変更するには、アドビサポート（attributionai-support@adobe.com）にお問い合わせください。メディア支出データが存在する場合は、売上高や ROI の増分など、さらに分析することができます。顧客プロファイルデータが使用可能な場合、クレジットを顧客プロファイルレベルに属性付けできます。

## 用語

- **コンバージョンイベント：**&#x200B;目標に向けたマイルストーンを示すために顧客が実行するデジタルイベントまたはデジタルインタラクション（会議の登録など）。その他の例としては、有料コンバージョン、無料アカウントの新規登録、特性の認定などがあります。

- **タッチポイント：**&#x200B;目標に向けたパスにおいて顧客が実行するデジタルイベントまたはデジタルインタラクション。例としては、購入前のマーケティング活動、ディスプレイ広告インプレッションの表示、有料検索のクリックなどがあります。

## Attribution AIスコアのダウンロード

>[!NOTE]
>
>未加工のスコアをダウンロードする必要がない場合は、この手順をスキップし、 [次の手順](#next-steps).

Attribution AIスコアのダウンロードは、API 呼び出しの組み合わせを通じて実行されます。 Platform API への呼び出しを実行する前に、[認証に関するチュートリアル](https://experienceleague.adobe.com/docs/experience-platform/landing/platform-apis/api-authentication.html?lang=ja)を完了する必要があります。認証に関するチュートリアルを完了すると、すべての Experience Platform API 呼び出しで使用する、以下のような各必須ヘッダーの値が提供されます。

- Authorization: Bearer `{ACCESS_TOKEN}`
- x-api-key： `{API_KEY}`
- x-gw-ims-org-id: `{ORG_ID}`

Experience Platform のすべてのリソースは、特定の仮想サンドボックスに分離されています。Platform API へのすべてのリクエストには、操作がおこなわれるサンドボックスの名前を指定するヘッダーが必要です。

- x-sandbox-name: `{SANDBOX_NAME}`

>[!NOTE]
>
>Platform のサンドボックスについて詳しくは、[サンドボックスの概要に関するドキュメント](../../sandboxes/home.md)を参照してください。

### API 呼び出し例の読み取り

ここでは、リクエストの形式を説明するために API 呼び出しの例を示します。これには、パス、必須ヘッダー、適切な形式のリクエストペイロードが含まれます。また、API レスポンスで返されるサンプル JSON も示されています。ドキュメントで使用される API 呼び出し例の表記について詳しくは、Experience Platform トラブルシューテングガイドの[API 呼び出し例の読み方](../../landing/troubleshooting.md)に関する節を参照してください。

## アクセス制御 {#access-control}

ロールベースのアクセス制御を使用する場合、 **表示Attribution AI** および **Attribution AIの管理** 権限は、Attribution AIの様々な機能へのアクセスを許可します。 この **Attribution AIの管理** 次の **作成**, **複製**, **編集**, **削除**, **有効**&#x200B;または **無効** 例 **表示Attribution AI** 次の **読み取り** または **表示** それは。 この **作成**, **編集** および **削除** アクションは監査ログで記録されます。

[アクセス制御の権限の割り当て](../../../help/access-control/home.md)または[監査ログを使用してアクセスとアクティビティを監視](../../../help/landing/governance-privacy-security/audit-logs/overview.md)する方法については、ドキュメントを参照してください。

## 次の手順 {#next-steps}

準備が整い、すべての資格情報とスキーマが整ったら、[Attribution AI ユーザーインターフェイスガイド](./user-guide.md)に従って開始します。このガイドでは、インスタンスを作成し、トレーニングおよびスコア付け用に送信する手順を説明します。
