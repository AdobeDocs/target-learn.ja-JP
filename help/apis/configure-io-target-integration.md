---
title: Adobe TargetAPIの認証の設定
keywords: recommendations;adobe recommendations;premium;api;apis
description: Adobe TargetRecommendationsには、レコメンデーション可能な商品やコンテンツのカタログを管理するためのAPIの専用セットが含まれています。レコメンデーションのアルゴリズムとキャンペーンを管理します。Web、モバイル、電子メール、IOTなどのチャネルに表示するJSON、HTMLまたはXMLオブジェクトでレコメンデーションを配信します。
kt: null
audience: developer
doc-type: tutorial
activity: use
feature: api
topics: recommendations;adobe recommendations;premium;api;apis
solution: Target
author: Judy Kim
translation-type: tm+mt
source-git-commit: 624172d4bc4bc2431ad8af0956c93d3bcc0b9870
workflow-type: tm+mt
source-wordcount: '1885'
ht-degree: 2%

---


# Adobe TargetAPIの認証の設定

管理APIを含むAdobe Target管理APIは、認証によって保護され、許可されたユーザーのみがAdobe Targetにアクセスするために使用できるようになります。 [!DNL Recommendations] AdobeDeveloper Console [(Developer Console](https://console.adobe.io/) )を使用して、を含むすべてのAdobe Experience Cloudソリューションでこの認証を管理し [!DNL Target]ます。

このレッスンでは、Adobe TargetAPIとのやり取りを正しく行うために必要な認証トークンを生成するために必要な暫定的な手順について説明します。 以降の節では、次の操作を行います。

1. Adobeデベロッパーコンソールでプロジェクト（旧称統合）を作成します。
2. プロジェクトの詳細をPostmanにエクスポートします。
3. ベアラアクセストークンを生成します。
4. bearerアクセストークンをテストします。

## 前提条件

| リソース | 詳細 |
| --- | --- |
| 郵便配達人 | これらの手順を正常に完了するには、ご使用のオペレーティングシステムの [Postmanアプリ](https://www.postman.com/downloads/) を入手してください。 Postman Basicはアカウントの作成で自由です。 一般的にAdobe TargetAPIを使用するために必須ではありませんが、PostmanはAPIワークフローを容易にし、Adobe TargetはAPIの実行や動作の学習に役立ついくつかのポストマンコレクションを提供しています。 このチュートリアルの残りの部分は、Postmanの作業知識を前提としています。 援助が必要な場合は、 [ポストマンのドキュメントを参照してください](https://learning.getpostman.com/)。 |
| リファレンス | このチュートリアルの残りの部分では、次のリソースに精通していることを前提としています。<UL><li>[AdobeI/O Github](https://github.com/adobeio)</li><li>[ターゲットAdobeI/Oドキュメント](https://developers.adobetarget.com/api/#introduction)</li><li>[RecommendationsAPIドキュメント](https://developers.adobetarget.com/api/recommendations/)</li></ul> |

## AdobeI/Oプロジェクトの作成

この節では、Adobeデベロッパーコンソールにアクセスし、のプロジェクトを作成し [!DNL Adobe Target]ます。 詳しくは、プロジェクトの [ドキュメントを参照してください](https://www.adobe.io/apis/experienceplatform/console/docs.html#!AdobeDocs/adobeio-console/master/projects.md)。

<!--1. Generate your private key and public certificate, per the [documentation on authentication](https://www.adobe.io/authentication/auth-methods.html#!AdobeDocs/adobeio-auth/master/JWT/JWTCertificate.md). //<!--as described in **Step 1** of [How to set up Adobe IO: Authentication - Step by Step](https://helpx.adobe.com/marketing-cloud-core/kb/adobe-io-authentication-step-by-step.html). After completing Step 1, return to this tutorial and resume with Step 2, below. // The outcome of this step should be the creation of a `private.key` file and a `certificate_pub.crt` file. Return to this tutorial once you have generated these two files.-->

1. [Adobe Admin Console](https://adminconsole.adobe.com/)で、Adobeユーザーアカウントに対する [製品管理者](https://helpx.adobe.com/enterprise/using/admin-roles.html) 、開発者 [レベルの両方のアクセス権が与えられていることを確認してくだ](https://helpx.adobe.com/enterprise/using/manage-developers.html)[!DNL Target]さい。

2. Adobeデベロッパーコンソール [(Developer Console](https://console.adobe.io/))で、この統合を作成するExperience Cloud組織を選択します。 (1つのExperience Cloud組織にしかアクセスできない場合があります)。

   ![configure-io-ターゲット-createproject2.png](assets/configure-io-target-createproject2.png)

3. Click **[!UICONTROL Create new project]**.

   ![configure-io-ターゲット-createproject3.png](assets/configure-io-target-createproject3.png)

4. 「 **** 追加 API」をクリックしてREST APIをプロジェクトに追加し、Adobeサービスと製品にアクセスします。

   ![追加API](assets/configure-io-target-createproject4.png)

5. 統合 **[!DNL Adobe Target]** するAdobeサービスを選択します。 表示される「 **[!UICONTROL 次へ]** 」ボタンをクリックします。

   ![configure-io-target-createproject5](assets/configure-io-target-createproject5.png)

6. 公開鍵と秘密鍵を、ターゲット用に作成するサービスアカウント統合に関連付けるオプションを選択します。 このチュートリアルでは、 **[!UICONTROL オプション1を選択します。キーペアを生成し]** 、「キーペアを **[!UICONTROL 生成]**」をクリックします。
   ![configure-io-target-createproject6](assets/configure-io-target-createproject6.png)

7. 結果をメモしておけ 指示に従って、自動的にダウンロードされた設定ファイル(`config`)に秘密鍵が含まれていることを控えておきます。 「**[!UICONTROL 次へ]**」をクリックします。
   ![configure-io-target-createproject7](assets/configure-io-target-createproject7.png)
8. ファイルシステムで、の場所を確認します。こ `config`れは、前の手順で作成した圧縮設定ファイルです。 この `config` ファイルには、秘密鍵が含まれています。後で必要になります。 ファイルシステム内の正確な場所は、ここに示す場所とは異なる場合があります。
   ![configure-io-target-createproject8](assets/configure-io-target-createproject8.png)
9. Adobeデベロッパーコンソールに戻り、使用しているプロパティに [対応する製品プロファイル](https://helpx.adobe.com/enterprise/using/manage-products-and-profiles.html) を選択し [!DNL Recommendations]ます。 （プロパティを使用しない場合は、「デフォルトのワークスペース」オプションを選択します）。 「設定済みAPI **[!UICONTROL を保存]**」をクリックします。
   ![configure-io-target-createproject9](assets/configure-io-target-createproject9.png)

10. 「統合を **[!UICONTROL 作成]**」をクリックします。 APIが正常に設定されたことを示す一時メッセージが表示されます。

11. 最後の手順として、プロジェクトの名前を元の名前よりも意味のある名前に変更し `Project 1`ます。 これを行うには、図に示すようにナビゲーションパスを使用してプロジェクトに移動し、「プロジェクトを **[!UICONTROL 編集]** 」をクリックして「**[!UICONTROL プロジェクトを編集] 」モーダルにアクセスし、プロジェクトの名前を変更します。

![configure-io-target-createproject11](assets/configure-io-target-createproject11.png)

>[!NOTE]
> 
>このチュートリアルでは、プロジェクトに「ターゲット統合」という名前を付けます。 プロジェクトを単なるAdobe Target以外に使用することを想定している場合は、それに応じて名前を付けることができます。 例えば、Adobe Experience Cloudの他のソリューションと共に使用できるので、「AdobeAPI」または「Experience CloudAPI」という名前を付けることができます。

## プロジェクトの詳細のエクスポート

アクセスに使用できるAdobeプロジェクトがあったので [!DNL Target]、そのプロジェクトの詳細とAdobeAPIリクエストを送信する必要があります。 これらの詳細は、複数のAPIを含む複数のAdobeAPIとやり取りするために必要で [!DNL Target] す。 例えば、統合の詳細には、 [!DNL Target] 管理者APIで必要な認証情報と認証情報が含まれます。 したがって、PostmanでAPIを使用するには、これらの詳細をPostmanに入手する必要があります。

Postmanではプロジェクトの詳細を指定する方法は多数ありますが、この節では、事前にビルドされた機能やコレクションを利用します。 最初に（この節で）統合の詳細をPostman環境にエクスポートします。 次に（次の節で）ベアラアクセストークンを生成し、必要なAdobeリソースへのアクセスを許可します。

>[!NOTE]
>
>など、どのExperience Cloudソリューションにも適用できるビデオ手順につ [!DNL Target]いては、「PostmanとExperience PlatformAPIの [使用](https://docs.adobe.com/content/help/en/platform-learn/tutorials/apis/postman.html)」を参照してください。 以下の節はAPIに関連してい [!DNL Target] ます。
>
> 1. AdobeI/O統合の詳細をPostmanにエクスポート
> 2. ポストマンでのアクセストークンの生成

>
> 
これらの手順も以下に示します。

1. 引き続き、 [Adobeデベロッパーコンソールで](https://console.adobe.io/)、新しいプロジェクトの **[!UICONTROL サービスアカウント(JWT)]** 資格情報を表示に移動します。 図に示すように、左側のナビゲーションまたは **[!UICONTROL 資格情報]** (Credentials)セクションを使用します。
   ![JWT1](assets/configure-io-target-jwt1.png)**[!UICONTROL 秘密鍵証明書の詳細]**( **公開鍵**、 **クライアントID**、およびサービスアカウントに関連するその他の情報を表示できます)。
   ![JWT1a](assets/configure-io-target-jwt1a.png)
2. をクリックして、 **[!UICONTROL Adobe Target]** APIに関する情報に移動します。 図に示すように、左側のナビゲーションまたは「 **[!UICONTROL 接続された製品とサービス]** 」セクションを使用します。
   ![JWT2](assets/configure-io-target-jwt2.png)
3. 「 **[!UICONTROL Postman用に]** ダウンロード **[!UICONTROL 」/「]** サービスアカウント」(JWT)をクリックして、Postman環境用の認証情報を取り込むJSONファイルを作成します。
   ![JWT3](assets/configure-io-target-jwt3.png)ファイルシステムにJSONファイルを書き留めます。
   ![JWT3a](assets/configure-io-target-jwt3a.png)
4. Postmanで、歯車アイコンをクリックして環境を管理し、「 **読み込み** 」をクリックしてJSONファイル(環境)を読み込みます。
   ![JWT4](assets/configure-io-target-jwt4.png)
5. ファイルを選択し、「 **開く**」をクリックします。
   ![JWT5](assets/configure-io-target-jwt5.png)
6. 「Postman **環境の管理** 」モーダルで、新しく読み込んだ環境の名前をクリックして検証します。 (環境名は、ここに表示されている名前とは異なる場合があります。 必要に応じて名前を編集します。 必ずしもAdobeプロジェクトの名前と一致する必要はありません)。
   ![JWT6](assets/configure-io-target-jwt6.png)
7. お `CLIENT_SECRET` よび `API_KEY` （他の変数と共に）は、Adobe開発者コンソールで定義されたとおりに、統合から取得された値が事前に設定されています。 (Postman `CLIENT_SECRET` 変数は、Developer Consoleに表示される `CLIENT SECRET` Adobe資格情報と一致する必要があります。Postmanでも、Developer Consoleで表示される `API_KEY``CLIENT ID` 資格情報と一致する必要があります)。 一方、注、 `PRIVATE_KEY`およびは空白 `JWT_TOKEN``ACCESS_TOKEN` です。 その `PRIVATE_KEY` 値を与えて開始をしよう。
   ![JWT7](assets/configure-io-target-jwt7.png)

   >[!NOTE]
   >
   >**驚き！**
   >
   >クイズ！ 秘密鍵がどこにあるか覚えてる？
   >そのとおり、Adobeデベロッパーコンソールから以前にダウンロードした `config` ファイルに含まれています。

8. ファイルシステムから、 `config` ファイルを開き、 `private` キーファイルを開きます。
   ![JWT8](assets/configure-io-target-jwt8.png)
9. キーファイルの内容全体を選択してコピーし `private` ます。
   ![JWT9](assets/configure-io-target-jwt9.png)
10. Postmanで、秘密鍵の値を「 **INITIAL VALUE** 」フィールドと「 **CURRENT VALUE** 」フィールドに貼り付けます。
   ![JWT10](assets/configure-io-target-jwt10.png)
11. 「 **[!UICONTROL 更新]**」をクリックし、環境モーダルを閉じます。


## bearerアクセストークンの生成

この節では、ベアラアクセストークンを生成します。これは、Adobe TargetAPIとの対話を認証するために必要です。 ベアラアクセストークンを生成するには、（前節で設定した）統合の詳細を [AdobeIdentity Managementサービス(IMS)に送信する必要があり](https://www.adobe.io/authentication/auth-methods.html#!AdobeDocs/adobeio-auth/master/AuthenticationOverview/AuthenticationGuide.md)ます。 これを行う方法はいくつかありますが、このチュートリアルでは、IMS APIに対する特示POSTリクエストを作成する方法を紹介します。 冗談だ。 このチュートリアルでは、プロセスを直接簡単に実行できる、事前ビルドIMS呼び出しを含むPostmanコレクションを利用します。 コレクションを読み込むと、必要に応じて再利用し、Adobe Targetだけでなく他のAdobeAPI用にも新しいトークンを生成できます。

1. Identity ManagementサービスAPIの [Adobeサンプル呼び出しに移動します](https://github.com/adobe/experience-platform-postman-samples/tree/master/apis/ims)。
   ![token1](assets/configure-io-target-generatetoken1.png)
2. [ **AdobeI/Oアクセストークン生成ポストマン]コレクションをクリックし**ます。
   ![token2](assets/configure-io-target-generatetoken2.png)
3. 「 **Raw**」をクリックし、結果のJSONをクリップボードにコピーして、このコレクションの生のJSONを取得します。 （または、生のJSONを.jsonファイルとして保存できます）。
   ![token3](assets/configure-io-target-generatetoken3.png)
4. Postmanで、生のJSONを貼り付けてクリップボードから送信し、コレクションを読み込みます。 （または、保存した.jsonファイルをアップロードすることもできます）。 「**続行**」をクリックします。
   ![token4](assets/configure-io-target-generatetoken4.png)
5. IMSを選択し **[!UICONTROL ます。AdobeI/Oアクセストークン生成ポストマンコレクションのJWT Generate + Auth via User Token]** request、環境が選択されていることを確認し、「 **送信** 」をクリックしてトークンを生成します。

   ![token5](assets/configure-io-target-generatetoken5.png)

   >[!NOTE]
   >
   >このベアラアクセストークンは24時間有効です。 新しいトークンを生成する必要が生じた場合は、必ずリクエストを再度送信します。

6. 「環境の管理」モーダルを再度開き、環境を選択します。
   ![token6](assets/configure-io-target-jwt11.png)
7. との値 `ACCESS_TOKEN``JWT_TOKEN` が入力されるようになりました。
   ![token7](assets/configure-io-target-generatetoken7.png)

>[!NOTE]
>
>Q:JSON Web Token(JWT)およびbearerアクセストークンを生成するには、AdobeI/Oアクセストークン生成ポストマンコレクションを使用する必要がありますか。
>
>A:だめ！ AdobeI/Oアクセストークン生成ポストマンコレクションは、ポストマンでJWTおよびベアラアクセストークンをより容易に生成できる利便性として利用できる。 または、Adobe開発者コンソール内の機能を使用して、ベアラアクセストークンを手動で生成できます。

## bearerアクセストークンのテスト

この実習では、新しいbearerアクセストークンを使用して、アカウントからアクティビティのリストを取得するAPIリクエストを送信します。 [!DNL Target] 正常に完了した場合は、APIを使用するために、Adobeプロジェクトと認証が期待どおりに動作していることを示します。

1. [Adobe Target管理APIのPostmanコレクションを読み込みます](https://developers.adobetarget.com/api/#admin-postman-collection)。 コレクションがPostmanに読み込まれるまで、プロンプトに従って操作します。
   ![testtoken1](assets/configure-io-target-testtoken0.png)
1. コレクションを展開し、 **[!UICONTROL リストアクティビティ]** リクエストをメモします。
   ![testtoken1](assets/configure-io-target-testtoken1.png)
1. などの変数は、最初は未解決 `{{access_token}}` です。 この問題を解決する方法はいくつかあります。例えば、という名前の新しいコレクション変数を定義する場合などです `{{access_token}}`が、このチュートリアルでは、APIリクエストを変更して以前使用したPostman環境を活用します。 これにより、環境は、AdobeAPI間で共通するすべての変数を単一で一貫性のある統合として機能し続けることができます。
   ![testtoken2](assets/configure-io-target-testtoken2.png)
1. 置き換える文字 `{{access_token}}` を入力し `{{ACCESS_TOKEN}}`ます。
   ![testtoken3](assets/configure-io-target-testtoken3.png)
1. 置き換える文字 `{{api_key}}` を入力し `{{API_KEY}}`ます。
   ![testtoken4](assets/configure-io-target-testtoken4.png)
1. 置き換える文字 `{{tenant}}` を入力し `{{TENANT_ID}}`ます。 注記 `{{TENANT_ID}}` はまだ認識されていません。
   ![testtoken4](assets/configure-io-target-testtoken4a.png)
1. 「環境の管理」モーダルを開き、環境を選択します。
   ![JWT11](assets/configure-io-target-jwt11.png)
1. 新しい `{{TENANT_ID}}` 環境変数を追加するには、と入力します。 新しい **環境変数の「** 初期値 **」フィールドと「** 現在の値 `TENANT_ID` 」フィールドに、テナントID値をコピー&amp;ペーストします。

   ![testtoken5](assets/configure-io-target-testtoken5.png)

   >[!NOTE]
   >
   >テナントIDがお客様のものと異なり [!DNL Target]`clientcode`ます。 テナントIDは、にログインしたときにURLに存在し [!DNL Target]ます。 テナントIDを取得するには、にログインし、 [!DNL Adobe Experience Cloud]を開き、カード [!DNL Target]をクリックし [!DNL Target] ます。 URLサブドメインに記載されているテナントID値を使用します。
   >
   >例えば、Adobe TargetにログインしたときのURLが
   >
   >`<https://mycompany.experiencecloud.adobe.com/...>`
   >
   >テナントIDが「mycompany」になる。

1. 正しい環境を選択したことを確認した上で、リクエストを送信します。 アクティビティのリストを含む回答を受け取ります。
   ![testtoken6](assets/configure-io-target-testtoken6.png)

おめでとう！ これでAdobe認証が検証されたので、これを使用してAdobe TargetAPI(および他のAdobeAPI)とやり取りできます。 例えば、RecommendationsAPIを [使用してレコメンデーションを作成または管理できます](https://docs.adobe.com/content/help/en/target-learn/recommendations-api-tutorial/recs-api-overview.html) 。
