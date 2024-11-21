---
keywords: Experience Platform; クエリサービス；IP アクセス制御；認証；API；はじめに
title: Data Distiller認証 API ガイド
description: Adobe Experience Platform クエリサービス内で安全にデータアクセスできるよう、認証と IP 範囲制限の開始に関する方法を説明します。
role: Developer
exl-id: d93ce774-c8b2-4f15-a4d9-117d9aa5d9e7
source-git-commit: ac29d10d3774a736d1e54255508ba244ff72f278
workflow-type: tm+mt
source-wordcount: '485'
ht-degree: 5%

---

# Data Distiller Authorization API の基本を学ぶ

>[!AVAILABILITY]
>
>この機能は、Data Distiller アドオンを購入したお客様が利用できます。 詳しくは、アドビ担当者にお問い合わせください。

Data Distiller Authorization API を使用すると、Adobe Experience Platformの SQL インターフェイスを介したデータアクセスをより厳しく制御できます。 この API を使用すると、IP 制限を定義し、指定したネットワークへのデータアクセスを制限し、セキュリティ監視を強化できます。

このガイドでは、Data Distiller Authorization API を呼び出すために必要な認証資格情報と権限の設定方法について説明します。

## はじめに {#getting-started}

以下の節では、必要な認証値を準備し、Data Distiller Authorization API に最初のリクエストを行う方法について説明します。

### 必要な権限 {#required-permissions}

クエリサービスで安全なデータアクセス制限を有効にするには、**[!UICONTROL 許可リストの管理]** 権限が必要です。 この権限を使用すると、SQL インターフェイスを介して Platform のデータにアクセスする権限がある特定の IP 範囲（IPv4 または IPv6 形式）を定義できます。 アクセスはサンドボックスレベルで管理されます。ユーザーは、許可されたネットワークのみにアクセスを制限する、承認済み IP アドレスまたは CIDR ブロックのリストを設定できます。

>[!NOTE]
>
>システム管理者は、Adobe[Admin Console](https://adminconsole.adobe.com/) からユーザー権限を設定できます。 詳しくは、[Admin Console ユーザーガイド](https://helpx.adobe.com/jp/enterprise/using/admin-console.html)を参照してください。

**[!UICONTROL 許可リストを管理]** 権限では、次の機能が使用できます。

- **許可される IP 範囲を定義**：これらの定義された範囲の IP アドレスまたは CIDR ブロックのみが、クエリサービスを通じて SQL を使用して Platform のデータにアクセスできます。
- **IP 範囲チェックの実施**：許可されている範囲外の IP からの接続は拒否されます。
- **監査およびアラート機能**：拒否された接続を含むすべてのアクセス試行が、監査イベントとして記録されます。 これらのイベントは、[Adobe Experience Platform監査ログ ](../../landing/governance-privacy-security/audit-logs/overview.md) で利用でき、潜在的なセキュリティ侵害を監視できます。

### 必須ヘッダーの値の収集 {#gather-values-for-required-headers}

Data Distiller認証 API を呼び出すには、API 呼び出しで必要なヘッダーの値を提供する [Platform API 認証チュートリアル ](../../landing/api-authentication.md) を完了する必要があります。 各リクエストに次のヘッダーを含めます。

- **認証**: `Bearer {ACCESS_TOKEN}`
- **x-api-key**: `{API_KEY}`
- **x-gw-ims-org-id**: `{ORG_ID}`
- **x-sandbox-name**: `{SANDBOX_NAME}`

>[!NOTE]
>
> サンドボックスについて詳しくは、[ サンドボックスの概要に関するドキュメント ](../../sandboxes/home.md) を参照してください。

ペイロード（POST、PUT、PATCH）を含むすべてのリクエストには、次のヘッダーも必要です。

- **Content-Type**: `application/json`

### 次の手順

必要な権限とヘッダー値が収集されたら、クエリサービスの IP 制限の設定を開始する準備が整います。 次のような CRUD 操作の詳細な例については、エンドポイントのドキュメントを参照してください。

- API 形式とサンプルのリクエスト/応答ペア。
- 各操作のヘッダー、ペイロードおよび応答コード。

各 API 呼び出しの例では、リクエストの形式を設定し応答を解釈する方法を示しているので、クエリサービスでのデータへの安全なアクセスを強制するのに役立ちます。

IP 制限の設定と検証の具体的な手順については、[IP アクセスエンドポイントのドキュメント ](./ip-access.md) および [IP 検証エンドポイントのドキュメント ](./validate.md) を参照してください。
