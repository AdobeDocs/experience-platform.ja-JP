---
description: 宛先テスト API を使用して、宛先テストで使用できる、ストリーミング宛先用のサンプルプロファイルを生成する方法を説明します。
title: ソーススキーマに基づくサンプルプロファイルの生成
exl-id: 5f1cd00a-8eee-4454-bcae-07b05afa54af
source-git-commit: e300e57df998836a8c388511b446e90499185705
workflow-type: tm+mt
source-wordcount: '1010'
ht-degree: 97%

---


# ソーススキーマに基づくサンプルプロファイルの生成 {#sample-profile-api-operations}

>[!IMPORTANT]
>
>**API エンドポイント**：`https://platform.adobe.io/data/core/activation/authoring/sample-profiles`

このページでは、`/authoring/sample-profiles` API エンドポイントを使用して実行できるすべての API の操作について説明します。

## 異なる API 用の異なるプロファイルタイプの生成 {#different-profiles-different-apis}

>[!IMPORTANT]
>
>この API エンドポイントを使用して、2 つの個別のユースケース用にサンプルプロファイルを生成します。以下のいずれかを実行できます。
>* *宛先 ID* をクエリパラメーターとして使用することで、[メッセージ変換テンプレートを作成およびテスト](create-template.md)する際に使用するプロファイルを生成する。
>* *宛先インスタンス ID* をクエリパラメーターとして使用することで、[宛先が正しく設定されているかどうかのテスト](streaming-destination-testing-overview.md)を呼び出す際に使用するプロファイルを生成する。

Adobe XDM ソーススキーマ（宛先をテストする際に使用）または宛先でサポートされているターゲットスキーマ（テンプレートを作成する際に使用）のどちらかに基づいて、サンプルプロファイルを生成できます。Adobe XDM ソーススキーマとターゲットスキーマの違いを理解するには、[メッセージ形式](../../functionality/destination-server/message-format.md)記事の概要の節を参照してください。

サンプルプロファイルを使用可能な目的は、入れ替えられないことに注意してください。*宛先 ID* に基づいて生成されたプロファイルは、メッセージ変換テンプレートを作成するためにのみ使用でき、*宛先インスタンス ID* に基づいて生成されたプロファイルは、宛先エンドポイントをテストするためにのみ使用できます。

## サンプルプロファイル生成 API 操作の概要 {#get-started}

続行する前に、「[はじめる前に](../../getting-started.md)」を参照し、API の呼び出しを正常に行うために必要となる重要な情報（必要な宛先オーサリング権限および必要なヘッダーの取得方法など）を確認してください。

## 宛先をテストする際に使用する、ソーススキーマに基づいたサンプルプロファイルの生成 {#generate-sample-profiles-source-schema}

>[!IMPORTANT]
>
>[宛先をテスト](streaming-destination-testing-overview.md)する際に、ここで生成したサンプルプロファイルを HTTP 呼び出しに追加します。

`authoring/sample-profiles/` エンドポイントに対して GET リクエストを行い、テストしたい宛先設定に基づいて作成した宛先インスタンスの ID を指定することで、ソーススキーマに基づいてサンプルプロファイルを生成できます。

宛先インスタンスの ID を取得するために、最初に Experience Platform UI で宛先への接続を作成してから、宛先をテストしてみる必要があります。[宛先のアクティブ化チュートリアル](../../../ui/activation-overview.md)およびこの API に使用する宛先インスタンス ID の取得方法に関する以下のヒントを参照してください。

>[!IMPORTANT]
>
>* この API を使用するには、Experience Platform UI に既存の宛先への接続がある必要があります。詳しくは、[宛先への接続](https://experienceleague.adobe.com/docs/experience-platform/destinations/ui/connect-destination.html?lang=ja)および[宛先に対するプロファイルとオーディエンスのアクティブ化](https://experienceleague.adobe.com/docs/experience-platform/destinations/ui/activate/activate-segment-streaming-destinations.html)を参照してください。
> * 宛先への接続を確立したら、[宛先との接続を参照](https://experienceleague.adobe.com/docs/experience-platform/destinations/ui/destination-details-page.html)する際に、このエンドポイントへの API 呼び出しで使用する必要がある、宛先インスタンス ID を取得します。
>![宛先インスタンス ID の取得方法の UI 画像](../../assets/testing-api/get-destination-instance-id.png)

**API 形式**

```http
GET authoring/sample-profiles?destinationInstanceId={DESTINATION_INSTANCE_ID}&count={COUNT}
```

| クエリパラメーター | 説明 |
| -------- | ----------- |
| `{DESTINATION_INSTANCE_ID}` | サンプルプロファイルの生成元となる宛先インスタンスの ID。 |
| `{COUNT}` | *オプション*。生成しているサンプルプロファイルの数。このパラメーターは、`1 - 1000` の値を取ることができます。<br> count パラメーターが指定されていない場合、生成されたプロファイルのデフォルトの数は、[宛先サーバー設定](../../authoring-api/destination-server/create-destination-server.md)の `maxUsersPerRequest` 値によって決まります。このプロパティが定義されていない場合は、アドビが 1 つのサンプルプロファイルを生成します。 |

{style="table-layout:auto"}


**リクエスト**

以下のリクエストは、`{DESTINATION_INSTANCE_ID}` および `{COUNT}` クエリパラメーターで設定された、サンプルプロファイルを生成します。

```shell
curl --location --request GET 'https://platform.adobe.io/data/core/activation/authoring/sample-profiles?destinationInstanceId=49966037-32cd-4457-a105-2cbf9c01826a&count=3' \
--header 'Content-Type: application/json' \
--header 'Accept: application/json' \
--header 'x-api-key: {API_KEY}' \
--header 'Authorization: Bearer {ACCESS_TOKEN}' \
--header 'x-gw-ims-org-id: {ORG_ID}' \
--header 'x-sandbox-name: {SANDBOX_NAME}' \
```

**応答**

応答が成功すると、HTTP ステータス 200 が、指定された数のサンプルプロファイル（ソース XDM スキーマに対応するオーディエンスメンバーシップ、ID およびプロファイル属性を含む）と共に返されます。

>[!TIP]
>
> 応答は、宛先インスタンスで使用されたオーディエンスメンバーシップ、ID およびプロファイル属性のみを返します。ソーススキーマに他のフィールドがあっても、それらは無視されます。

```json
[
    {
        "segmentMembership": {
            "ups": {
                "03fb9938-8537-4b4c-87f9-9c4d413a0ee5": {
                    "lastQualificationTime": "2021-06-30T18:40:07.591378Z",
                    "status": "realized"
                },
                "27e05542-d6a3-46c7-9c8e-d59d50229530": {
                    "lastQualificationTime": "2021-06-30T18:40:07.591380Z",
                    "status": "realized"
                }
            }
        },
        "personalEmail": {
            "address": "john.smith@abc.com"
        },
        "identityMap": {
            "ECID": [
                {
                    "id": "ECID-7VEsJ"
                }
            ]
        },
        "person": {
            "name": {
                "firstName": "string"
            }
        }
    },
    {
        "segmentMembership": {
            "ups": {
                "03fb9938-8537-4b4c-87f9-9c4d413a0ee5": {
                    "lastQualificationTime": "2021-06-30T18:40:07.591378Z",
                    "status": "realized"
                },
                "27e05542-d6a3-46c7-9c8e-d59d50229530": {
                    "lastQualificationTime": "2021-06-30T18:40:07.591380Z",
                    "status": "realized"
                }
            }
        },
        "personalEmail": {
            "address": "john.smith@abc.com"
        },
        "identityMap": {
            "ECID": [
                {
                    "id": "ECID-Y55JJ"
                }
            ]
        },
        "person": {
            "name": {
                "firstName": "string"
            }
        }
    },
    {
        "segmentMembership": {
            "ups": {
                "03fb9938-8537-4b4c-87f9-9c4d413a0ee5": {
                    "lastQualificationTime": "2021-06-30T18:40:07.591378Z",
                    "status": "realized"
                },
                "27e05542-d6a3-46c7-9c8e-d59d50229530": {
                    "lastQualificationTime": "2021-06-30T18:40:07.591380Z",
                    "status": "realized"
                }
            }
        },
        "personalEmail": {
            "address": "john.smith@abc.com"
        },
        "identityMap": {
            "ECID": [
                {
                    "id": "ECID-Nd9GK"
                }
            ]
        },
        "person": {
            "name": {
                "firstName": "string"
            }
        }
    }
]
```

| プロパティ | 説明 |
| -------- | ----------- |
| `segmentMembership` | 個人のオーディエンスメンバーシップを記述するマップオブジェクト。`segmentMembership` について詳しくは、[オーディエンスメンバーシップの詳細](https://experienceleague.adobe.com/docs/experience-platform/xdm/field-groups/profile/segmentation.html)を参照してください。 |
| `lastQualificationTime` | 前回、このプロファイルがセグメントに選定された際のタイムスタンプ。 |
| `xdm:status` | オーディエンスメンバーシップが現在のリクエストの一環として実現されたかどうかを示す文字列フィールド。以下の値を使用できます。 <ul><li>`realized`：プロファイルは、セグメントの一部です。</li><li>`exited`：プロファイルは、現在のリクエストの一環としてオーディエンスから外れています。</li></ul> |
| `identityMap` | 個人の様々な ID 値を、関連する名前空間と共に記述するマップタイプフィールド。`identityMap` について詳しくは、[スキーマ構成の基本](https://experienceleague.adobe.com/docs/experience-platform/xdm/schema/composition.html#identityMap)を参照してください。 |

{style="table-layout:auto"}

## メッセージ変換テンプレートを作成する際に使用する、ターゲットスキーマに基づいたサンプルプロファイルの生成 {#generate-sample-profiles-target-schema}

>[!IMPORTANT]
>
>[テンプレートのレンダリング手順](render-template-api.md#multiple-profiles-with-body)でテンプレートを作成する際に、ここで生成したサンプルプロファイルを使用します。

`authoring/sample-profiles/` エンドポイントに対して GET リクエストを行い、作成しているテンプレートに基づいて宛先設定の宛先 ID を指定すると、ターゲットスキーマに基づいたサンプルプロファイルを生成できます。

>[!TIP]
>
>* ここで使用する必要がある宛先 ID は、宛先設定に対応した `instanceId` で、`/destinations` エンドポイントを使用して作成されています。詳しくは、[宛先設定の取得](../../authoring-api/destination-configuration/retrieve-destination-configuration.md)を参照してください。

**API 形式**


```http
GET authoring/sample-profiles?destinationId={DESTINATION_ID}&count={COUNT}
```

| クエリパラメーター | 説明 |
| -------- | ----------- |
| `{DESTINATION_ID}` | サンプルプロファイルの生成元となる宛先設定の ID。 |
| `{COUNT}` | *オプション*。生成しているサンプルプロファイルの数。このパラメーターは、`1 - 1000` の値を取ることができます。<br> count パラメーターが指定されていない場合、生成されたプロファイルのデフォルトの数は、[宛先サーバー設定](../../authoring-api/destination-server/create-destination-server.md)の `maxUsersPerRequest` 値によって決まります。このプロパティが定義されていない場合は、アドビが 1 つのサンプルプロファイルを生成します。 |

{style="table-layout:auto"}

**リクエスト**

以下のリクエストは、`{DESTINATION_ID}` および `{COUNT}` クエリパラメーターで設定された、サンプルプロファイルを生成します。

```shell
curl --location --request GET 'https://platform.adobe.io/data/core/activation/authoring/sample-profiles?destinationId=49966037-32cd-4457-a105-2cbf9c01826a&count=3' \
--header 'Content-Type: application/json' \
--header 'Accept: application/json' \
--header 'x-api-key: {API_KEY}' \
--header 'Authorization: Bearer {ACCESS_TOKEN}' \
--header 'x-gw-ims-org-id: {ORG_ID}' \
--header 'x-sandbox-name: {SANDBOX_NAME}' \
```

**応答**

応答が成功すると、HTTP ステータス 200 が、指定された数のサンプルプロファイル（ターゲット XDM スキーマに対応するオーディエンスメンバーシップ、ID およびプロファイル属性を含む）と共に返されます。

```json
[
    {
        "segmentMembership": {
            "ups": {
                "segmentid1": {
                    "lastQualificationTime": "2021-06-30T18:42:27.609326Z",
                    "status": "realized"
                },
                "segmentid3": {
                    "lastQualificationTime": "2021-06-30T18:42:27.609328Z",
                    "status": "exited"
                },
                "segmentid2": {
                    "lastQualificationTime": "2021-06-30T18:42:27.609328Z",
                    "status": "realized"
                }
            }
        },
        "identityMap": {
            "phone_sha256": [
                {
                    "id": "phone_sha256-vizii"
                }
            ],
            "gaid": [
                {
                    "id": "gaid-adKYs"
                }
            ],
            "idfa": [
                {
                    "id": "idfa-t4sKv"
                }
            ],
            "extern_id": [
                {
                    "id": "extern_id-C3enB"
                }
            ],
            "email_lc_sha256": [
                {
                    "id": "email_lc_sha256-bfnbs"
                }
            ]
        }
    },
    {
        "segmentMembership": {
            "ups": {
                "segmentid1": {
                    "lastQualificationTime": "2021-06-30T18:42:27.609626Z",
                    "status": "realized"
                },
                "segmentid3": {
                    "lastQualificationTime": "2021-06-30T18:42:27.609627Z",
                    "status": "exited"
                },
                "segmentid2": {
                    "lastQualificationTime": "2021-06-30T18:42:27.609627Z",
                    "status": "realized"
                }
            }
        },
        "identityMap": {
            "phone_sha256": [
                {
                    "id": "phone_sha256-6YjGc"
                }
            ],
            "gaid": [
                {
                    "id": "gaid-SfJ21"
                }
            ],
            "idfa": [
                {
                    "id": "idfa-eQMWS"
                }
            ],
            "extern_id": [
                {
                    "id": "extern_id-d3WzP"
                }
            ],
            "email_lc_sha256": [
                {
                    "id": "email_lc_sha256-eWfFn"
                }
            ]
        }
    },
    {
        "segmentMembership": {
            "ups": {
                "segmentid1": {
                    "lastQualificationTime": "2021-06-30T18:42:27.609823Z",
                    "status": "realized"
                },
                "segmentid3": {
                    "lastQualificationTime": "2021-06-30T18:42:27.609824Z",
                    "status": "exited"
                },
                "segmentid2": {
                    "lastQualificationTime": "2021-06-30T18:42:27.609824Z",
                    "status": "realized"
                }
            }
        },
        "identityMap": {
            "phone_sha256": [
                {
                    "id": "phone_sha256-2PMjZ"
                }
            ],
            "gaid": [
                {
                    "id": "gaid-3aLez"
                }
            ],
            "idfa": [
                {
                    "id": "idfa-D2H1J"
                }
            ],
            "extern_id": [
                {
                    "id": "extern_id-i6PsF"
                }
            ],
            "email_lc_sha256": [
                {
                    "id": "email_lc_sha256-VPUtZ"
                }
            ]
        }
    }
]
```

## API エラー処理 {#api-error-handling}

Destination SDK API エンドポイントは、一般的な Experience Platform API エラーメッセージの原則に従います。Platform トラブルシューティングガイドの [API ステータスコード](../../../../landing/troubleshooting.md#api-status-codes)および[リクエストヘッダーエラー](../../../../landing/troubleshooting.md#request-header-errors)を参照してください。

## 次の手順

このドキュメントでは、[メッセージ変換テンプレートをテスト](create-template.md)する場合や[宛先が正しく設定されているかどうかをテスト](streaming-destination-testing-overview.md)する場合に使用される、サンプルプロファイルの生成方法を確認しました。
