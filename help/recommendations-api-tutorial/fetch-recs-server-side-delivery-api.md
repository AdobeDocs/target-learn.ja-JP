---
title: 配信APIでRecommendationsを取得する方法
description: チュートリアルのこの部分では、Adobe Target配信APIを使用してレコメンデーションコンテンツを取得するために必要な手順について、開発者に説明します。
role: Developer
level: Intermediate
topic: Personalization, Administration, Integrations, Development
feature: APIs/SDKs, Recommendations, Administration & Configuration
doc-type: tutorial
kt: 3815
thumbnail: null
author: Judy Kim
translation-type: tm+mt
source-git-commit: 2c371ea17ce38928bcf3655a0d604a69e29963a0
workflow-type: tm+mt
source-wordcount: '1459'
ht-degree: 0%

---


# 配信APIを使用して[!DNL Recommendations]を取得しています

Adobe TargetとAdobe Targetの[!DNL Recommendations] APIはWebページへの応答の配信に使用できますが、アプリ、画面、コンソール、電子メール、キオスク、その他のディスプレイデバイスを含む、HTML以外のベースのエクスペリエンスでも使用できます。 つまり、[!DNL Target]ライブラリとJavaScriptを使用できない場合でも、**[!DNL Target]配信API**&#x200B;を使用すれば、パーソナライズされたエクスペリエンスを提供するための[!DNL Target]機能の全機能にアクセスできます。

>[!NOTE]
>
> 実際のレコメンデーションを含むコンテンツ（推奨商品や商品）をリクエストする場合は、[!DNL Target]配信APIを使用します。

レコメンデーションを取得するには、Adobe Target配信APIPOST呼び出しを適切なコンテキスト情報と共に送信します。これには、ユーザーID(最近表示した品目など、プロファイル固有のレコメンデーションで使用)、関連するmbox名、mboxパラメーター、プロファイルパラメーター、その他の属性が含まれます。 この応答には、推奨されるentity.ids（および他のエンティティデータを含む場合もあります）がJSON形式またはHTML形式で含まれ、これらはデバイスに表示できます。

Adobe Targetの[配信API](https://developers.adobetarget.com/api/delivery-api/)は、標準の[!DNL Target]リクエストが提供する既存の機能をすべて公開します。

>[!NOTE]
>配信API:
>* 場所とオーディエンスのエクスペリエンスまたはオファーをRESTful方法で取得できます。
>* 認証は不要です。
>* POSTのみ。
>* cookieまたはリダイレクト呼び出しを処理しません。
>* 「ユーザーの役割」を必要としないか、認識しません。 単純にコンテンツを取得するか、[!DNL Target]エッジサーバーにイベントを報告します。


配信APIを使用して[!DNL Target]エクスペリエンス（recommendationsを含む）を提供するには、次の手順に従います。

1. （Visual Experience Composerではなく）フォームベースのコンポーザーを使用して、[!DNL Target]アクティビティ（A/B、XT、APまたは[!DNL Recommendations]）を作成します。
2. 配信APIを使用して、先ほど作成した[!DNL Target]アクティビティによって生成されたリクエストに対する応答を取得します。

<!-- Q: Why are BOTH steps necessary for this? If you have a Form-based recommendation defined for an mbox, what's the point/benefit of ALSO having the Delivery API step in to retrieve results? Why can't you just have the Form-based Rec deliver the results in the destination device...?? A: See use case below... it's when you want to "intercept" the pending results in order to do more stuff prior to displaying the results. Things like real-time comparisons to inventory levels. -->

## フォームベースのExperience Composerを使用したレコメンデーションの作成

配信APIで使用できるレコメンデーションを作成するには、[フォームベースのコンポーザー](https://docs.adobe.com/content/help/en/target/using/experiences/form-experience-composer.html)を使用します。

1. 最初に、レコメンデーションで使用するJSONベースのデザインを作成し、保存します。 サンプルJSON、およびフォームベースのアクティビティの設定時にJSON応答を返す方法に関する背景情報については、[推奨デザインの作成](https://docs.adobe.com/content/help/en/target/using/recommendations/recommendations-design/create-design.html)に関するドキュメントを参照してください。 この例では、デザインの名前は&#x200B;*シンプルJSONです。*

   ![server-side-create-recs-json-design.png](assets/server-side-create-recs-json-design.png)

2. [!DNL Target]で、**[!UICONTROL アクティビティ]/[!UICONTROL アクティビティを作成]/[!UICONTROL Recommendations]**&#x200B;に移動し、**[!UICONTROL フォーム]**&#x200B;を選択します。

   ![server-side-create-recs.png](assets/server-side-create-recs.png)

3. プロパティを選択し、「**[!UICONTROL 次へ]**」をクリックします。
4. レコメンデーションの応答を受け取るユーザーの場所を定義します。 次の例では、*api_charter*&#x200B;という名前の場所を使用しています。 *シンプルJSONという名前で、以前に作成したJSONベースのデザインを選択します。*
   ![server-side-create-recs-form.png](assets/server-side-create-recs-form1.png)
5. レコメンデーションを保存してアクティブ化します。 結果が生成されます。 [結果の準備が整ったら](https://docs.adobe.com/content/help/en/target/using/recommendations/recommendations-activity/previewing-and-launching-your-recommendations-activity.html)、配信APIを使用して結果を取得できます。

## 配信APIの使用

[配信API](https://developers.adobetarget.com/api/delivery-api/#tag/Delivery-API)の構文：

`POST https://{{CLIENT_CODE}}.tt.omtrdc.net/rest/v1/delivery`

1. クライアントコードは必須です。 お知らせしますが、お客様のクライアントコードは、**[!UICONTROL Recommendations] > [!UICONTROL 設定]**&#x200B;に移動すると、Adobe Targetで見つかるかもしれません。 **[!UICONTROL Recommendation APIトークン]**&#x200B;セクションの&#x200B;**[!UICONTROL クライアントコード]**の値に注意してください。
   ![client-code.png](assets/client-code.png)
1. クライアントコードを入手したら、配信API呼び出しを作成します。 以下の例は、[配信APIポストマンコレクション](https://developers.adobetarget.com/api/delivery-api/#section/Getting-Started/Postman-Collection)に提供された&#x200B;**[!UICONTROL Webバッチ処理されたmbox配信API呼び出し]**&#x200B;で始まり、関連する変更を加えています。 次に例を示します。
   * **browser**&#x200B;および&#x200B;**address**&#x200B;オブジェクトは、HTML以外の使用例では不要なので、**Body**&#x200B;から削除されました。
   * *api_* charterisは、この例の場所名として表示されます
   * entity.idが指定されているのは、このレコメンデーションがコンテンツの類似性に基づいているためです。これには、現在の項目キーを[!DNL Target]に渡す必要があります。
      ![server-side-配信-API-call.](assets/server-side-delivery-api-call2.png)
pngクエリパラメーターを正しく設定する必要があります。例えば、 
`{{CLIENT_CODE}}`が必要です。<!--Q: In the updated call syntax, entity.id is listed as a profileParameter instead of an mboxParameter as in older versions. --> <!--Q: Old image ![server-side-create-recs-post.png](assets/server-side-create-recs-post.png) Old accompanying text: "Note this recommendation is based on Content Similar products based on the entity.id sent via mboxParameters." -->
      ![client-code3](assets/client-code3.png)
1. リクエストを送信します。 この処理は、*api_charter*&#x200B;の場所に対して実行されます。この場所では、アクティブなレコメンデーションが実行され、JSONデザインで定義され、レコメンデーションされたエンティティのリストが出力されます。
1. JSONデザインに基づいて応答を受け取ります。
   ![server-side-create-recs-json-response2.](assets/server-side-create-recs-json-response2.png)
png応答には、キーIDと推奨エンティティのエンティティIDが含まれます。

この方法で配信APIを[!DNL Recommendations]と共に使用すると、HTML以外のデバイスで訪問者にレコメンデーションを表示する前に、追加の手順を実行できます。 例えば、配信APIからの応答を受け取って、最終的な結果を表示する前に、別のシステム（CMS、PIM、eコマースプラットフォームなど）からエンティティ属性の詳細（在庫、価格、評価など）を追加のリアルタイムで参照できます。

このチュートリアルで概要を説明するアプローチを使用すると、[!DNL Target]からの回答を活用して、パーソナライズされたレコメンデーションを提供するアプリを利用できます。

## 実装例

次のリソースは、HTMLに重点を置いていない様々な実装の例を示しています。 システムとデバイスが関係するので、実装はすべて一意になることに注意してください。

| リソース | 詳細 |
| --- | --- |
| [AEMでのRESTful APIの使用](https://helpx.adobe.com/experience-manager/using/restful-services.html) | サードパーティのRESTful Webサービスのデータを使用するAdobe Experience ManagerOSGiバンドルを作成および展開する方法。 |
| [Adobe Targetの至る所に — サーバ側またはIoTに実装](https://expleague.azureedge.net/labs/L733/index.html) | Adobe Targetのサーバー側APIを使用するReactアプリケーションに対する実践体験を提供するAdobeサミット2019ラボ。 |
| [AdobeSDKを使用しないモバイルアプリでのAdobe Target](https://community.tealiumiq.com/t5/Universal-Data-Hub/Adobe-Target-in-a-Mobile-App-Without-the-Adobe-SDK/ta-p/26753) | このガイドでは、AdobeSDKをインストールせずにモバイルアプリでAdobe Targetを設定する方法を説明します。 このソリューションでは、Tealium SDK WebビューとRemote Commandsモジュールを使用して、Adobe訪問者API(Experience Cloud)とAdobe TargetAPIにリクエストを送受信します。 |
| [モバイルアプリでのAdobe Targetの仕組み](https://docs.adobe.com/content/help/en/target/using/implement-target/mobile-apps/mobile-how-target-works-mobile-apps.html) | [!DNL Target]とモバイルSDKの連携 |
| [API [!DNL Target] extension in Experience Platform Launch and Implementing [!DNL Target] の設定](https://aep-sdks.gitbook.io/docs/using-mobile-extensions/adobe-target) | Experience Platform Launchでの[!DNL Target]拡張機能の設定、アプリに[!DNL Target]拡張機能の追加、アクティビティの要求、オファーのプリフェッチ、ビジュアルプレビューモードの開始のための[!DNL Target] APIの実装の手順を説明します。 |
| [Adobe Targetノードクライアント](https://www.npmjs.com/package/@adobe/target-nodejs-sdk) | オープンソースの[!DNL Target] Node.js SDK v1.0 |
| [サーバー側の概要](https://docs.adobe.com/content/help/en/target/using/implement-target/server-side/api-and-sdk-overview.html) | Adobe Targetサーバ側配信API、サーバ側バッチ配信API、Node.js SDK、Adobe Target[!DNL Recommendations] APIに関する情報です。 |
| [Adobe CampaignコンテンツRecommendations（電子メール）](https://medium.com/adobetech/adobe-campaign-content-recommendations-in-email-b51ced771d7f) | Adobe CampaignのAdobe TargetやAdobe I/O Runtime経由の電子メールでコンテンツのレコメンデーションを活用する方法を説明するブログ。 |

## APIを使用した[!DNL Recommendations]セットアップの管理

ほとんどの場合、レコメンデーションはAdobe TargetUIで設定され、[!DNL Target] APIを使用またはアクセスします。これは、上の節で説明したような理由からです。 このUI-APIの調整は一般的です。 ただし、場合によっては、設定と結果の使用の両方のAPIを使用して、すべての操作を実行したい場合があります。 一般的ではありませんが、ユーザーは絶対に設定、実行、*および*&#x200B;を行うことができます。これらの結果は、APIを完全に使用したレコメンデーションの結果を利用できます。

[前の](manage-catalog.md)節で、Adobe TargetRecommendationsのエンティティを管理し、サーバ側で配信する方法を学びました。 同様に、Adobe I/Oを使用すると、Adobe Targetにログインすることなく、条件、プロモーション、コレクション、デザインテンプレートを管理できます。 すべての[!DNL Recommendations] APIの完全なリストは、[ここ](http://developers.adobetarget.com/api/recommendations/)にありますが、以下は参考用の要約です。

| リソース | 詳細 |
| --- | --- |
| [コレクション](http://developers.adobetarget.com/api/recommendations/#tag/Collections) | コレクションのリスト、作成、取得、編集および削除を行います。 |
| [条件](http://developers.adobetarget.com/api/recommendations/#tag/Criteria) | リストと条件の取得を参照してください。 |
| [デザイン](http://developers.adobetarget.com/api/recommendations/#tag/Designs) | デザインのリスト、作成、取得、編集、削除および検証を行います。 |
| [エンティティ](http://developers.adobetarget.com/api/recommendations/#tag/Entities) | エンティティを保存、削除、取得します。 |
| [プロモーション](http://developers.adobetarget.com/api/recommendations/#tag/Promotions) | リスト、プロモーションの作成、取得、編集、削除を行います。 |
| [カテゴリ条件](http://developers.adobetarget.com/api/recommendations/#tag/Category-Criteria) | カテゴリ条件のリスト、作成、取得、編集および削除を行います。 |
| [カスタム条件](http://developers.adobetarget.com/api/recommendations/#tag/Custom-Criteria) | カスタム条件のリスト、作成、取得、編集および削除を行います。 |
| [品目基準](http://developers.adobetarget.com/api/recommendations/#tag/Item-Criteria) | リスト条件、作成、取得、編集および削除を行います。 |
| [人気度基準](http://developers.adobetarget.com/api/recommendations/#tag/Popularity-Criteria) | 人気度条件のリスト、作成、取得、編集および削除を行います。 |
| [プロファイル属性の条件](http://developers.adobetarget.com/api/recommendations/#tag/Profile-Attribute-Criteria) | リスト、プロファイル属性条件の作成、取得、編集および削除を行います。 |
| [最近の条件](http://developers.adobetarget.com/api/recommendations/#tag/Recent-Criteria) | 最近使用した条件のリスト、作成、取得、編集および削除を行います。 |
| [シーケンス条件](http://developers.adobetarget.com/api/recommendations/#tag/Sequence-Criteria) | リスト、シーケンス条件の作成、取得、編集および削除を行います。 |

## リファレンスドキュメント

* [Adobe TargetAPIドキュメント](https://developers.adobetarget.com/api/#getting-started)
* [Adobe Target配信API](https://developers.adobetarget.com/api/delivery-api/)
* [電子メ [!DNL Recommendations] ールとの統合](https://docs.adobe.com/content/help/en/target/using/recommendations/recommendations-faq/integrating-recs-email.html)

## 概要とレビュー

おめでとう！ このチュートリアルを終了すると、次の方法を学習できます。
* [RecommendationsAPIを使用したカタログの管理](manage-catalog.md)
* [RecommendationsAPIを使用したカスタム条件の管理](manage-custom-criteria.md)
* [Recommendationsでの配信APIの使用](fetch-recs-server-side-delivery-api.md)
