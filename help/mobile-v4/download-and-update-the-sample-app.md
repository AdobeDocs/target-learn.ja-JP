---
title: We.Travel サンプルアプリのダウンロードと更新
description: We.Travel サンプルアプリは、Adobe Mobile Services SDK v4 で事前に実装されています。 更新する必要があるのは、独自のExperience Cloud組織とソリューションアカウントを指すようにするだけです。
role: Developer
level: Intermediate
topic: Mobile, Personalization
feature: Implement Mobile
doc-type: tutorial
kt: 3040
exl-id: 244bcf7a-b59b-4dd1-bd05-0a55ce7a7132
source-git-commit: 342e02562b5296871638c1120114214df6115809
workflow-type: tm+mt
source-wordcount: '479'
ht-degree: 0%

---

# We.Travel サンプルアプリのダウンロードと更新

We.Travel サンプルアプリは、Adobe Mobile Services SDK v4 で事前に実装されています。 更新する必要があるのは、独自のExperience Cloud組織とソリューションアカウントを指すようにするだけです。

## 学習目標

このレッスンを終了すると、次の操作を実行できるようになります。

* Android Studio で We.Travel サンプルアプリをダウンロードして開きます
* [!DNL Target] の Mobile Services SDK設定の確認と更新

## We.Travel アプリのダウンロード

* [sample-app-android-SDKv4-Base-Version.zip](assets/sample-app-android-SDKv4-Base-Version.zip) をダウンロードします
* zip ファイルを解凍します
* Android Studio でアプリを既存のプロジェクトとして開きます（「無効な VCS ルートマッピング」に関するエラーは無視します）。
* エミュレーターでアプリを実行して、アプリがビルドされ、ホーム画面が表示されることを確認します
* アプリを閲覧し、予約プロセスを完了できることを確認します（任意の支払いオプションを選択し、「続行」をクリックして請求画面をスキップします）。

  ![ アプリ確認画面 ](assets/wetravel_homeScreen.png)![ 開く ](assets/wetravel_confirmationScreen.png)

## [!DNL Target] の Mobile Services SDK設定の確認と更新

Adobe Mobile Services SDKは、（ドキュメントに従って [We.Travel アプリ内にプリインストールされてい ](https://experienceleague.adobe.com/docs/mobile-services/android/getting-started-android/requirements.html?lang=ja) す。 次に、独自の [!DNL Target] アカウントを指すようにインストールを更新します。

まず、Mobile Services ユーザーインターフェイスで新しいアプリを作成します。

1. [Adobe Mobile Services インターフェイス ](https://mobilemarketing.adobe.com/) にログインします。
1. [!UICONTROL Manage Apps] に移動し、「**[!UICONTROL Add]**」をクリックして、このチュートリアルで使用する新しいアプリを追加します（**[!UICONTROL Manage Apps]**/**[!UICONTROL Add]**）。
1. 実稼動以外のデータを含む Analytics レポートスイートを選択し、アプリに名前を付けて、**[!UICONTROL Standard]** のタイプを選択し、「**[!UICONTROL Save]**」をクリックします。
1. アプリを追加したら、[!DNL Target] のセクションの次の画面で、[!UICONTROL SDK Target Options] クライアントコードを追加します（クライアントコードは、[!DNL Target] インターフェイスの **[!UICONTROL Setup]** > **[!UICONTROL Implementation]** > **[!UICONTROL Edit Settings]** の「`at.js` をダウンロード」ボタンの横にあります）。
1. [!UICONTROL Request Timeout] 設定は、タイムアウトの命令を実行する前に、アプリが [!DNL Target] サーバーからの応答を待つ時間を決定します。 デフォルト設定のままにします。
1. [!UICONTROL Visitor ID Service] を有効にし、ドロップダウンで [!UICONTROL Organization] が選択されていることを確認します。
1. 変更を保存するには、ウィンドウの右上にある **[!UICONTROL Save]** をクリックします（[!UICONTROL Universal Links]、[!UICONTROL App Links] のオプション、または [!UICONTROL Push Services] のセクションのアイコンではありません）。
1. ページ下部の「アプリSDKのダウンロード」セクションまでスクロールし、設定ファイルをダウンロードします。

   ![ 設定ファイルのダウンロード ](assets/config_file.jpg)

1. Android Studio プロジェクトのアセットフォルダー（app/src/main/assets）の `ADBMobileConfig.json` ファイルを置き換えます。

1. 次に、`ADBMobileConfig.json` ファイルを開き、[!DNL Target] クライアントコードや Analytics の詳細など、期待される変更が含まれていることを確認します。
   ![ 設定ファイルのダウンロード ](assets/client_code.jpg)

設定が表示されない場合は、**[!UICONTROL Save]** インターフェイスの右 [!UICONTROL Mobile Services] ボタンをクリックし、ファイルを正しい場所にコピーしたことを確認します。

おめでとうございます。 SDKと [!DNL Target] アカウントの詳細が更新されました。 次のレッスンでリクエストを追加したら、設定 [!DNL Target] 追加の検証を行います。

**[次へ：「ターゲットリクエストの追加」 >](add-requests.md)**
