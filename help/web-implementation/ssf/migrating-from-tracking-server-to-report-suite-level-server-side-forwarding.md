---
title: トラッキングサーバーからレポートスイートレベルのサーバーサイド転送への移行
description: トラッキングサーバーレベルではなくレポートスイートレベルで、Adobe Analytics データのサーバーサイド転送をAudience Managerに対して有効にする方法を説明します。
product: audience manager
feature: Adobe Analytics Integration
topics: null
activity: implement
doc-type: technical video
team: Technical Marketing
kt: 1776
role: Developer, Data Engineer
level: Intermediate
exl-id: 08b81e52-a28a-43e4-a284-df2460a43016
source-git-commit: 4adaade180545bcf5f911b7453a7e9939e2ed178
workflow-type: tm+mt
source-wordcount: '582'
ht-degree: 0%

---

# トラッキングサーバーからレポートスイートレベルのサーバーサイド転送への移行 {#migrating-from-tracking-server-to-report-suite-level-server-side-forwarding}

この記事とビデオでは、[!DNL Analytics] Data のサーバーサイド転送を [!UICONTROL report suite] レベルではなく [!UICONTROL tracking server] レベルでAudience Managerに有効にする方法を説明します。

## 概要 {#introduction}

Adobe Audience ManagerとAdobe Analyticsがある場合は、[!DNL Analytics] データのサーバーサイド転送をAudience Managerに実装できます。 つまり、ページは 2 つのヒット（1 つは [!DNL Analytics] に、1 つはAudience Managerに）を送信する代わりに、[!DNL Analytics] にヒットを送信でき、[!DNL Analytics] はそのデータをAudience Managerに転送します。

既に稼働しており、2017 年 10 月より前に有効または実装している場合、サーバーサイド転送は、[!UICONTROL Tracking Server] に基づいている可能性があります。これは、Adobe カスタマーケアまたはAdobe Consultingによって有効にする必要がありました。 2017 年 10 月から、サーバーサイド転送を自分で設定して、レポートスイートレベル（レポートスイートごとの転送）で実行できるようになりました。 これには大きなメリットがあります。これについては、以下で説明します。

## [!UICONTROL Tracking server] 転送 {#tracking-server-forwarding}

[!UICONTROL tracking server] は、[!DNL Analytics] データを送信する場所であり、イメージリクエストと cookie が書き込まれているドメインでもあります。 DTM または [!DNL Experience Platform Launch]、または [!DNL AppMeasurement.js] ファイルで設定する必要があります。通常は次のように表示され、サイト名またはビジネス名が「mysite」に置き換えられます。

`s.trackingServer = "mysite.sc.omtrdc.net";`

サーバーサイド転送が [!UICONTROL tracking server] レベルで転送するように設定されている場合、この [!UICONTROL tracking server] に送信されているヒットはすべて（Experience Cloud ID サービスも有効になっている場合）、Audience Managerに転送されます。 これは、Adobe カスタマーケアまたはAdobe Consultingで有効にする必要がありました。 また、以下に説明するように、[!UICONTROL report suite] 転送に切り替えた後で無効にすることもできます。

[!DNL tracking server forwarding] が有効になっているかどうかわからない場合は、Adobe カスタマーケアまたはAdobe Consultingにお問い合わせください。お知らせいたします。

## [!UICONTROL Report-suite] レベルのサーバーサイド転送 {#report-suite-level-server-side-forwarding}

[!UICONTROL report suite] 転送から [!UICONTROL tracking server] 転送に移行する最大の利点の 1 つは、「Audience Analytics」を使用できるようになることです。これは、Audience Manager [!UICONTROL segments] をAdobe Analyticsに転送し、詳細なセグメント分析を行う機能です。 この優れた機能は、まだ [!UICONTROL tracking server] 転送中で [!UICONTROL report suite] 転送でない場合はサポートされません。 Audience Analyticsについて詳しくは、[ ドキュメント ](https://experienceleague.adobe.com/docs/analytics/integration/audience-analytics/mc-audiences-aam.html?lang=ja) を参照してください。

>[!VIDEO](https://video.tv.adobe.com/v/23701/?quality=12)

## 重要ヒント {#additional-resources}

上記のビデオで説明しているように、Audience Managerに転送する [!UICONTROL report suites] ールをすべて転送に設定したら、Adobe カスタマーケアまたはAdobe Consultingに連絡して、[!UICONTROL tracking server] 転送を無効にしてもらう必要があります。 [!UICONTROL tracking server] 転送と [!UICONTROL report suite] 転送の両方を持っているとヒットが重複することはないので、これを行うことは緊急ではありません。 ただし、ベストプラクティスとして、[!UICONTROL report suite] 転送のみを行うことをお勧めします。

[!UICONTROL tracking server] 転送をオンのままにすると、転送したくない [!UICONTROL report suites] ーザーからデータが転送されるだけでなく、今後、[!UICONTROL tracking server] 転送がオンになっていることを（および会社の全員が）忘れた後で、特定の [!UICONTROL report suite] ーザーに対してデータが転送されていないと思うかもしれません。 これは、レポートスイートレベルでオンになっていませんが、[!UICONTROL tracking server] ラーが原因でデータがまだ転送されているからです。 その後、転送する理由を把握し、予期していなかったAAM サーバーコールの料金を支払うことで、時間とコストを無駄にします。 したがって、ビジネスニーズに合った転送 [!UICONTROL tracking server] ールをすべて設定したら、すぐに [!UICONTROL report suites] 転送を無効にすることをお勧めします。
