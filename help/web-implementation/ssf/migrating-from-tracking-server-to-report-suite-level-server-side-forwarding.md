---
title: トラッキングサーバーからレポートスイートレベルのサーバーサイド転送への移行
description: Adobe Analytics データのAudience Managerへのサーバーサイド転送を、トラッキングサーバーレベルではなくレポートスイートレベルで有効にする方法を説明します。
product: audience manager
feature: Adobe Analytics Integration
topics: null
activity: implement
doc-type: technical video
team: Technical Marketing
kt: 1776
role: Developer
level: Intermediate
exl-id: 08b81e52-a28a-43e4-a284-df2460a43016
TQID: https://experienceleague.adobe.com/-fWEu9LWHY-PtIZ-7Phf-ZOHPCD-A67mwb9i3kA7nec
product_v2: id: df80eeb1-8d72-467e-b0df-9d51c7d3a0a1
feature_v2: id: a8b0238e-1d43-4679-a3b4-5ba1bad83baa
role_v2: id: ff6a42d2-313e-452e-93a6-792e4fad9ff8
level_v2: id: b5a62a22-46f7-4f0d-b151-3fc640bef588
topic_v2: id: b5ce8718-c3af-4fdb-a1a9-fca32f83a87cid: c2be0313-b3ae-45e0-b454-d20bf54b23f2
source-git-commit: 3152e8fc51e0e06c90c17dce0aa203a27995e88d
workflow-type: tm+mt
source-wordcount: 608
ht-degree: 1%

---

# トラッキングサーバーからレポートスイートレベルのサーバーサイド転送への移行 {#migrating-from-tracking-server-to-report-suite-level-server-side-forwarding}

この記事とビデオでは、[!DNL Analytics] データのAudience Managerへのサーバーサイド転送を[!UICONTROL tracking server] レベルではなく[!UICONTROL report suite] レベルで有効にする方法について説明します。

## 概要 {#introduction}

Adobe Audience ManagerとAdobe Analyticsがある場合は、[!DNL Analytics] データをAudience Managerにサーバーサイドで転送できます。 つまり、2つのヒットを送信する代わりに（1つは[!DNL Analytics]に、1つはAudience Managerに）、[!DNL Analytics]にヒットを送信し、[!DNL Analytics]はそのデータをAudience Managerに転送します。

この機能を既に使用しており、2017年10月より前に有効/実装していた場合、サーバーサイド転送は[!UICONTROL Tracking Server]に基づいている可能性があります。Adobe カスタマーケアまたはAdobe Consultingで有効にする必要がありました。 2017年10月より、サーバーサイド転送を自分で設定し、レポートスイートレベル（レポートスイートごとに転送）で実行できるようになりました。 これには大きな利点があります。次にその点を説明します。

## [!UICONTROL Tracking server]転送 {#tracking-server-forwarding}

[!UICONTROL tracking server]は、[!DNL Analytics] データを送信する場所であり、画像リクエストとCookieが書き込まれるドメインでもあります。 DTMまたは[!DNL Experience Platform Launch]、または[!DNL AppMeasurement.js] ファイルで設定する必要があり、通常は次のようになります。サイト名またはビジネス名が「mysite」に置き換わります。

`s.trackingServer = "mysite.sc.omtrdc.net";`

サーバーサイド転送が[!UICONTROL tracking server] レベルで転送するように設定されている場合、この[!UICONTROL tracking server]に送信されているすべてのヒット（Experience Cloud ID サービスも有効になっている場合）は、Audience Managerに転送されます。 これは、AdobeのカスタマーケアまたはAdobe Consultingによって有効にする必要がありました。 また、後述のとおり、[!UICONTROL report suite]転送に切り替えた後に無効にできるユーザーもあります。

[!DNL tracking server forwarding]が有効になっているかどうかわからない場合は、Adobe カスタマーケアまたはAdobe Consultingにお問い合わせください。

## [!UICONTROL Report-suite] レベルのサーバーサイド転送 {#report-suite-level-server-side-forwarding}

[!UICONTROL tracking server]転送から[!UICONTROL report suite]転送に移行する最大のメリットの1つは、「Audience Analytics」を使用できるようになったことです。これは、Audience Manager [!UICONTROL segments]をAdobe Analyticsに転送して詳細なセグメント分析を行うことができます。 [!UICONTROL report suite]転送ではなく[!UICONTROL tracking server]転送中の場合、この素晴らしい機能はサポートされていません。 Audience Analyticsについて詳しくは、[ ドキュメント ](https://experienceleague.adobe.com/docs/analytics/integration/audience-analytics/mc-audiences-aam.html?lang=ja)を参照してください。

>[!VIDEO](https://video.tv.adobe.com/v/23701/?quality=12)

## 重要なヒント {#additional-resources}

上記のビデオに記載されているように、[!UICONTROL report suites]のすべてがAudience Managerに転送するように設定されたら、Adobe カスタマーケアまたはAdobe Consultingに連絡して、[!UICONTROL tracking server]転送を無効にする必要があります。 [!UICONTROL tracking server]転送と[!UICONTROL report suite]転送の両方を行っても重複したヒットが発生しないため、これを行うのは緊急事態ではありません。 ただし、[!UICONTROL report suite]転送のみを有効にすることをお勧めします。

[!UICONTROL tracking server]転送を終了すると、データを[!UICONTROL report suites]から転送する必要がなくなるだけでなく、将来的には、[!UICONTROL tracking server]転送が有効であることをユーザー（および社内の全員）が忘れてしまった後、特定の[!UICONTROL report suite]に対してデータが転送されていないと思う可能性があります。 これは、レポートスイートレベルではオンになっていませんが、[!UICONTROL tracking server]が原因で、データがまだ転送されているためです。 その後、時間と費用を無駄にして、なぜそれが転送されているのかを理解し、予期していなかったAAM server呼び出しにも支払います。 したがって、ビジネスニーズに合わせて意味のある転送を行うように、すべての[!UICONTROL report suites]が設定されたら、すぐに[!UICONTROL tracking server]転送を無効にすることをお勧めします。
