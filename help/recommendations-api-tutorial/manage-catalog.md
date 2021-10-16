---
title: API を使用したRecommendationsカタログの管理方法
description: チュートリアルのこの部分では、Adobe Target API を使用してRecommendationsカタログ内のエンティティを作成、更新、保存、取得、削除するために必要な手順を開発者に説明します。
role: Developer
level: Intermediate
topic: Personalization, Administration, Integrations, Development
feature: APIs/SDKs, Recommendations, Administration & Configuration
doc-type: tutorial
kt: 3815
author: Judy Kim
exl-id: 8060b69b-e8e5-4fe7-895f-742410d8ed45
source-git-commit: 342e02562b5296871638c1120114214df6115809
workflow-type: tm+mt
source-wordcount: '903'
ht-degree: 2%

---

# API を使用した [!DNL Recommendations] カタログの管理

この時点で、JWT 認証フローを使用してアクセストークンを生成し、Adobe Target Admin API をAdobe I/Oで使用する方法を学びました。

[Recommendations API](https://developers.adobetarget.com/api/recommendations/) を使用して、recommendations カタログの項目の追加、更新、削除をおこなうことができます。 残りのAdobe Target Admin API と同様に、[!DNL Recommendations] API では認証が必要です。

>[!TIP]
>
>**[!UICONTROL IMS を送信します。JWT 認証用のアクセストークンを更新する必要が生じた場合は常に、ユーザートークン]** リクエストを介して+認証を生成します。これは、認証が 24 時間後に期限切れになるからです。 手順については、「[AdobeAPI 認証の設定 ](../apis/configure-io-target-integration.md)」を参照してください。

![JWT3ff](assets/configure-io-target-jwt3ff.png)

>[!NOTE]
>
>先に進む前に、[Recommendations Postman コレクション ](https://developers.adobetarget.com/api/recommendations/#section/Postman) を取得します。

## エンティティの保存 API を使用した項目の作成と更新

CSV 製品フィードや [!DNL Target] 要求が製品ページで実行されるのではなく、API を使用して [!DNL Recommendations] 製品データベースを設定するには、[Save Entities API](https://developers.adobetarget.com/api/recommendations/#operation/saveEntities) を使用します。 このリクエストは、1 つの [!DNL Target] 環境内の項目を追加または更新します。 構文は次のとおりです。

```
POST https://mc.adobe.io/{{TENANT_ID}}/target/recs/entities
```

例えば、「保存エンティティ」を使用すると、在庫や価格のしきい値など、特定のしきい値を満たすたびに品目を更新し、品目にフラグを付けてレコメンデーションを実行しないようにできます。

1. **[!DNL Target]/ [!UICONTROL  セットアップ ] / [!UICONTROL  ホスト ] / [!UICONTROL  環境]** に移動して、項目を追加または更新する [!DNL Target] 環境 ID を取得します。

   ![SaveEntities1](assets/SaveEntities01.png)

2. `TENANT_ID` と `API_KEY` が、先ほど確立した Postman 環境変数を参照していることを確認します。 以下の画像を比較に使用してください。 必要に応じて、API リクエストの Headers とパスを、以下の画像のヘッダーとパスに一致するように変更します。

   ![SaveEntities3](assets/SaveEntities03.png)

3. JSON に **raw** コードを **本文** に入力します。 `environment` 変数を使用して、必ず環境 ID を指定してください。 （以下の例では、環境 ID は 6781 です）。

   ![SaveEntities4.png](assets/SaveEntities04.png)

   >!![NOTE]
   以下に、Toaster Oven 製品の関連エンティティ値を含む entity.id kit2001 を環境 6781 に追加する JSON のサンプルを示します。

   ```
      {
      "entities": [{
              "name": "Toaster Oven",
              "id": "kit2001",
              "environment": 6781,
              "categories": [
                  "housewares:appliances"
              ],
              "attributes": {
                  "inventory": 77,
                  "margin": 23,
                  "message": "crashing helicopter",
                  "pageUrl": "www.foobar.foo.com/helicopter.html",
                  "thumbnailUrl": "www.foobar.foo.com/helicopter.jpg",
                  "value": 19.2
              }
          }]
      }
   ```

4. 「**送信**」をクリックします。次の応答が返されます。

   ![SaveEntities5.png](assets/SaveEntities05.png)

JSON オブジェクトは、複数の製品を送信するように拡大/縮小できます。 例えば、この JSON は 2 つのエンティティを指定します。

```
    {
        "entities": [{
                "name": "Toaster Oven",
                "id": "kit2001",
                "environment": 6781,
                "categories": [
                    "housewares:appliances"
                ],
                "attributes": {
                    "inventory": 89,
                    "margin": 11,
                    "message": "Toaster Oven",
                    "pageUrl": "www.foobar.foo.com/helicopter.html",
                    "thumbnailUrl": "www.foobar.foo.com/helicopter.jpg",
                    "value": 102.5
                }
            },
            {
                "name": "Blender",
                "id": "kit2002",
                "environment": 6781,
                "categories": [
                    "housewares:appliances"
                ],
                "attributes": {
                    "inventory": 36,
                    "margin": 5,
                    "message": "Blender",
                    "pageUrl": "www.foobar.foo.com/helicopter.html",
                    "thumbnailUrl": "www.foobar.foo.com/helicopter.jpg",
                    "value": 54.5
                }
            }
        ]
    }
```

1. 今度は君の番だ！ **エンティティの保存** API を使用して、次の項目をカタログに追加します。 上記のサンプル JSON を基にしてください。 （JSON を拡張して、追加のエンティティを含める必要があります）。

   ![SaveEntities6.png](assets/SaveEntities06.png)

ああ、最後の 2 つのアイテムは属していないようだ。 ここでは、**Get Entity** API を使用して検査し、必要に応じて **Delete Entities** API を使用して削除します。

## エンティティ取得 API を使用した品目の詳細の取得

既存の項目の詳細を取得するには、[Get Entity API](https://developers.adobetarget.com/api/recommendations/#operation/getEntity) を使用します。 構文は次のとおりです。

```
GET https://mc.adobe.io/{{TENANT_ID}}/target/recs/entities/[entity.id]
```

エンティティの詳細は、一度に 1 つのエンティティに対してのみ取得できます。 「エンティティを取得」を使用して、カタログが期待どおりに更新されたことを確認したり、カタログの内容を監査したりできます。

1. API リクエストで、変数 `entityId` を使用してエンティティ ID を指定します。 次の例は、entityId=kit2004 のエンティティの詳細を返します。

   ![GetEntity1](assets/GetEntity1.png)

2. `TENANT_ID` と `API_KEY` が、先ほど確立した Postman 環境変数を参照していることを確認します。 以下の画像を比較に使用してください。 必要に応じて、API リクエストの Headers とパスを、以下の画像のヘッダーとパスに一致するように変更します。

   ![GetEntity2](assets/GetEntity2.png)

3. リクエストを送信します。

   ![GetEntity3](assets/GetEntity3.png)
上の例に示すように、エンティティが見つからなかったことを示すエラーが表示された場合は、リクエストを正しい環境に送信していることを確認し [!DNL Target] ます。

   >[!NOTE]
   環境が明示的に指定されていない場合、Get Entity は、[ デフォルト環境 ](https://experienceleague.adobe.com/docs/target/using/administer/hosts.html?lang=en) からのみエンティティを取得しようとします。 デフォルト環境以外の環境から取り込む場合は、環境 ID を指定する必要があります。

4. 必要に応じて、`environmentId` パラメーターを追加し、要求を再送信します。

   ![GetEntity4](assets/GetEntity4.png)

5. 別の **Get Entity** リクエストを送信します。今回は、entityId=kit2005 のエンティティを調べます。

   ![GetEntity5](assets/GetEntity5.png)

これらのエンティティをカタログから削除する必要があると判断したとします。 **エンティティを削除** API を使用します。

## エンティティ削除 API を使用した品目の削除

カタログから項目を削除するには、[ エンティティを削除 API](https://developers.adobetarget.com/api/recommendations/#operation/deleteEntities) を使用します。 構文は次のとおりです。

```
DELETE https://mc.adobe.io/{{TENANT_ID}}/target/recs/entities?ids=[comma-delimited-entity-ids]&environment=[environmentId]
```

>[!WARNING]
この API は、指定した ID で参照されているエンティティを削除します。
エンティティ ID を指定しない場合、指定した環境内のすべてのエンティティが削除されます。 環境 ID を指定しない場合、エンティティはすべての環境から削除されます。 これは注意して使用してください。

1. **[!DNL Target]/ [!UICONTROL  セットアップ ] / [!UICONTROL  ホスト ] / [!UICONTROL  環境]** に移動して、項目を削除する [!DNL Target] 環境 ID を取得します。

   ![DeleteEntities1](assets/SaveEntities01.png)

2. API リクエストで、`&ids=[comma-delimited-entity-ids]` （クエリパラメーター）という構文を使用して、削除するエンティティのエンティティ ID を指定します。 複数のエンティティを削除する場合は、コンマで ID を区切ります。

   ![DeleteEntities2](assets/DeleteEntities2.png)

3. 構文 `&environment=[environmentId]` を使用して環境 ID を指定します。そうしないと、すべての環境のエンティティが削除されます。

   ![DeleteEntities3](assets/DeleteEntities3.png)

4. `TENANT_ID` と `API_KEY` が、先ほど確立した Postman 環境変数を参照していることを確認します。 以下の画像を比較に使用してください。 必要に応じて、API リクエストの Headers とパスを、以下の画像のヘッダーとパスに一致するように変更します。

   ![DeleteEntities4](assets/DeleteEntities4.png)

5. リクエストを送信します。

   ![DeleteEntities5](assets/DeleteEntities5.png)

6. 「**エンティティ** を取得」を使用して結果を確認します。これは、削除されたエンティティが見つからないことを示すはずです。

   ![DeleteEntities6](assets/DeleteEntities6.png)

   ![DeleteEntities6](assets/DeleteEntities7.png)

おめでとう！ [!DNL Recommendations] API を使用して、カタログ内のエンティティの詳細を作成、更新、削除、取得できるようになりました。 次の節では、カスタム条件の管理方法を学びます。

[次：「カスタム条件の管理」>](manage-custom-criteria.md)
