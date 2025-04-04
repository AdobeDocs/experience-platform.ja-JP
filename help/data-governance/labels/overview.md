---
keywords: Experience Platform;ホーム;人気のピック;データガバナンス;Data Usage Label API;Policy Service API;データ使用ラベルの概要
solution: Experience Platform
title: データ使用ラベルの概要
description: Adobe Experience Platform でデータガバナンスのコンプライアンスを適用するために、データ使用ラベルを使用する方法について説明します。
exl-id: 4f113000-b9a1-4dfb-9502-6a5d08f0b26f
source-git-commit: f129c215ebc5dc169b9a7ef9b3faa3463ab413f3
workflow-type: tm+mt
source-wordcount: '789'
ht-degree: 94%

---

# データ使用ラベルの概要 {#overview}

>[!CONTEXTUALHELP]
>id="platform_privacyConsole_dataUsageLabels_description"
>title="機密データと保護データへのアクセスの制御"
>abstract="<h2>説明</h2><p>特定のデータ属性やセグメントへのアクセスを制御することで、Experience Platform のユースケースを運用する様々なペルソナやチーム向けの柔軟なワークフローを設計できます。</p>"

Adobe Experience Platform では、関連する[データガバナンスポリシー](../policies/overview.md)と[アクセス制御ポリシー](../../access-control/abac/ui/policies.md)に従ってそれぞれを分類し、データ使用ラベルをデータセットとフィールドに適用できます。

このドキュメントでは、[!DNL Experience Platform] のデータ使用ラベルの概要を説明します。

## データ使用ラベルについて

データ使用ラベルを使用すると、データに適用されるガバナンスポリシーに従って、データセットとフィールドを分類できます。ラベルはいつでも適用でき、データの管理方法を柔軟に選択できます。ベストプラクティスでは、データが [!DNL Experience Platform] に取得されるとすぐに、またはデータが [!DNL Experience Platform] で使用できるようになるとすぐに、データのラベル付けが推奨されます。

データ使用ラベルがデータセットレベルで適用されると、そのデータセット内のすべてのフィールドにラベルが伝播されます。ラベルは、伝播ではなく、データセット内の個々のフィールド（列ヘッダー）に直接適用することもできます。

[!DNL Experience Platform] は、「コア」データ使用ラベルを標準搭載で提供しています。これは、データガバナンスに適用される様々な一般的な制限をカバーしています。これらのラベルとガバナンスポリシーについて詳しくは、[コアデータ使用ラベル](reference.md)のガイドを参照してください。

アドビが提供するラベルに加えて、組織に独自のカスタムラベルを定義することもできます。詳しくは、[ラベルの管理](#manage-labels)のセクションを参照してください。

## オーディエンスセグメントのラベルの継承

[Adobe Experience Platform セグメント化サービス](../../segmentation/home.md)で作成されたすべてのオーディエンスセグメントは、対応するデータセットの使用ラベルを継承します。これにより、Experience Platform は、宛先に対してセグメントをアクティブ化する際に、自動ポリシー適用を提供できます。

データセットレベルのラベルを継承する以外に、デフォルトでは、セグメントは関連するデータセットからフィールドレベルのラベルをすべて継承します。したがって、セグメントから除外する属性をより容易に識別し、除外するフィールドのラベルを継承しないようにすることができます。

Experience Platformでの自動適用の動作方法について詳しくは、[ 自動ポリシーの適用 ](../enforcement/auto-enforcement.md) の概要を参照してください。

### Adobe Audience Manager データ書き出しのコントロールからの継承

[!DNL Experience Platform] には、Adobe Audience Manager とセグメントを共有する機能があります。Audience Manager セグメントに適用されたデータエクスポートのコントロールは、[!DNL Experience Platform] データガバナンスが認識する同等のラベルおよびマーケティングアクションに変換されます。

特定のデータエクスポートのコントロールが [!DNL Experience Platform] のデータ使用ラベルにどのようにマッピングされるかについては、[Audience Manager ドキュメント](https://experienceleague.adobe.com/docs/audience-manager/user-guide/implementation-integration-guides/integration-experience-platform/aam-aep-audience-sharing.html?lang=ja#aam-data-export-control-in-aep)を参照してください。

## でのデータ使用ラベルの管理 [!DNL Experience Platform] {#manage-labels}

>[!CONTEXTUALHELP]
>id="platform_privacyConsole_dataUsageLabels_instructions"
>title="手順"
>abstract="<ul><li>XDM フィールドとセグメントにラベルを付けて、アクセス制限の対象となるフィールドやセグメントを分類します。</li><li>役割にラベルを付けます。役割にラベルを付加すると、その役割のメンバーが制限対象となるラベルを定義できます。</li><li>ポリシーを作成します。ポリシーを使用すると、XDM フィールドやセグメントなどのラベル付きオブジェクトのラベルと役割のラベルとの関係を作成できます。ラベルが一致する場合は、アクセスの許可または制限を定義できます。</li></ul>"

[!DNL Experience Platform] API またはユーザーインターフェイスを使用して、データ使用ラベルを管理できます。それぞれの詳細については、以下のサブセクションを参照してください。

### UI の使用

[!DNL Experience Platform] UI の&#x200B;**[!UICONTROL ポリシー]**&#x200B;ワークスペースでは、組織のコアとカスタムラベルの表示と管理が可能です。**[!UICONTROL スキーマ]**&#x200B;ワークスペースを使用して[エクスペリエンスデータモデル（XDM）スキーマにラベルを適用](../../xdm/tutorials/labels.md)したり、代わりにデータ使用ラベルユーザーガイドを参照して、[**[!UICONTROL ポリシー] UI でカスタムラベルを作成および管理](./user-guide.md)する方法を学んだりできます。

>[!IMPORTANT]
>
>データセットレベルでフィールドにラベルを適用できなくなりました。このワークフローは非推奨（廃止予定）になり、スキーマレベルでラベルを適用するようになりました。データセットオブジェクトレベルで以前に適用したラベルは、2024 年 5 月 31 日（PT）までExperience Platform UI を通じて引き続きサポートされます。 すべてのスキーマ間でラベルの一貫性を確保するには、データセットレベルで以前にフィールドに付けていたラベルを、今後 1 年間にスキーマレベルに移行する必要があります。これを行う方法の手順については、[以前に適用したラベルの移行](../e2e.md#migrate-labels)に関する節を参照してください。

### API の使用

[Policy Service API](https://www.adobe.io/experience-platform-apis/references/policy-service/) の `/labels` エンドポイントを使用すると、カスタムラベルの作成など、データ使用ラベルをプログラムに従って管理できます。詳しくは、『[ラベルのエンドポイントガイド](../api/labels.md)』を参照してください。

[データセットサービス API ](https://www.adobe.io/experience-platform-apis/references/dataset-service/)は、データセットとフィールドのラベルの管理に使用します。詳しくは、[データセットラベルの管理](./dataset-api.md)のガイドを参照してください。

## 次の手順

このドキュメントでは、データ使用ラベルと、データガバナンスフレームワーク内でのラベルの役割について説明しました。[!DNL Experience Platform] でのラベルの管理方法の詳細については、このガイドを通じてリンクされているドキュメントを参照してください。
