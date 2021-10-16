---
title: Adobe Target API の認証を設定する方法
description: このチュートリアルでは、Adobe Target API とのやり取りに必要な認証トークンを生成するために必要な手順を開発者に説明します。 以下の手順に従って、Adobe開発者コンソールを使用して、Target API の使用に必要な bearer アクセストークンを生成およびテストします。
role: Developer, Admin, Architect
level: Intermediate
topic: Personalization, Administration, Integrations, Development
feature: APIs/SDKs, Administration & Configuration
doc-type: tutorial
kt: null
author: Judy Kim
exl-id: 8a1e93e4-67b2-4942-a8da-fc0f2cbb2df2
source-git-commit: 342e02562b5296871638c1120114214df6115809
workflow-type: tm+mt
source-wordcount: '1883'
ht-degree: 2%

---

# Adobe Target API の認証の設定

[!DNL Recommendations] Admin API を含むAdobe Target Admin API は、認証によって保護され、権限のあるユーザーのみがAdobe Targetにアクセスするようにします。 [Adobe開発者コンソール ](https://console.adobe.io/) を使用して、[!DNL Target] を含むすべてのAdobe Experience Cloudソリューションでこの認証を管理します。

このレッスンでは、Adobe Target API とのやり取りに必要な認証トークンを生成するために必要な事前の手順について説明します。 以降の節では、以下をおこないます。

1. プロジェクト開発者コンソールで、Adobe（旧称「統合」）を作成します。
2. プロジェクトの詳細を Postman にエクスポートします。
3. bearer アクセストークンを生成します。
4. bearer アクセストークンをテストします。

## 前提条件

| リソース | 詳細 |
| --- | --- |
| 郵便配達員 | これらの手順を正しく完了するには、お使いのオペレーティングシステムの [Postman アプリ ](https://www.postman.com/downloads/) を入手します。 Postman 基本はアカウントの作成で無料です。 Adobe Target API を一般的に使用するために必須ではありませんが、Postman は API ワークフローを容易にします。Adobe Targetは、API の実行とその動作の学習に役立つ、複数の Postman コレクションを提供します。 このチュートリアルの残りの部分は、Postman に関する実務知識を前提としています。 サポートが必要な場合は、[Postman のドキュメント ](https://learning.getpostman.com/) を参照してください。 |
| 参照 | このチュートリアルの残りの部分では、次のリソースに関する知識が必要です。<UL><li>[Adobe I/OGithub](https://github.com/adobeio)</li><li>[TargetAdobe I/Oドキュメント](https://developers.adobetarget.com/api/#introduction)</li><li>[Recommendations API ドキュメント](https://developers.adobetarget.com/api/recommendations/)</li></ul> |

## Adobe I/Oプロジェクト

この節では、Adobe開発者コンソールにアクセスし、[!DNL Adobe Target] のプロジェクトを作成します。 詳しくは、[ プロジェクトのドキュメント ](https://www.adobe.io/apis/experienceplatform/console/docs.html#!AdobeDocs/adobeio-console/master/projects.md) を参照してください。

<!--1. Generate your private key and public certificate, per the [documentation on authentication](https://www.adobe.io/authentication/auth-methods.html#!AdobeDocs/adobeio-auth/master/JWT/JWTCertificate.md). //<!--as described in **Step 1** of [How to set up Adobe IO: Authentication - Step by Step](https://helpx.adobe.com/marketing-cloud-core/kb/adobe-io-authentication-step-by-step.html). After completing Step 1, return to this tutorial and resume with Step 2, below. // The outcome of this step should be the creation of a `private.key` file and a `certificate_pub.crt` file. Return to this tutorial once you have generated these two files.-->

1. [Adobe Admin Console](https://adminconsole.adobe.com/) で、Adobeユーザーアカウントに [ 製品管理者 ](https://helpx.adobe.com/enterprise/using/admin-roles.html) と [ 開発者 ](https://helpx.adobe.com/enterprise/using/manage-developers.html) の両方のレベルで [!DNL Target] へのアクセス権が付与されていることを確認します。

2. [Adobe開発者コンソール ](https://console.adobe.io/) で、この統合を作成するExperience Cloud組織を選択します。 ( お客様は 1 つのExperience Cloud組織にのみアクセスできる場合があります )。

   ![configure-io-target-createproject2.png](assets/configure-io-target-createproject2.png)

3. 「**[!UICONTROL 新しいプロジェクトを作成]**」をクリックします。

   ![configure-io-target-createproject3.png](assets/configure-io-target-createproject3.png)

4. 「**[!UICONTROL API を追加]**」をクリックして、Adobe サービスと製品にアクセスするための REST API をプロジェクトに追加します。

   ![API を追加](assets/configure-io-target-createproject4.png)

5. 統合先のAdobe サービスとして **[!DNL Adobe Target]** を選択します。 表示される「**[!UICONTROL 次へ]**」ボタンをクリックします。

   ![configure-io-target-createproject5](assets/configure-io-target-createproject5.png)

6. 公開鍵と秘密鍵を、Target 用に作成するサービスアカウント統合に関連付けるオプションを選択します。 このチュートリアルでは、**[!UICONTROL オプション 1:キーペア]** を生成し、**[!UICONTROL キーペアを生成]** をクリックします。
   ![configure-io-target-createproject6](assets/configure-io-target-createproject6.png)

7. 結果をメモしておきます。 指示に従って、自動的にダウンロードされた設定ファイル (`config`) をメモしておきます。このファイルには、秘密鍵が含まれています。 「**[!UICONTROL 次へ]**」をクリックします。
   ![configure-io-target-createproject7](assets/configure-io-target-createproject7.png)
8. ファイルシステムで、前の手順で作成した圧縮構成ファイル `config` の場所を確認します。 この `config` ファイルには秘密鍵が含まれていますが、後で必要になります。 ファイルシステム内の正確な場所は、次に示す場所とは異なる場合があります。
   ![configure-io-target-createproject8](assets/configure-io-target-createproject8.png)
9. Adobe開発者コンソールに戻り、[!DNL Recommendations] を使用しているプロパティに対応する [ 製品プロファイル ](https://helpx.adobe.com/enterprise/using/manage-products-and-profiles.html) を選択します。 （プロパティを使用しない場合は、「デフォルトのワークスペース」オプションを選択します）。 「**[!UICONTROL 設定した API を保存]**」をクリックします。
   ![configure-io-target-createproject9](assets/configure-io-target-createproject9.png)

10. 「**[!UICONTROL 統合を作成]**」をクリックします。 API が正常に設定されたことを示す一時的なメッセージが表示されます。

11. 最後の手順として、プロジェクトの名前を元の `Project 1` よりも意味のある名前に変更します。 それには、図のようにナビゲーションパスを使用してプロジェクトに移動し、「**[!UICONTROL プロジェクトを編集]**」をクリックして「**[!UICONTROL  プロジェクトを編集 ]」モーダルにアクセスし、プロジェクトの名前を変更します。

![configure-io-target-createproject11](assets/configure-io-target-createproject11.png)

>[!NOTE]
> 
>このチュートリアルでは、プロジェクトに「Target 統合」という名前を付けます。 Adobe Target以外の用途でプロジェクトを使用することを想定している場合は、それに応じて名前を付けることができます。 例えば、Adobe Experience Cloudの他のソリューションで使用できるので、「AdobeAPI」または「Experience CloudAPI」という名前を選択できます。

## プロジェクトの詳細の書き出し

これで [!DNL Target] にアクセスするために使用できるAdobeプロジェクトが完成したので、そのプロジェクトの詳細をAdobeAPI リクエストと共に送信する必要があります。 これらの詳細は、複数の [!DNL Target] API を含む複数のAdobeAPI とやり取りするために必要です。 例えば、統合の詳細には、[!DNL Target] Admin API で必要な認証情報と認証情報が含まれます。 したがって、Postman で API を使用するには、これらの詳細を Postman に取り込む必要があります。

Postman でプロジェクトの詳細を指定する方法は多数ありますが、この節では、事前に作成された機能とコレクションを利用します。 まず、（この節の）統合の詳細を Postman 環境に書き出します。 次に（次の節で）、必要なリソースへのアクセス権を付与する bearer アクセストークンを生成します。Adobe

>[!NOTE]
>
>[!DNL Target] を含む任意のExperience Cloudソリューションに適用できるビデオ手順については、 [Experience PlatformAPI での Postman の使用 ](https://experienceleague.adobe.com/docs/platform-learn/tutorials/platform-api-authentication.html?lang=en) を参照してください。 以下の節は、[!DNL Target] API に関連します。
>
> 1. Adobe I/O統合の詳細を Postman に書き出す
> 2. Postman でのアクセストークンの生成

>
> これらの手順も以下に示します。

1. 引き続き [Adobeデベロッパーコンソール ](https://console.adobe.io/) で、新しいプロジェクトの **[!UICONTROL サービスアカウント (JWT)]** 資格情報に移動して表示します。 図のように、左側のナビゲーションまたは **[!UICONTROL 資格情報]** セクションを使用します。
   ![JWT1 ](assets/configure-io-target-jwt1.png)
秘密鍵証明 **[!UICONTROL 書の詳細]**&#x200B;で、公開鍵 **、クライアント ID** ****、およびサービスアカウントに関連するその他の情報を確認できます。
   ![JWT1a](assets/configure-io-target-jwt1a.png)
2. クリックして、**[!UICONTROL Adobe Target]** API に関する情報に移動します。 図のように、左側のナビゲーションまたは **[!UICONTROL Connected products and services]** セクションを使用します。
   ![JWT2](assets/configure-io-target-jwt2.png)
3. **[!UICONTROL Postman 用にダウンロード]** / **[!UICONTROL サービスアカウント (JWT)]** をクリックして、Postman 環境用の認証情報をキャプチャする JSON ファイルを作成します。
   ![JWT3](assets/configure-io-target-jwt3.png)
：ファイルシステムに JSON ファイルが存在することを確認します。
   ![JWT3a](assets/configure-io-target-jwt3a.png)
4. Postman で、歯車アイコンをクリックして環境を管理し、「**読み込み**」をクリックして JSON ファイル（環境）を読み込みます。
   ![JWT4](assets/configure-io-target-jwt4.png)
5. ファイルを選択し、「**開く**」をクリックします。
   ![JWT5](assets/configure-io-target-jwt5.png)
6. Postman **環境の管理** モーダルで、新しく読み込んだ環境の名前をクリックして検査します。 ( 環境名は、ここに示す名前とは異なる場合があります。 必要に応じて名前を編集します。 必ずしもAdobeプロジェクトの名前と一致する必要はありません )。
   ![JWT6](assets/configure-io-target-jwt6.png)
7. 注意： `CLIENT_SECRET` と `API_KEY`（他の変数と共に）は、Adobe開発者コンソールで定義したとおりに、統合から取得した値が事前に設定されています。 (Postman `CLIENT_SECRET` 変数は、開発者コンソールに表示される `CLIENT SECRET`Adobe資格情報に一致する必要があります。Postman の `API_KEY` 変数は、開発者コンソールの `CLIENT ID` にも一致する必要があります。) これに対して、`PRIVATE_KEY`、`JWT_TOKEN`、`ACCESS_TOKEN` は空白です。 まず、`PRIVATE_KEY` 値を指定します。
   ![JWT7](assets/configure-io-target-jwt7.png)

   >[!NOTE]
   >
   >**サプライズ！**
   >
   >ポップクイズ！ 秘密鍵がどこにあるか覚えてる？
   >そうです、以前にAdobe開発者コンソールからダウンロードした `config` ファイルにあります。

8. ファイルシステムから `config` ファイルを開き、`private` キーファイルを開きます。
   ![JWT8](assets/configure-io-target-jwt8.png)
9. `private` キーファイルの内容全体を選択してコピーします。
   ![JWT9](assets/configure-io-target-jwt9.png)
10. Postman で、秘密鍵の値を **INITIAL VALUE** フィールドと **CURRENT VALUE** フィールドに貼り付けます。
   ![JWT10](assets/configure-io-target-jwt10.png)
11. **[!UICONTROL 更新]** をクリックし、環境モーダルを閉じます。


## bearer アクセストークンの生成

この節では、Adobe Target API とのインタラクションを認証するために必要な bearer アクセストークンを生成します。 Bearer アクセストークンを生成するには、（前の節で確立した）統合の詳細を [AdobeのIdentity Managementサービス (IMS)](https://www.adobe.io/authentication/auth-methods.html#!AdobeDocs/adobeio-auth/master/AuthenticationOverview/AuthenticationGuide.md) に送信する必要があります。 これをおこなう方法はいくつかありますが、このチュートリアルでは、IMS API に対するカスタムPOSTリクエストを作成します。 冗談だ。 このチュートリアルでは、プロセスを直接簡単に実行できる、事前にビルドされた IMS 呼び出しを含む Postman コレクションを利用します。 コレクションを読み込むと、必要に応じて再利用して、Adobe Targetだけでなく他のAdobeAPI 用にも新しいトークンを生成できます。

1. [AdobeIdentity Managementサービス API のサンプル呼び出し ](https://github.com/adobe/experience-platform-postman-samples/tree/master/apis/ims) に移動します。
   ![token1](assets/configure-io-target-generatetoken1.png)
2. 「**Adobe I/Oアクセストークン生成ポストマンコレクション**」をクリックします。
   ![token2](assets/configure-io-target-generatetoken2.png)
3. **Raw** をクリックしてこのコレクションの生の JSON を取得し、結果の JSON をクリップボードにコピーします。 （または、生の JSONを.json ファイルとして保存できます）。
   ![token3](assets/configure-io-target-generatetoken3.png)
4. Postman で、生の JSON を貼り付けてクリップボードから送信し、コレクションを読み込みます。 （または、保存した.json ファイルをアップロードできます）。 「**続行**」をクリックします。
   ![token4](assets/configure-io-target-generatetoken4.png)
5. **[!UICONTROL IMS を選択します。JWTAdobe I/Oアクセストークン生成ポストマンコレクションの「ユーザートークン]** を介して+認証を生成」リクエストで、環境が選択されていることを確認し、「**送信**」をクリックしてトークンを生成します。

   ![token5](assets/configure-io-target-generatetoken5.png)

   >[!NOTE]
   >
   >この bearer アクセストークンは 24 時間有効です。 新しいトークンを生成する必要が生じたら、いつでもリクエストを送信します。

6. 環境の管理モーダルを再度開き、環境を選択します。
   ![token6](assets/configure-io-target-jwt11.png)
7. `ACCESS_TOKEN` と `JWT_TOKEN` の値が設定されていることに注意してください。
   ![token7](assets/configure-io-target-generatetoken7.png)

>[!NOTE]
>
>Q:JSON Web トークン (JWT) と bearer アクセストークンを生成するには、Adobe I/Oアクセストークン生成ポストマンコレクションを使用する必要がありますか？
>
>回答：いや！ Adobe I/Oアクセストークン生成 Postman コレクションは、Postman で JWT および bearer アクセストークンをより簡単に生成できる便利さとして利用できます。 または、Adobe開発者コンソール内の機能を使用して、bearer アクセストークンを手動で生成できます。

## bearer アクセストークンのテスト

この演習では、[!DNL Target] アカウントからアクティビティのリストを取得する API リクエストを送信して、新しい Bearer アクセストークンを使用します。 正常な応答は、Adobeプロジェクトと認証が API を使用するために期待どおりに動作していることを示します。

1. [Adobe Target Admin APIs Postman Collection](https://developers.adobetarget.com/api/#admin-postman-collection) を読み込みます。 コレクションが Postman に読み込まれるまで、すべてのプロンプトに従います。
   ![testtoken1](assets/configure-io-target-testtoken0.png)
1. コレクションを展開し、**[!UICONTROL リストアクティビティ]** リクエストをメモします。
   ![testtoken1](assets/configure-io-target-testtoken1.png)
1. `{{access_token}}` などの変数は、最初は未解決です。 この問題を解決する方法はいくつかあります（例えば `{{access_token}}` という新しいコレクション変数を定義する場合など）。ただし、このチュートリアルでは、代わりに API リクエストを変更して、以前使用していた Postman 環境を活用します。 これにより、環境は、AdobeAPI 間で共通するすべての変数を 1 つの一貫した統合として引き続き機能します。
   ![testtoken2](assets/configure-io-target-testtoken2.png)
1. `{{access_token}}` を `{{ACCESS_TOKEN}}` に置き換えるには、と入力します。
   ![testtoken3](assets/configure-io-target-testtoken3.png)
1. `{{api_key}}` を `{{API_KEY}}` に置き換えるには、と入力します。
   ![testtoken4](assets/configure-io-target-testtoken4.png)
1. `{{tenant}}` を `{{TENANT_ID}}` に置き換えるには、と入力します。 `{{TENANT_ID}}` はまだ認識されていません。
   ![testtoken4](assets/configure-io-target-testtoken4a.png)
1. 環境の管理モーダルを開き、環境を選択します。
   ![JWT11](assets/configure-io-target-jwt11.png)
1. 新しい `{{TENANT_ID}}` 環境変数を追加するには、と入力します。 新しい `TENANT_ID` 環境変数の **INITIAL VALUE** フィールドと **CURRENT VALUE** フィールドにテナント ID 値をコピーして貼り付けます。

   ![testtoken5](assets/configure-io-target-testtoken5.png)

   >[!NOTE]
   >
   >テナント ID が [!DNL Target] `clientcode` と異なります。 [!DNL Target] にログインすると、URL にテナント ID が存在します。 テナント ID を取得するには、[!DNL Adobe Experience Cloud] にログインし、[!DNL Target] を開いて [!DNL Target] カードをクリックします。 URL サブドメインで指定されているように、テナント ID 値を使用します。
   >
   >例えば、Adobe Targetにログインしたときの URL が
   >
   >`<https://mycompany.experiencecloud.adobe.com/...>`
   >
   >テナント ID は「mycompany」になります。

1. 正しい環境を選択したことを確認した後、リクエストを送信します。 アクティビティのリストを含む応答を受け取ります。
   ![testtoken6](assets/configure-io-target-testtoken6.png)

おめでとう！ これで、Adobe認証を確認したので、この認証を使用してAdobe Target API( および他のAdobeAPI) を操作できます。 例えば、[Recommendations API](https://experienceleague.adobe.com/docs/target-learn/recommendations-api-tutorial/recs-api-overview.html?lang=en) を使用して、レコメンデーションを作成または管理できます。
