<properties
    pageTitle="Automatische Anmeldung mit Azure Active Directory for Windows Domain-Joined | Microsoft Azure"
    description="IT-Administratoren können ihre Domäne Windows-Geräte automatisch und im Hintergrund mit Azure Active Directory (Azure AD) registrieren."
    services="active-directory"
    documentationCenter=""
    authors="markusvi"
    manager="femila"
    editor=""/>

<tags
    ms.service="active-directory"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="09/30/2016"
    ms.author="markvi"/>

# <a name="automatic-device-registration-with-azure-active-directory-for-windows-domain-joined-devices"></a>Automatische Anmeldung mit Active Directory für Windows Azure Domäne

Als IT-Administrator können Sie automatisch und im Hintergrund Ihre Geräte Domäne Windows Azure Active Directory (Azure AD) registrieren. Dies kann nützlich sein, wenn Sie konfiguriert Gerät bedingte Richtlinien, Office365 oder lokal durch AD FS verwaltet. Sie können Szenarien Registrierung Gerät erfahren [Azure Active Directory Gerät Registrierung-Übersicht](active-directory-conditional-access-device-registration-overview.md)lesen.

>AZURE. Hinweis für aktuelle Informationen zum automatische Registrierung Siehe [Automatische Registrierung des Windows-Domäne einrichten mit Azure Active Directory verbunden](active-directory-conditional-access-automatic-device-registration-setup.md).

Automatische Registrierung des Geräts in Azure Active Directory steht für Windows 7 und Windows 8.1, die Active Directory-Domäne hinzugefügt. Sind in der Regel Unternehmen für Information Worker bereitgestellten Computer gehört.

Zunächst registrieren Ihrer Domäne angeschlossen Geräte von Windows Azure AD, führen Sie die folgenden erforderlichen Komponenten. Abschluss die erforderlichen Komponenten für Ihre Windows Geräte Domäne konfigurieren Sie automatische Registrierung.

## <a name="prerequisites-for-automatic-device-registration-of-domain-joined-windows-devices-with-azure-active-directory"></a>Erforderliche Komponenten für die automatische Registrierung von Domänennamen verbunden Geräte Windows Azure Active Directory

<a name="deploy-ad-fs-and-connect-to-azure-active-directory-using-azure-active-directory-connect"></a>Bereitstellen von AD FS und Azure Active Directory mithilfe von Active Directory Azure Connect an
----------------------------------------------------------------------------------------------
1. Verwenden Sie Active Directory-Verbunddienste (AD FS) mit Windows Server 2012 R2 bereitstellen und eine Föderation Beziehung Azure Active Directory Azure Active Directory herstellen.
2. Konfigurieren Sie eine zusätzliche Azure Active Directory relying Party vertrauen Anspruchsregel.
3. Die AD FS-Verwaltungskonsole öffnen und navigieren zu **AD FS**>**Vertrauensstellungen > verlassen vertrauen**. **Anspruchsregeln bearbeiten** auswählen und auf das Microsoft Office 365 Identitätsplattform relying Party Vertrauensobjekt klicken
4. Wählen Sie auf der Registerkarte **Ausgabe transformieren Regeln** **Regel hinzufügen**.
5. Wählen Sie aus **Anspruch Regel** Vorlage Dropdownfeld **Ansprüche mithilfe einer benutzerdefinierten Regel senden** . Wählen Sie **Weiter**.
6. *Authentifizierungsregel Methode Anspruch* geben die **Name der Anspruchsregel:** Textfeld.
7. Geben Sie die folgende Regel Antrag in die **Anspruchsregel:** Textfeld:

        c:[Type == "http://schemas.microsoft.com/claims/authnmethodsreferences"]
        => issue(claim = c);

8. Klicken Sie auf **OK** , zweimal, um das Dialogfeld abzuschließen.

<a name="configure-an-additional-azure-active-directory-relying-party-trust-authentication-class-reference"></a>Konfigurieren Sie zusätzliche Azure Active Directory verlassen Partei vertrauen Referenz für die Authentifizierung
-----------------------------------------------------------------------------------------------------
Öffnen Sie auf Ihrem Verbundserver ein Windows PowerShell-Befehl und geben:


  `Set-AdfsRelyingPartyTrust -TargetName <RPObjectName> -AllowedAuthenticationClassReferences wiaormultiauthn`

Wo <RPObjectName> ist der relying Party Objektname für Ihre Azure Active Directory relying Party Vertrauensobjekt. Dieses Objekt ist i. d. r. Microsoft Office 365 Identitätsplattform benannt.

<a name="ad-fs-global-authentication-policy"></a>AD FS globale Authentifizierungsrichtlinie
-----------------------------------------------------------------------------
Konfigurieren Sie den AD FS globale primäre Authentifizierungsrichtlinie damit integrierte Windows-Authentifizierung für das Intranet kann (Dies ist die Standardeinstellung).


<a name="internet-explorer-configuration"></a>Internet Explorer-Konfiguration
------------------------------------------------------------------------------
Konfigurieren der folgenden in Internet Explorer auf Windows-Geräten für die lokale Intranetzone:

- Keine Aufforderung zur Clientzertifikatsauswahl, wenn nur ein Zertifikat vorhanden ist: **Aktivieren**
- Skripting zulassen: **Aktivieren**
- Automatisches Anmelden nur in der Intranetzone: **aktiviert**

Dies sind die Standardeinstellungen für Internet Explorer lokale Intranetzone. Anzeigen oder verwalten diese Einstellung in Internet Explorer **Internetoptionen**Navigation > **Security** > Lokales Intranet > Stufe. Sie können auch diese Active Directory-Gruppenrichtlinien konfigurieren.

<a name="network-connectivity"></a>Netzwerkkonnektivität
-------------------------------------------------------------
Domäne Windows Geräte Verbindung zu AD FS eine Active Directory-Domänencontroller automatisch mit Azure registrieren müssen. Das bedeutet normalerweise, dass der Computer mit dem Unternehmensnetzwerk verbunden werden muss. Dazu gehören eine drahtgebundene Verbindung, WLAN, DirectAccess oder VPN.

## <a name="configure-azure-active-directory-device-registration-discovery"></a>Azure Active Directory Geräteregistrierung Discovery konfigurieren
Windows 7 und Windows 8.1 Geräte entdecken Registrierungsserver Gerät durch Kombinieren der Benutzerkontoname mit einem bekannten Geräteregistrierung Servernamen. Sie müssen DNS CNAME-Eintrag erstellen, der auf den A-Datensatz zugeordnet Ihre Azure Active Directory Gerät Registrierung verweist. CNAME-Eintrag verwenden die bekannte Präfix **Enterpriseregistration** gefolgt von Benutzerkonten in Ihrer Organisation verwendete Benutzerprinzipalnamen-Suffix. Verwendet Ihre Organisation mehrere Benutzerprinzipalnamen-Suffixe müssen mehrere CNAME-Einträge in DNS erstellt.

Beispielsweise verwenden zwei Benutzerprinzipalnamen-Suffixe in Ihrer Organisation mit dem Namen @contoso.com und @region.contoso.com, erstellen Sie DNS-Einträge.

| Eintrag                                     | Typ  | Adresse                            |
|-------------------------------------------|-------|------------------------------------|
| enterpriseregistration.contoso.com        | CNAME | enterpriseregistration.Windows.NET |
| enterpriseregistration.Region.contoso.com | CNAME | enterpriseregistration.Windows.NET |

##<a name="configure-automatic-device-registration-for-windows-7-and-windows-81-domain-joined-devices"></a>Konfigurieren Sie automatische Gerät Registrierung für Windows 7 und Windows 8.1 Domäne beitreten Geräte

Konfigurieren Sie automatische Registrierung des Geräts für die Windows 7 und Windows 8.1 Domäne beitreten-Geräte über die folgenden Links. Achten Sie darauf, dass Sie die oben aufgeführten Komponenten abgeschlossen haben, bevor Sie fortfahren.

* [Konfigurieren Sie automatische Gerät Registrierung für Windows 8.1 Domäne beitreten Geräte](active-directory-conditional-access-automatic-device-registration-windows-8-1.md)

* [Konfigurieren Sie automatische Gerät Registrierung für Windows 7 Domäne beitreten Geräte](active-directory-conditional-access-automatic-device-registration-windows7.md)

* [Automatische Anmeldung mit Azure Active Directory for Windows 10 Domain-Joined](active-directory-azureadjoin-devices-group-policy.md)

<a name="additional-notes"></a>Zusätzliche Hinweise
--------------------------------------------------------------------

Geräteregistrierung mit Azure bietet die breiteste Palette an Funktionen. Azure AD Gerät Registrierung Anmeldung Mobilgeräte (BYOD) und Unternehmen, Domäne Geräte. Die Geräte können mit beiden gehosteten Dienste wie Office365 und Services verwalteten lokalen mit AD FS verwendet werden.

Unternehmen mit mobilen und traditionellen oder, Office365, Azure AD verwenden oder anderen Microsoft-Diensten sollten Geräte in Azure AD mit Azure AD Gerät Registrierungsdienst registriert. Wenn Ihr Unternehmen mobile Geräte nicht verwenden und keine Microsoft-Dienste wie Office365, Azure AD oder Windows Intune stattdessen Hosts nur lokale Programme und die Möglichkeit, Geräte in Active Directory registrieren mit AD FS.

Erfahren Sie mehr zum Bereitstellen von geräteregistrierung mit AD FS [hier](https://technet.microsoft.com/library/dn486831.aspx).

## <a name="additional-topics"></a>Weitere Themen

- [Azure Active Directory Geräteregistrierung-Übersicht](active-directory-conditional-access-device-registration-overview.md)
- [Konfigurieren Sie automatische Registrierung für Windows 7-Domäne beitreten-Geräte](active-directory-conditional-access-automatic-device-registration-windows7.md)
- [Konfigurieren Sie automatische Registrierung für Windows 8.1 Domäne beitreten Geräte](active-directory-conditional-access-automatic-device-registration-windows-8-1.md)
- [Automatische Anmeldung mit Azure Active Directory für Windows 10 Domäne](active-directory-azureadjoin-devices-group-policy.md)
