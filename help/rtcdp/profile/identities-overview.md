---
title: ID と ID 名前空間
seo-title: Adobe Experience Platform ID サービス
description: 説明
seo-description: SEO 説明
translation-type: tm+mt
source-git-commit: 5cba5a1e8139dd85f23250d42a1cd8d2318eb916

---


# Real-time CDP での ID

Adobe Experience Platform ID サービスは、デバイスとシステム間で ID を結合することで、顧客とその行動をより良く把握できるようにします。通常、顧客は複数のチャネルを通じてブランドとやり取りします。これには、Web サイトのオンライン閲覧、店頭での購入、ロイヤリティープログラムへの参加、サポートのためのヘルプデスクへの電話、他多数が含まれます。複数のシステムにわたって、その顧客に対して ID が作成され、ID サービスを使用すると、それらの ID を統合して全体像を確認できます。

5 つの異なるチャネル間でブランドとやりとりする 5 人の顧客が同一人物であることが分かれば、各インタラクションを通じて一貫性のあるパーソナライズされた関連性のあるエクスペリエンスを顧客に提供することができます。顧客に関する情報がさらに多くなると（例えば、Web サイトの匿名ブラウザーがアカウントにサインアップしてログインする）、情報が結合され、顧客の姿がより明確になります。

## ID 名前空間

ID 名前空間は ID サービスのコンポーネントで、顧客 ID に追加のコンテキストを提供するインジケーターとして機能します。一般的に使用される ID 名前空間の例としては、「電子メール」が挙げられます。複数の Web サイトで同じ電子メールアドレスが使用されると、同じ顧客に属する複数の異なる（それぞれが一意の顧客 ID を持つ） ID を結合できます。Experience Platform では、ID 名前空間を使用して、ユーザーインターフェイス内の個々のプロファイルを検索できます。プロファイルの表示について詳しくは、「[プロファイルビューアの概要](/help/rtcdp/profile/profile-viewer.md)」を参照してください。ID 名前空間について詳しくは、Adobe I/O の『[ID 名前空間の概要](https://www.adobe.io/apis/experienceplatform/home/profile-identity-segmentation/profile-identity-segmentation-services.html#!api-specification/markdown/narrative/technical_overview/identity_namespace_overview/identity_namespace_overview.md)』を参照してください。

## ID グラフ

ID グラフは、異なる ID 名前空間間の関係のマップで、顧客が様々なチャネルを通じてブランドとどのようにやり取りするかを視覚的に示します。すべての顧客 ID グラフは、顧客の行動に応じて、ほぼリアルタイムで ID サービスによって管理および更新されます。

ID サービスは、組織のみが表示できるデータに基づいて構築された ID グラフ（「プライベートグラフ」）を管理します。取り込んだデータレコードに複数の ID が含まれている場合、ID サービスはプライベートグラフを増補し、見つかった ID 間の関係を追加します。

## 次の手順

ID および ID 間の関係は、ID サービスによって定義および維持され、リアルタイム顧客プロファイルによって使用されて個々の顧客とそのインタラクションの全体像を作成します。詳しくは、[Adobe I/O の ID サービスのドキュメント](https://www.adobe.io/apis/experienceplatform/home/profile-identity-segmentation/profile-identity-segmentation-services.html#!api-specification/markdown/narrative/technical_overview/identity_services_architectural_overview/identity_services_architectural_overview.md)を参照してください。