---
keywords: Experience Platform；ホーム；人気の高いトピック；ソース；コネクタ；ソースコネクタ；ソース sdk;SDK;SDK
solution: Experience Platform
title: フローサービス API（ベータ版）を使用して新しい接続仕様を作成します
topic-legacy: tutorial
description: 次のドキュメントでは、フローサービス API を使用して接続仕様を作成し、ソース SDK を使用して新しいソースを統合する手順を説明します。
hide: true
hidefromtoc: true
exl-id: 0b0278f5-c64d-4802-a6b4-37557f714a97
source-git-commit: 5af36c096a030c9f06c304f2c899ce49066ae4cb
workflow-type: tm+mt
source-wordcount: '524'
ht-degree: 3%

---

# を使用して新しい接続仕様を作成します。 [!DNL Flow Service] API（ベータ版）

>[!IMPORTANT]
>
>ソース SDK は現在ベータ版です。お客様の組織はまだアクセスできない可能性があります。 このドキュメントで説明する機能は、変更される場合があります。

接続仕様は、ソースの構造を表します。 ソースの認証要件に関する情報が含まれ、ソースデータの調査および検査方法が定義され、特定のソースの属性に関する情報が提供されます。 この `/connectionSpecs` エンドポイント [!DNL Flow Service] API を使用すると、組織内の接続仕様をプログラムで管理できます。

次のドキュメントでは、 [!DNL Flow Service] API を使用し、Sources SDK を使用して新しいソースを統合します。

## はじめに

続行する前に、 [入門ガイド](./getting-started.md) 関連ドキュメントへのリンク、このドキュメントの API 呼び出し例の読み方のガイド、および任意のExperience PlatformAPI を正しく呼び出すために必要な必須ヘッダーに関する重要な情報。

## アーティファクトを収集

を通じて新しいソースを作成する最初の手順 [!DNL Sources SDK] は、Adobeの担当者と調整し、ソースの対応する値を特定するために使用します **アイコン**, **説明**, **ラベル**、および **カテゴリ**.

| アーティファクト | 説明 | 例 |
| --- | --- | --- |
| ラベル | ソースの名前。 | [!DNL MailChimp Members] |
| 説明 | ソースの簡単な説明。 | へのライブインバウンド接続を作成します [!DNL Mailchimp Members] インスタンスを使用して、履歴データとスケジュールされたデータの両方をExperience Platformに取り込みます。 |
| アイコン | ソースを表す画像またはロゴ。 このアイコンは、ソースの Platform UI レンダリングに表示されます。 | `mailchimp-members-icon.svg` |
| カテゴリ | ソースのカテゴリ。 | <ul><li>`advertising`</li><li>`crm`</li><li>`customer success`</li><li>`database`</li><li>`ecommerce`</li><li>`marketing automation`</li><li>`payments`</li><li>`protocols`</li></ul> |

{style=&quot;table-layout:auto&quot;}

## 接続仕様テンプレートをコピー

必要なアーティファクトを収集したら、次の接続仕様テンプレートをコピーして任意のテキストエディターに貼り付け、ブラケットで属性を更新します `{}` 特定のソースに関連する情報を含む。

```json
{
  "name": "generic-rest-extension",
  "type": "generic-rest",
  "description": "{DESCRIPTION}",
  "providerId": "0ed90a81-07f4-4586-8190-b40eccef1c5a",
  "version": "1.0",
  "attributes": {
    "uiAttributes": {
      "apiFeatures": {
        "explorePaginationSupported": false
      }
    }
  },
  "authSpec": [
    {
      "name": "OAuth2 Refresh Code",
      "type": "OAuth2RefreshCode",
      "spec": {
        "$schema": "http://json-schema.org/draft-07/schema#",
        "type": "object",
        "description": "Define auth params required for connecting to generic rest using oauth2 authorization code.",
        "properties": {
          "host": {
            "type": "string",
            "description": "Enter resource url host path."
          },
          "authorizationTestUrl": {
            "description": "Authorization test url to validate accessToken.",
            "type": "string"
          },
          "clientId": {
            "description": "Client id of user account.",
            "type": "string"
          },
          "clientSecret": {
            "description": "Client secret of user account.",
            "type": "string",
            "format": "password"
          },
          "accessToken": {
            "description": "Access Token",
            "type": "string",
            "format": "password"
          },
          "refreshToken": {
            "description": "Refresh Token",
            "type": "string",
            "format": "password"
          },
          "expirationDate": {
            "description": "Date of token expiry.",
            "type": "string",
            "format": "date",
            "uiAttributes": {
              "hidden": true
            }
          },
          "accessTokenUrl": {
            "description": "Access token url to fetch access token.",
            "type": "string"
          },
          "requestParameterOverride": {
            "type": "object",
            "description": "Specify parameter to override.",
            "properties": {
              "accessTokenField": {
                "description": "Access token field name to override.",
                "type": "string"
              },
              "refreshTokenField": {
                "description": "Refresh token field name to override.",
                "type": "string"
              },
              "expireInField": {
                "description": "ExpireIn field name to override.",
                "type": "string"
              },
              "authenticationMethod": {
                "description": "Authentication method override.",
                "type": "string",
                "enum": [
                  "GET",
                  "POST"
                ]
              },
              "clientId": {
                "description": "ClientId field name override.",
                "type": "string"
              },
              "clientSecret": {
                "description": "ClientSecret field name override.",
                "type": "string"
              }
            }
          }
        },
        "required": [
          "host",
          "accessToken"
        ]
      }
    },
    {
      "name": "Basic Authentication",
      "type": "BasicAuthentication",
      "spec": {
        "$schema": "http://json-schema.org/draft-07/schema#",
        "type": "object",
        "description": "defines auth params required for connecting to rest service.",
        "properties": {
          "host": {
            "type": "string",
            "description": "Enter resource url host path"
          },
          "username": {
            "description": "Username to connect rest endpoint.",
            "type": "string"
          },
          "password": {
            "description": "Password to connect rest endpoint.",
            "type": "string",
            "format": "password"
          }
        },
        "required": [
          "host",
          "username",
          "password"
        ]
      }
    }
  ],
  "sourceSpec": {
    "attributes": {
      "uiAttributes": {
        "isSource": true,
        "isPreview": true,
        "isBeta": true,
        "category": {
          "key": "{CATEGORY}"
        },
        "icon": {
          "key": "{ICON}"
        },
        "description": {
          "key": "{DESCRIPTION}"
        },
        "label": {
          "key": "{LABEL}"
        }
      },
      "urlParams": {
        "path": "{RESOURCE_PATH}",
        "method": "{GET_or_POST}",
        "queryParams": "{QUERY_PARAMS}"
      },
      "headerParams": "{HEADER_VALUES}",
      "bodyParams": "{BODY_PARAMS_USED_IF_METHOD_IS_POST}",
      "contentPath": {
        "path": "{PATH_SHOULD_POINT_TO_COLLECTION_OF_RECORDS}",
        "skipAttributes": [],
        "overrideWrapperAttribute": "{OVERRIDE_ATTRIBUTES}"
      },
      "explodeEntityPath": {
        "path": "{PATH_SHOULD_POINT_TO_COLLECTION_OF_RECORDS}",
        "skipAttributes": [],
        "overrideWrapperAttribute": "{OVERRIDE_ATTRIBUTES}"
      },
      "paginationParams": {
        "type": "{OFFSET_OR_POINTER}",
        "limitName": "{NUMBER_OF_RECORDS_ATTRIBUTE_NAME}",
        "limitValue": "{NUMBER_OF_RECORDS_PER_PAGE}",
        "offSetName": "{OFFSET_ATTRIBUTE_NAME_REQUIRED_IN_CASE_OF_OFFSET BASED_PAGINATION}",
        "pointerName": "{POINTER_PATH_REQUIRED_IN__CASE_OF_POINTER BASED_PAGINATION}"
      },
      "scheduleParams": {
        "scheduleStartParamName": "{START_TIME_PARAMETER_NAME}",
        "scheduleEndParamName": "{END_TIME_PARAMETER_NAME}",
        "scheduleStartParamFormat": "{DATE_TIME_FORMAT_FOR_START_TIME}",
        "scheduleEndParamFormat": "{END_TIME_FORMAT_FOR_START_TIME}"
      }
    },
    "spec": {
      "$schema": "http://json-schema.org/draft-07/schema#",
      "type": "object",
      "description": "Define user input parameters to fetch resource values.",
      "properties": {
        "listId": {
          "type": "string",
          "description": "listId for which members need to fetch."
        }
      }
    }
  },
  "exploreSpec": {
    "name": "Resource",
    "type": "Resource",
    "requestSpec": {
      "$schema": "http://json-schema.org/draft-07/schema#",
      "type": "object"
    },
    "responseSpec": {
      "$schema": "http: //json-schema.org/draft-07/schema#",
      "type": "object",
      "properties": {
        "format": {
          "type": "string"
        },
        "schema": {
          "type": "object",
          "properties": {
            "columns": {
              "type": "array",
              "items": {
                "type": "object",
                "properties": {
                  "name": {
                    "type": "string"
                  },
                  "type": {
                    "type": "string"
                  }
                }
              }
            }
          }
        },
        "data": {
          "type": "array",
          "items": {
            "type": "object"
          }
        }
      }
    }
  }
}
```

## 接続仕様の作成 {#create}

接続仕様テンプレートを取得したら、ソースに対応する適切な値を入力して、新しい接続仕様の作成を開始できます。

接続仕様は、次の 3 つの個別の部分に分割できます。認証仕様、ソース仕様、およびエクスプローラ仕様。

接続仕様の各部分の値を入力する手順については、次のドキュメントを参照してください。

* [認証仕様の設定](../config/authspec.md)
* [ソースの指定を構成します。](../config/sourcespec.md)
* [エクスプローラの仕様を設定する](../config/explorespec.md)

仕様情報を更新したら、に対してPOST・リクエストを実行して、新しい接続仕様を発行できます。 `/connectionSpecs` エンドポイント [!DNL Flow Service] API

**API 形式**

```http
POST /connectionSpecs
```

**リクエスト**

次のリクエストは、 [!DNL MailChimp] ソース：

```shell
curl -X POST \
  'https://platform.adobe.io/data/foundation/flowservice/connectionSpecs' \
  -H 'Authorization: Bearer {ACCESS_TOKEN}' \
  -H 'x-api-key: {API_KEY}' \
  -H 'x-gw-ims-org-id: {IMS_ORG}' \
  -H 'x-sandbox-name: {SANDBOX_NAME}' \
  -H 'Content-Type: application/json' \
  -d '{
      "name": "MailChimp Members source",
      "description": "MailChimp Members source using generic-rest and SDK",
      "type": "generic-rest",
      "providerId": "0ed90a81-07f4-4586-8190-b40eccef1c5a",
      "version": "1.0",
      "attributes": {
          "uiAttributes": {
              "apiFeatures": {
                  "explorePaginationSupported": false
              }
          }
      },
      "authSpec": [
          {
              "name": "OAuth2 Refresh Code",
              "type": "OAuth2RefreshCode",
              "spec": {
                  "$schema": "http://json-schema.org/draft-07/schema#",
                  "type": "object",
                  "description": "Define auth params required for connecting to generic rest using oauth2 authorization code.",
                  "properties": {
                      "host": {
                          "type": "string",
                          "description": "Enter resource url host path"
                      },
                      "authorizationTestUrl": {
                          "description": "Authorization test url to validate accessToken.",
                          "type": "string"
                      },
                      "accessToken": {
                          "description": "Access Token of mailChimp endpoint.",
                          "type": "string",
                          "format": "password"
                      }
                  },
                  "required": [
                      "host",
                      "accessToken"
                  ]
              }
          },
          {
              "name": "Basic Authentication",
              "type": "BasicAuthentication",
              "spec": {
                  "$schema": "http://json-schema.org/draft-07/schema#",
                  "type": "object",
                  "description": "defines auth params required for connecting to rest service.",
                  "properties": {
                      "host": {
                          "type": "string",
                          "description": "Enter resource url host path."
                      },
                      "username": {
                          "description": "Username to connect mailChimp endpoint.",
                          "type": "string"
                      },
                      "password": {
                          "description": "Password to connect mailChimp endpoint.",
                          "type": "string",
                          "format": "password"
                      }
                  },
                  "required": [
                      "host",
                      "username",
                      "password"
                  ]
              }
          }
      ],
      "sourceSpec": {
          "attributes": {
              "uiAttributes": {
                  "isSource": true,
                  "isPreview": true,
                  "isBeta": true,
                  "icon": {
                      "key": "mailchimpMembersIcon"
                  },
                  "description": {
                      "key": "mailchimpMembersDescription"
                  },
                  "label": {
                      "key": "mailchimpMembersLabel"
                  }
              },
              "urlParams": {
                  "path": "/3.0/lists/${listId}/members",
                  "method": "GET"
              },
              "contentPath": "$.members",
              "paginationParams": {
                  "type": "OFFSET",
                  "limitName": "count",
                  "limitValue": "100",
                  "offSetName": "offset"
              },
              "scheduleParams": {
                  "scheduleStartParamName": "since_last_changed",
                  "scheduleEndParamName": "before_last_changed",
                  "scheduleStartParamFormat": "yyyy-MM-ddTHH:mm:ss:fffffffK",
                  "scheduleEndParamFormat": "yyyy-MM-ddTHH:mm:ss:fffffffK"
              }
          },
          "spec": {
              "$schema": "http://json-schema.org/draft-07/schema#",
              "type": "object",
              "description": "Define user input parameters to fetch resource values.",
              "properties": {
                  "listId": {
                      "type": "string",
                      "description": "listId for which members need to fetch."
                  }
              }
          }
      },
      "exploreSpec": {
          "name": "Resource",
          "type": "Resource",
          "requestSpec": {
              "$schema": "http://json-schema.org/draft-07/schema#",
              "type": "object"
          },
          "responseSpec": {
              "$schema": "http: //json-schema.org/draft-07/schema#",
              "type": "object",
              "properties": {
                  "format": {
                      "type": "string"
                  },
                  "schema": {
                      "type": "object",
                      "properties": {
                          "columns": {
                              "type": "array",
                              "items": {
                                  "type": "object",
                                  "properties": {
                                      "name": {
                                          "type": "string"
                                      },
                                      "type": {
                                          "type": "string"
                                      }
                                  }
                              }
                          }
                      }
                  },
                  "data": {
                      "type": "array",
                      "items": {
                          "type": "object"
                      }
                  }
              }
          }
      }
  }'
```

**応答**

正常な応答は、新しく作成された接続仕様 ( 一意の `id`.

```json
{
    "id": "f6c0de0c-0a42-4cd9-9139-8768bf2f1b55",
    "createdAt": 1633388930134,
    "updatedAt": 1633388930134,
    "createdBy": "{CREATED_BY}",
    "updatedBy": "{UPDATED_BY}",
    "createdClient": "{CREATED_CLIENT}",
    "updatedClient": "{UPDATED_CLIENT}",
    "sandboxId": "{SANDBOX_ID}",
    "sandboxName": "{SANDBOX_NAME}",
    "imsOrgId": "{IMS_ORG}",
    "name": "MailChimp Members source",
    "providerId": "0ed90a81-07f4-4586-8190-b40eccef1c5a",
    "version": "1.0",
    "type": "generic-rest",
    "authSpec": [
        {
            "name": "OAuth2 Refresh Code",
            "type": "OAuth2RefreshCode",
            "spec": {
                "$schema": "http://json-schema.org/draft-07/schema#",
                "type": "object",
                "description": "Define auth params required for connecting to generic rest using oauth2 authorization code.",
                "properties": {
                    "host": {
                        "type": "string",
                        "description": "Enter resource url host path"
                    },
                    "authorizationTestUrl": {
                        "description": "Authorization test url to validate accessToken.",
                        "type": "string"
                    },
                    "accessToken": {
                        "description": "Access Token of mailChimp endpoint.",
                        "type": "string",
                        "format": "password"
                    }
                },
                "required": [
                    "host",
                    "accessToken"
                ]
            }
        },
        {
            "name": "Basic Authentication",
            "type": "BasicAuthentication",
            "spec": {
                "$schema": "http://json-schema.org/draft-07/schema#",
                "type": "object",
                "description": "defines auth params required for connecting to rest service.",
                "properties": {
                    "host": {
                        "type": "string",
                        "description": "Enter resource url host path."
                    },
                    "username": {
                        "description": "Username to connect mailChimp endpoint.",
                        "type": "string"
                    },
                    "password": {
                        "description": "Password to connect mailChimp endpoint.",
                        "type": "string",
                        "format": "password"
                    }
                },
                "required": [
                    "host",
                    "username",
                    "password"
                ]
            }
        }
    ],
    "sourceSpec": {
        "spec": {
            "$schema": "http://json-schema.org/draft-07/schema#",
            "type": "object",
            "description": "Define user input parameters to fetch resource values.",
            "properties": {
                "listId": {
                    "type": "string",
                    "description": "listId for which members need to fetch."
                }
            }
        },
        "attributes": {
            "uiAttributes": {
                "isSource": true,
                "isPreview": true,
                "isBeta": true,
                "icon": {
                    "key": "mailchimpMembersIcon"
                },
                "description": {
                    "key": "mailchimpMembersDescription"
                },
                "label": {
                    "key": "mailchimpMembersLabel"
                }
            },
            "urlParams": {
                "path": "/3.0/lists/${listId}/members",
                "method": "GET"
            },
            "contentPath": {
                    "path": "$.members",
                    "skipAttributes": [
                      "_links",
                      "total_items",
                      "list_id"
                    ],
                    "overrideWrapperAttribute": "member"
                  },
            "paginationParams": {
                "type": "OFFSET",
                "limitName": "count",
                "limitValue": "100",
                "offSetName": "offset"
            },
            "scheduleParams": {
                "scheduleStartParamName": "since_last_changed",
                "scheduleEndParamName": "before_last_changed",
                "scheduleStartParamFormat": "yyyy-MM-ddTHH:mm:ss:fffffffK",
                "scheduleEndParamFormat": "yyyy-MM-ddTHH:mm:ss:fffffffK"
            }
        }
    },
    "exploreSpec": {
        "name": "Resource",
        "type": "Resource",
        "requestSpec": {
            "$schema": "http://json-schema.org/draft-07/schema#",
            "type": "object"
        },
        "responseSpec": {
            "$schema": "http: //json-schema.org/draft-07/schema#",
            "type": "object",
            "properties": {
                "format": {
                    "type": "string"
                },
                "schema": {
                    "type": "object",
                    "properties": {
                        "columns": {
                            "type": "array",
                            "items": {
                                "type": "object",
                                "properties": {
                                    "name": {
                                        "type": "string"
                                    },
                                    "type": {
                                        "type": "string"
                                    }
                                }
                            }
                        }
                    }
                },
                "data": {
                    "type": "array",
                    "items": {
                        "type": "object"
                    }
                }
            }
        }
    },
    "attributes": {
        "uiAttributes": {
            "apiFeatures": {
                "explorePaginationSupported": false
            }
        }
    }
}
```

## 次の手順

新しい接続仕様を作成したら、対応する接続仕様 ID を既存のフロー仕様に追加する必要があります。 に関するチュートリアルを参照してください。 [フロー仕様の更新](./update-flow-specs.md) を参照してください。

作成した接続仕様を変更するには、次のチュートリアルを参照してください。 [接続仕様の更新](./update-connection-specs.md).
