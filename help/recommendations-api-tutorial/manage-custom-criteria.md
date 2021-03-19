---
title: カスタム条件の管理方法
description: チュートリアルのこの部分では、開発者がAdobe TargetAPIを使用してAdobe TargetRecommendationsの条件の管理、作成、リスト、編集、取得、削除を行うために必要な手順について説明します。
role: 開発者
level: 中間
topic: パーソナライゼーション、管理、統合、開発
feature: API/SDK、Recommendations、管理および設定
doc-type: tutorial
kt: 3815
thumbnail: null
author: Judy Kim
translation-type: tm+mt
source-git-commit: b89732fcca0be8bffc6e580e4ae0e62df3c3655d
workflow-type: tm+mt
source-wordcount: '952'
ht-degree: 1%

---


# カスタム条件の管理

[!DNL Recommendations]が提供するアルゴリズムが、プロモーションしたい特定のアイテムを表示できない場合があります。 このような場合、カスタム条件を使用すると、特定の主要品目またはカテゴリに対してレコメンデーション品目の特定のセットを配信できます。 主要品目またはカテゴリとレコメンデーション品目の間のマッピングを定義し、そのマッピングをカスタム条件として読み込みます。 このプロセスについては、[カスタム条件ドキュメント](https://docs.adobe.com/content/help/en/target/using/recommendations/criteria/recommendations-csv.html)で説明しています。 このドキュメントに記載されているように、[!DNL Target]ユーザーインターフェイス(UI)を使用して、カスタム条件を作成、編集、削除できます。 ただし、[!DNL Target]は、カスタム条件をより詳細に管理できるカスタム条件APIのセットも提供しています。

>[!IMPORTANT]
>
>カスタム条件については、次の使用ガイドラインに従います。
>
> APIを使用して、特定のカスタム条件に対してすべての操作（作成、編集、削除）を行うか、またはUIを使用してすべての操作（作成、編集、削除）を行います。 UIとAPIを組み合わせてカスタム条件を管理すると、情報が競合したり予期しない結果が生じたりする場合があります。 例えば、UIでカスタム条件を作成し、その後APIで編集した場合、APIで表示されるように、バックエンドで更新される場合でも、UIでの更新が反映されません。

## カスタム条件の作成

[カスタム条件を作成API](https://developers.adobetarget.com/api/recommendations/#operation/createCriteriaCustom)を使用してカスタム条件を作成するには、構文を次に示します。

`POST https://mc.adobe.io/{{TENANT_ID}}/target/recs/criteria/custom`

>[!WARNING]
>
>この実習で説明するように、カスタム条件を作成APIを使用して作成したカスタム条件は、UIに表示され、UIで保持されます。 UIでは、これらを編集または削除できません。 API **を使用して**&#x200B;ファイルを編集または削除できますが、どちらの方法でも、[!DNL Target] UIに引き続き表示されます。 UIからの編集または削除のオプションを維持するには、カスタム条件を作成APIではなく、[ドキュメント](https://docs.adobe.com/content/help/en/target/using/recommendations/criteria/recommendations-csv.html)ごとにUIを使用してカスタム条件を作成します。

上記の警告を読み、UIから削除できない新しいカスタム条件を作成しやすい場合にのみ、このチュートリアルを進めてください。

1. **カスタム条件**&#x200B;の`TENANT_ID`と`API_KEY`を確認し、以前に確立したポストマン環境変数を参照します。 比較には、次の画像を使用してください。

   ![CreateCustomCriteria1](assets/CreateCustomCriteria1.png)

2. 追加&#x200B;**Body**&#x200B;を&#x200B;**raw** JSONとして、カスタム条件のCSVファイルの場所を定義します。 [カスタム条件を作成API](https://developers.adobetarget.com/api/recommendations/#operation/getAllCriteriaCustom)ドキュメントに記載されている例をテンプレートとして使用し、必要に応じて`environmentId`と他の値を指定します。 この例では、LAST_PURCHASEDをキーとして使用します。

   ![CreateCustomCriteria2](assets/CreateCustomCriteria2.png)

3. リクエストを送信し、作成したカスタム条件の詳細が含まれる応答を観察します。

   ![CreateCustomCriteria3](assets/CreateCustomCriteria3.png)

4. カスタム条件が作成されたことを確認するには、Adobe Targetから&#x200B;**[!UICONTROL Recommendations] > [!UICONTROL 条件]**&#x200B;に移動し、名前で条件を検索するか、次の手順で&#x200B;**リストカスタム条件API**&#x200B;を使用します。

   ![CreateCustomCriteria4](assets/CreateCustomCriteria4.png)

この場合、エラーが発生します。 **リストカスタム条件API**&#x200B;を使用して、カスタム条件をより詳細に調べて、エラーを調査します。

## リストカスタム条件

すべてのカスタム条件のリストと各条件の詳細を取得するには、[リストカスタム条件API](https://developers.adobetarget.com/api/recommendations/#operation/getAllCriteriaCustom)を使用します。 構文：

`GET https://mc.adobe.io/{{TENANT_ID}}/target/recs/criteria/custom`

1. `TENANT_ID`と`API_KEY`を以前と同じように確認し、リクエストを送信します。 この応答では、カスタム条件IDと、前述のエラーメッセージに関する詳細をメモしておきます。
   ![ListCustomCriteria](assets/ListCustomCriteria.png)

この場合、サーバー情報が正しくないこと、つまり[!DNL Target]がカスタム条件の定義を含むCSVファイルにアクセスできないことが原因でエラーが発生しました。 カスタム条件を編集して修正しましょう。

## カスタム条件を編集

カスタム条件の定義の詳細を変更するには、[カスタム条件の編集API](https://developers.adobetarget.com/api/recommendations/#operation/updateCriteriaCustom)を使用します。 構文：

`POST https://mc.adobe.io/{{TENANT_ID}}/target/recs/criteria/custom/:criteriaId`

1. `TENANT_ID`と`API_KEY`を前と同じように確認します。
   ![EditCustomCriteria1](assets/EditCustomCriteria1.png)

1. 編集する（単一の）カスタム条件の条件IDを指定します。
   ![EditCustomCriteria2](assets/EditCustomCriteria2.png)

1. Body内で、更新したJSONに正しいサーバー情報を指定します。 （この手順では、アクセス可能なサーバーへのFTPアクセスを指定します）。
   ![EditCustomCriteria3](assets/EditCustomCriteria3.png)

1. リクエストを送信し、応答をメモします。
   ![EditCustomCriteria4](assets/EditCustomCriteria4.png)

**Get Custom Criteria API**&#x200B;を使用して、更新されたカスタム条件の成功を確認します。

## カスタム条件の取得

特定のカスタム条件のカスタム条件の詳細を表示するには、[カスタム条件の取得API](https://developers.adobetarget.com/api/recommendations/#operation/getCriteriaCustom)を使用します。 構文：

`GET https://mc.adobe.io/{{TENANT_ID}}/target/recs/criteria/custom/:criteriaId`

1. 詳細を取得するカスタム条件の条件IDを指定します。 リクエストを送信し、応答を確認します。
   ![GetCustomCriteria.png](assets/GetCustomCriteria.png)
1. 成功を確認します。 （この場合、FTPエラーがないことを確認します）。
   ![GetCustomCriteria1.png](assets/GetCustomCriteria1.png)
1. （オプション）更新がUIに正確に反映されることを確認します。
   ![GetCustomCriteria2.png](assets/GetCustomCriteria2.png)

## カスタム条件の削除

前述の条件IDを使用して、[カスタム条件の削除API](https://developers.adobetarget.com/api/recommendations/#operation/deleteCriteriaCustom)を使用してカスタム条件を削除します。 構文：

`DELETE https://mc.adobe.io/{{TENANT_ID}}/target/recs/criteria/custom/:criteriaId`

1. 削除する（単一の）カスタム条件の条件IDを指定します。 「**送信**」をクリックします。
   ![DeleteCustomCriteria1](assets/DeleteCustomCriteria1.png)

1. 「カスタム条件を取得」を使用して条件が削除されていることを確認します。
   ![DeleteCustomCriteria2](assets/DeleteCustomCriteria2.png)
この場合、予期される404エラーは、削除された条件が見つからないことを示します。

>[!NOTE]
>お知らせしますが、条件はカスタム条件を作成APIを使用して作成されたので、削除されても[!DNL Target] UIから削除されません。

おめでとう！ これで、[!DNL Recommendations] APIを使用して、カスタム条件の作成、リスト、編集、削除、および詳細の取得が可能になりました。 次の節では、[!DNL Target]配信APIを使用してレコメンデーションを取得します。

[次に、「サーバ側配信APIでRecommendationsを取得する」>](fetch-recs-server-side-delivery-api.md)
