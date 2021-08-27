---
title: We.Travelサンプルアプリのダウンロードと更新
description: 'We.Travelサンプルアプリは、Mobile Services SDK v4Adobeで事前に実装されています。 独自のExperience Cloud組織とソリューションアカウントを指すように更新するだけです。   '
role: Developer
level: Intermediate
topic: Mobile, Personalization
feature: Implement Mobile
doc-type: tutorial
kt: 3040
thumbnail: null
exl-id: 244bcf7a-b59b-4dd1-bd05-0a55ce7a7132
source-git-commit: ee58c7c85708722cf040cd9b039a2855dd390a16
workflow-type: tm+mt
source-wordcount: '520'
ht-degree: 0%

---

# We.Travelサンプルアプリのダウンロードと更新

We.Travelサンプルアプリは、Mobile Services SDK v4Adobeで事前に実装されています。 必要な操作は、独自のExperience Cloud組織とソリューションアカウントを指すように、更新するだけです。

## 学習内容

このレッスンを最後まで学習すると、次の内容を習得できます。

* Android StudioでWe.Travelサンプルアプリをダウンロードして開きます。
* [!DNL Target]のMobile Services SDK設定の確認と更新

## We.Travelアプリのダウンロード

* [sample-app-android-SDKv4-Base-Version.zip](assets/sample-app-android-SDKv4-Base-Version.zip)をダウンロードします。
* zipファイルを解凍します。
* Android Studioで既存のプロジェクトとしてアプリを開きます（「無効なVCSのルートマッピング」に関するエラーは無視します）。
* エミュレーターでアプリを実行し、アプリがビルドされ、ホーム画面が表示されることを確認します
* アプリを閲覧し、予約プロセスを完了できることを確認します（支払いオプションを選択し、「続行」をクリックして請求画面をスキップします）。

   ![appConfirmation画面](assets/wetravel_homeScreen.png)![を開きます。](assets/wetravel_confirmationScreen.png)

## [!DNL Target]のMobile Services SDK設定の確認と更新

AdobeMobile Services SDKは、ドキュメント](https://experienceleague.adobe.com/docs/mobile-services/android/getting-started-android/requirements.html?lang=en)に従って、We.Travelアプリ[内にプレインストールされています。 次に、自分の[!DNL Target]アカウントを指すようにインストールを更新します。

最初に、Mobile Servicesユーザーインターフェイスで新しいアプリを作成します。

1. [AdobeMobile Servicesインターフェイス](https://mobilemarketing.adobe.com/)にログインします。
1. [!UICONTROL アプリの管理]に移動し、**[!UICONTROL 追加]**&#x200B;をクリックして、このチュートリアルで使用する新しいアプリを追加します（**[!UICONTROL アプリの管理]**/****&#x200B;の追加）。
1. 実稼動以外のデータを含むAnalyticsレポートスイートを選択し、アプリに名前を付け、「**[!UICONTROL 標準]**」タイプを選択して、「**[!UICONTROL 保存]**」をクリックします。
1. アプリを追加したら、次の画面の「[!UICONTROL SDK Targetオプション]」セクションに[!DNL Target]クライアントコードを追加します（[!DNL Target]インターフェイスの&#x200B;**[!UICONTROL 設定]** / **[!UICONTROL 実装]** / **[!UICONTROL 設定を編集]**、をクリックします）。`at.js`
1. [!UICONTROL リクエストのタイムアウト]設定は、タイムアウトの指示を実行する前に、アプリが[!DNL Target]サーバーからの応答を待つ時間を決定します。 デフォルト設定のままにします。
1. [!UICONTROL 訪問者IDサービス]を有効にし、ドロップダウンで[!UICONTROL 組織]が選択されていることを確認します。
1. ウィンドウの右上にある「**[!UICONTROL 保存]**」（「[!UICONTROL ユニバーサルリンク]」、「[!UICONTROL アプリリンク]」、「[!UICONTROL プッシュサービス]」セクションのリンクではなく）をクリックして、変更を保存します。
1. ページ下部の「 App SDKのダウンロード数」セクションまでスクロールし、設定ファイルをダウンロードします。

   ![設定ファイルのダウンロード](assets/config_file.jpg)

1. Android Studioプロジェクトアセットフォルダー（アプリ/src/main/assets）の`ADBMobileConfig.json`ファイルを置き換えます。

1. 次に、`ADBMobileConfig.json`ファイルを開き、[!DNL Target]クライアントコードやAnalyticsの詳細など、予期される変更が含まれていることを確認します。
   ![設定ファイルのダウンロード](assets/client_code.jpg)

設定が表示されない場合は、[!UICONTROL Mobile Services]インターフェイスで右の「**[!UICONTROL 保存]**」ボタンをクリックし、ファイルを正しい場所にコピーしたことを確認します。

おめでとう！ SDKを[!DNL Target]アカウントの詳細で更新しました。 次のレッスンで[!DNL Target]リクエストを追加した後に、設定の追加検証をおこないます。

**[次へ：「Targetリクエストの追加」>](add-requests.md)**
