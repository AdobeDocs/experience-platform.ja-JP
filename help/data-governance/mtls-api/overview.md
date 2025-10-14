---
title: MTLS API ガイド
description: mTLS サービス API を使用して、Adobeによって発行された公開証明書を安全に取得および検証する方法を説明します。
role: Developer
source-git-commit: f805d03ff2cd3a4f84ca8068023d83986f8bdfbb
workflow-type: tm+mt
source-wordcount: '196'
ht-degree: 1%

---

# MTLS サービス API の概要

MTLS サービス API を使用して、組織のアプリケーション用にAdobeによって発行された公開証明書を安全に取得します。 この API は、顧客とAdobe Experience Platformの間のデータ交換が認証および暗号化されていることを確認し、セキュリティレイヤーを強化します。 証明書の信頼性を外部で検証することで、信頼性を高め、機密情報を保護できます。

## 公開証明書

公開証明書は、安全な通信におけるサーバーまたはクライアントの ID の認証に使用されるデジタルドキュメントです。 mTLS サービス API のコンテキストでは、これらの証明書により、Adobe Experience Platformとのデータ交換が認証および暗号化されます。 API を通じてこれらの証明書を取得および検証することで、その真正性が確認され、データトランザクションのセキュリティと信頼性が向上し、機密情報が保護されます。 公開証明書を取得する方法については、[&#x200B; エンドポイントガイド &#x200B;](./public-certificate-endpoint.md) を参照して、呼び出しを行う方法を確認してください。

## 次の手順

MTLS サービス API を使用した呼び出しを開始するには、[&#x200B; はじめる前に &#x200B;](./getting-started.md) を参照して、必要なヘッダー、サンプル API 呼び出しの読み取りなどに関する重要な情報を確認します。
