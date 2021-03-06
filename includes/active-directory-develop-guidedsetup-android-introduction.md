
# <a name="call-the-microsoft-graph-api-from-an-android-app"></a>Aufrufen der Microsoft Graph-API aus einer Android-App

Dieser Leitfaden veranschaulicht, wie eine native Android-Anwendung ein Zugriffstoken abrufen und die Microsoft Graph-API und andere APIs aufrufen kann, die Zugriffstoken vom Azure Active Directory v2-Endpunkt anfordern.

Am Ende dieses Leitfadens kann Ihre Anwendung Anmeldungen von persönlichen Konten (z.B. outlook.com, live.com u.a.) sowie von Geschäfts,- Schul- oder Unikonten von Unternehmen oder Organisationen akzeptieren, die Azure Active Directory nutzen. Die Anwendung ruft dann eine API auf, die durch den Azure Active Directory v2-Endpunkt geschützt wird.  

## <a name="how-this-sample-works"></a>Funktionsweise dieses Beispiels
![Funktionsweise dieses Beispiels](media/active-directory-develop-guidedsetup-android-intro/android-intro.png)

Die in diesem Leitfaden erstellte Beispielanwendung basiert auf einem Szenario, bei dem mit einer Android-Anwendung eine Web-API abgefragt wird, die Token vom Azure Active Directory v2-Endpunkt akzeptiert (in diesem Fall Microsoft Graph-API). In diesem Szenario fügt Ihre Anwendung HTTP-Anforderungen das abgerufene Token über den Autorisierungsheader hinzu. Tokenabruf und -erneuerung werden für Sie von der Microsoft-Authentifizierungsbibliothek (Microsoft Authentication Library, MSAL) gehandhabt.

## <a name="prerequisites"></a>Voraussetzungen
* Dieses Setup mit Anleitung konzentriert sich auf Android Studio. Andere Android-Anwendungsentwicklungsumgebungen sind jedoch ebenfalls akzeptabel. 
* Android SDK 21 oder höher ist erforderlich (SDK 25 empfohlen).
* Google Chrome oder ein Webbrowser mit benutzerdefinierten Registerkarten ist für diese Version der MSAL für Android erforderlich.

> [!NOTE]
> Google Chrome ist nicht im Visual Studio-Emulator für Android enthalten. Wir empfehlen Ihnen, diesen Code in einem Emulator mit API 25 oder einem Image mit API 21 oder höher zu testen, in dem Google Chrome installiert ist.

## <a name="handling-token-acquisition-for-accessing-protected-web-apis"></a>Verarbeiten des Beziehens von Token für den Zugriff auf geschützte Web-APIs

Nach der Authentifizierung des Benutzers empfängt die Beispielanwendung ein Zugriffstoken, mit dem eine durch Azure Active Directory v2 geschützte Microsoft Graph-API oder Web-API abgefragt werden kann.

APIs wie Microsoft Graph benötigen ein Zugriffstoken, um den Zugriff auf bestimmte Ressourcen zu ermöglichen. Ein Zugriffstoken ist beispielsweise erforderlich, um das Profil eines Benutzers zu lesen, auf den Kalender eines Benutzers zuzugreifen oder eine E-Mail zu senden. Ihre Anwendung kann unter Verwendung von MSAL ein Zugriffstoken anfordern, um auf diese Ressourcen durch Angeben von API-Bereichen zuzugreifen. Dieses Zugriffstoken wird dann dem HTTP-Autorisierungsheader für jeden Aufruf hinzugefügt, der für die geschützte Ressource erfolgt. 

MSAL nimmt Ihrer Anwendung die Verwaltung der Zwischenspeicherung und Aktualisierung von Zugriffstoken ab.

## <a name="libraries"></a>Bibliotheken

In dieser Anleitung werden die folgenden Bibliotheken verwendet:

|Bibliothek|BESCHREIBUNG|
|---|---|
|[com.microsoft.identity.client](http://javadoc.io/doc/com.microsoft.identity.client/msal)|Microsoft Authentication Library (MSAL)|
