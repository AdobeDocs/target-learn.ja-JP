---
title: We.Travel サンプルアプリのダウンロードと更新
description: 'We.Travel サンプルアプリは、AdobeMobile Services SDK v4 で事前に実装されています。 独自のExperience Cloud組織とソリューションアカウントを指すように更新するだけです。   '
role: Developer
level: Intermediate
topic: Mobile, Personalization
feature: Implement Mobile
doc-type: tutorial
kt: 3040
exl-id: 244bcf7a-b59b-4dd1-bd05-0a55ce7a7132
source-git-commit: 342e02562b5296871638c1120114214df6115809
workflow-type: tm+mt
source-wordcount: '520'
ht-degree: 0%

---

# We.Travel サンプルアプリのダウンロードと更新

We.Travel サンプルアプリは、AdobeMobile Services SDK v4 で事前に実装されています。 単に、独自のExperience Cloud組織とソリューションアカウントを指すように更新する必要があります。

## 学習内容

このレッスンを最後まで学習すると、次の内容を習得できます。

* Android Studio で We.Travel サンプルアプリをダウンロードして開きます。
* [!DNL Target] の Mobile Services SDK 設定の確認と更新

## We.Travel アプリのダウンロード

* [sample-app-android-SDKv4-Base-Version.zip](assets/sample-app-android-SDKv4-Base-Version.zip) をダウンロードします。
* zip ファイルを解凍します。
* 既存のプロジェクトとして Android Studio でアプリを開きます（「無効な VCS のルートマッピング」に関するエラーは無視します）。
* エミュレーターでアプリを実行し、アプリがビルドされ、ホーム画面が表示されることを確認します
* アプリを閲覧し、予約プロセスを完了できることを確認します（支払いオプションを選択し、「続行」をクリックして請求画面をスキップします。）

   ![appConfirmation 画面](assets/wetravel_homeScreen.png)![を開きます。](assets/wetravel_confirmationScreen.png)

## [!DNL Target] の Mobile Services SDK 設定の確認と更新

Adobeの Mobile Services SDK は、ドキュメント ](https://experienceleague.adobe.com/docs/mobile-services/android/getting-started-android/requirements.html?lang=en) に従って、We.Travel アプリ [ 内にプレインストールされています。 次に、自分の [!DNL Target] アカウントを指すようにインストールを更新します。

最初に、Mobile Services ユーザーインターフェイスで新しいアプリを作成します。

1. [AdobeMobile Services インターフェイス ](https://mobilemarketing.adobe.com/) にログインします。
1. [!UICONTROL  アプリの管理 ] に移動し、**[!UICONTROL 追加]** をクリックして、このチュートリアルで使用する新しいアプリを追加します（**[!UICONTROL アプリの管理]**/**[!UICONTROL 追加]**）。
1. 非実稼動データを含む Analytics レポートスイートを選択し、アプリに名前を付け、**[!UICONTROL 標準]** タイプを選択して、「**[!UICONTROL 保存]**」をクリックします。
1. アプリを追加したら、次の画面の「[!UICONTROL SDK Target オプション ]」セクションに [!DNL Target] クライアントコードを追加します（[!DNL Target] インターフェイスの **[!UICONTROL 設定]** / **[!UICONTROL 実装]** / **[!UICONTROL 設定を編集]** にあります）。をクリックします )。`at.js`
1. [!UICONTROL  リクエストのタイムアウト ] 設定は、タイムアウトの指示を実行する前に、アプリが [!DNL Target] サーバーからの応答を待機する時間を決定します。 デフォルト設定のままにします。
1. [!UICONTROL  訪問者 ID サービス ] を有効にし、ドロップダウンで [!UICONTROL  組織 ] が選択されていることを確認します。
1. 変更を保存するには、ウィンドウの右上（[!UICONTROL  ユニバーサルリンク ]、[!UICONTROL  アプリリンク ]、または [!UICONTROL  プッシュサービス ] セクションのリンクではなく）にある「**[!UICONTROL 保存]**」をクリックします。
1. ページ下部の「 App SDK のダウンロード数」セクションまでスクロールし、設定ファイルをダウンロードします。

   ![設定ファイルのダウンロード](assets/config_file.jpg)

1. Android Studio のプロジェクトアセットフォルダー（アプリ/src/main/assets）の `ADBMobileConfig.json` ファイルを置き換えます。

1. 次に `ADBMobileConfig.json` ファイルを開き、[!DNL Target] クライアントコードや Analytics の詳細など、予期される変更が含まれていることを確認します。
   ![設定ファイルのダウンロード](assets/client_code.jpg)

設定が表示されない場合は、[!UICONTROL Mobile Services] インターフェイスで右の「**[!UICONTROL 保存]**」ボタンをクリックし、ファイルを正しい場所にコピーしたことを確認します。

おめでとう！ SDK を [!DNL Target] アカウントの詳細で更新しました。 次のレッスンで [!DNL Target] リクエストを追加した後に、設定の追加検証を行います。

**[次へ：「Target リクエストの追加」>](add-requests.md)**
