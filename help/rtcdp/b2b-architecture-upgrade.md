---
title: Real-Time CDP B2B editionのアーキテクチャのアップグレード
description: Real-Time CDP B2B editionのアーキテクチャの包括的なアップグレードについては、このドキュメントを参照してください。
badgeB2B: label="B2B edition" type="Informative" url="https://helpx.adobe.com/jp/legal/product-descriptions/real-time-customer-data-platform-b2b-edition-prime-and-ultimate-packages.html newtab=true"
exl-id: d958a947-e195-4dd4-a04c-63ad82829728
source-git-commit: 1a3be99ca3c270dda6e8dc559359cbe21bb8f4fb
workflow-type: tm+mt
source-wordcount: '1074'
ht-degree: 0%

---

# Real-Time CDP B2B editionへのアーキテクチャのアップグレード

>[!IMPORTANT]
>
>このドキュメントでは、Real-Time CDP B2B および B2P エディションへのアーキテクチャのアップグレードの概要を説明します。 アップグレードでは、ほとんどのお客様の対応は必要ありません。 ただし、自動的にアップグレードできないオーディエンスもあります。 Adobeは、これらのシナリオに対処するために、お客様と直接協力します。 アップグレードがAdobe Experience Platformの既存の機能に与える影響については、このドキュメントを参照してください。 ご不明な点については、Adobe アカウントチームにお問い合わせください。

Adobeでは、拡張性、パフォーマンス、信頼性を強化すると同時に、より高度な B2B のユースケースをサポートするように、Real-Time CDP B2B および B2P エディションを再設計しました。 すべてのお客様にこれらの機能強化のメリットを確実に享受していただけるように、Adobeは既存の B2B および B2P のお客様を新しいアーキテクチャにアップグレードします。

拡張アーキテクチャを使用すると、次のメリットがあります。

* **データ取り込みのスケーラビリティ**：多数の人と接続されているアカウントなど、基数の高い B2B 関係のサポートを改善しました。
* **パフォーマンスと信頼性の高いオーディエンス評価**：複雑な B2B オーディエンスに対して、より迅速で柔軟なセグメント化を行います。
* **エンティティの解決**:B2B エンティティの ID 解決の強化、データ品質の向上、重複の減少により、より正確なセグメント化と集計が可能になります。

## 新機能

アーキテクチャのアップグレードに含まれる主な機能強化については、次をお読みください。

### オーディエンスメンバーシップのアカウントスナップショット

新しい B2B アーキテクチャで、スナップショット書き出しのアカウントエンティティにオーディエンスメンバーシップの詳細が含まれるようになりました。 この機能を使用すると、アカウントレベルのオーディエンスステータス、タイムスタンプおよびメンバーシップ指標にアクセスできます。

このアップグレードにより、以下が可能になります。

* マーケティングチームおよび運用チームが、アカウントオーディエンスメンバーシップを直接検証できるようにします。
* プロファイル（ユーザー）とアカウントのセグメント化モデルの間で機能の等価性を実現し、エンティティ間で一貫したエクスペリエンスを確保します。

詳しくは、[ アカウントオーディエンス ](../segmentation/types/account-audiences.md) に関するドキュメントを参照してください。

### B2B エンティティを含んだオーディエンスのオーディエンス数

B2B エンティティを使用するオーディエンスのオーディエンスサイズの予測が、正確な精度で計算されるようになりました。 これらの予測値はプレビュー時に使用でき、B2B の複雑な関係を含むオーディエンスに対して、より正確で信頼性の高いインサイトを提供します。

このアップグレードにより、以下が可能になります。

* 正確なオーディエンスサイズの予測からのインサイトを使用して、オーディエンス作成プロセス中の計画および意思決定を改善します。
* より正確なオーディエンス推定の知識を持ち、複雑な B2B オーディエンスを自信を持って設計します。
* よりスマートなキャンペーン計画、より正確なターゲティング、より優れたリソース割り当てを可能にします。

詳しくは、[ アカウントオーディエンス ](../segmentation/types/account-audiences.md) に関するドキュメントを参照してください。

## 既存機能のアップグレード

B2B アーキテクチャのアップグレードの一環として、次の機能が更新されました。

### B2B 属性とエクスペリエンスイベントを使用したマルチエンティティオーディエンスの更新

新しいアーキテクチャのアップグレードの一環として、B2B 属性を含む単一のマルチエンティティオーディエンス内でエクスペリエンスイベントフィルターを使用できなくなりました。

同じオーディエンスロジックを実現するには、セグメントビルダーを使用して [ オーディエンスを追加し、参照 ](../segmentation/ui/segment-builder.md#adding-audiences) します。

例：

* エクスペリエンスイベントオーディエンスの作成
   * 行動条件を個別に定義します。 例：「過去 3 日間に価格ページにアクセスした人物。」
* B2B 属性を持つマルチエンティティオーディエンスを作成します。
   * ここから、このオーディエンスの条件の一部として、エクスペリエンスイベントオーディエンスを参照できます。 例えば、「**ーザーの意思決定者」は、アカウントが** ーザー金融 **業界に属するオポチュニティを持ち、過去 3 日間に価格ページにアクセスした人物オーディエンス** メンバーです。

アップグレードが完了したら、B2B 属性とエクスペリエンスイベントを持つ新しいマルチエンティティオーディエンスを、[ セグメントのセグメント ](../segmentation/methods/edge-segmentation.md#edge-segmentation-query-types) アプローチを使用して作成する必要があります。

>[!TIP]
>
>**セグメントのセグメント** とは、1 つ以上のバッチセグメントまたはエッジセグメントを含む任意のセグメント定義です。 **メモ**：セグメントのセグメントを使用する場合、プロファイルの不適格が発生します **24 時間ごと**。

### B2B オーディエンスでのエンティティの解決と時間の優先順位の結合

アーキテクチャのアップグレードの一環として、Adobeでは、アカウントとオポチュニティにエンティティの解決を導入しています。 エンティティの解決は、決定論的な ID マッチングに基づき、最新のデータに基づいています。 エンティティ解決ジョブは、B2B 属性を持つマルチエンティティオーディエンスを評価する前に、バッチセグメント化中に毎日実行されます。

>[!BEGINSHADEBOX]

#### エンティティの解決はどのように機能しますか？

* **以前**：データユニバーサル番号付けシステム（DUNS）番号が追加 ID として使用され、アカウントの DUNS 番号が CRM などのソースシステムで更新された場合、アカウント ID は古い DUNS 番号と新しい DUNS 番号の両方にリンクされます。
* **後**:DUNS 番号が追加 ID として使用され、アカウントの DUNS 番号が CRM などのソースシステムで更新された場合、アカウント ID は新しい DUNS 番号にのみリンクされ、アカウントの現在の状態がより正確に反映されます。

>[!ENDSHADEBOX]

このアップグレードにより、以下が可能になります。

* 毎日のエンティティ解決ジョブが完了したら、[!DNL Profile Access] API を使用して最新の結合プロファイルを表示します。
* セグメンテーション、アクティベーション、分析に、改善された精度と一貫性のあるアカウントと商談データを利用します。

詳しくは、[[!DNL Profile Access] API](../profile/api/entities.md) を参照してください。

### マルチエンティティ B2B オーディエンスでの結合ポリシーのサポート

B2B 属性を持つマルチエンティティオーディエンスで、複数の結合ポリシーではなく、単一の結合ポリシー（ユーザーが設定するデフォルトの結合ポリシー）がサポートされるようになりました。

詳しくは、[Real-Time CDP B2B editionのセグメント化ユースケースガイド ](./segmentation/b2b.md) を参照してください。

### [!DNL Profile Access] API での B2B エンティティの参照と削除の廃止

[!DNL Profile Access] API を使用する B2B エンティティに対する次のルックアップ機能は、非推奨（廃止予定）になっています。

* アカウントと人物の関係
* 商談と担当者の関係
* Campaign
* キャンペーンメンバー
* マーケティングリスト
* マーケティングリストメンバー

[!DNL Profile Access] API を使用した次の B2B エンティティに対する削除リクエストは非推奨（廃止予定）になっています。

* アカウント
* アカウントと人物の関係
* Opportunity
* 商談と担当者の関係
* Campaign
* キャンペーンメンバー
* マーケティングリスト
* マーケティングリストメンバー

詳しくは、[[!DNL Profile Access] API](../profile/api/entities.md) を参照してください。

### アカウントおよび商談プロファイルのルックアップ

アカウントスキーマと商談スキーマを、毎日のエンティティ解決プロセスを完了した後にのみ、ルックアップディメンションエンティティとして取得できるようになりました。 新しく取り込まれたレコードは、次のエンティティ解決サイクルが完了するまで（通常 24 時間ごと）、プロファイルエンリッチメントまたはセグメント定義には使用できません。

<!-- ### Deprecation of audience creation via API for B2B entities

Creation of audiences using B2B entities via API is being deprecated. The list of affected B2B entities include:

* Account
* Opportunity
* Account-Person Relation
* Opportunity-Person Relation
* Campaign
* Campaign Member
* Marketing List
* Marketing List Member

Read the [segment definitions endpoint API guide](../segmentation/api/segment-definitions.md) for more information. -->

### サンドボックスツールでのマルチエンティティオーディエンスの読み込みの変更

アーキテクチャのアップグレードにより、これらのオーディエンスを含むパッケージがアップグレード前に公開された場合、B2B 属性およびエクスペリエンスイベントを使用してマルチエンティティオーディエンスをインポートできなくなります。 これらのオーディエンスは読み込みに失敗し、新しいアーキテクチャに自動的に変換することはできません。 この制限を回避するには、更新されたオーディエンスを含む新しいパッケージを作成し、サンドボックスツールを使用してそれぞれのターゲットサンドボックスに読み込む必要があります。

開発用サンドボックスは新しいアーキテクチャにアップグレードされます。 自動更新できるオーディエンスはアップグレードされます。無効にできないオーディエンスは無効になります。 無効なオーディエンスは、アップグレード後に再作成する必要があります。

詳しくは、[ サンドボックスツールガイド ](../sandboxes/ui/sandbox-tooling.md) を参照してください。
