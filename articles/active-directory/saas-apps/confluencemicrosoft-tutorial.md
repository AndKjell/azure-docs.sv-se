---
title: 'Självstudier: Azure Active Directory-integration med växer samman SAML SSO av Microsoft | Microsoft Docs'
description: Lär dig hur du konfigurerar enkel inloggning mellan Azure Active Directory och växer samman SAML SSO av Microsoft.
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.reviewer: joflore
ms.assetid: 1ad1cf90-52bc-4b71-ab2b-9a5a1280fb2d
ms.service: active-directory
ms.component: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 04/16/2018
ms.author: jeedes
ms.openlocfilehash: 856809d6eb480d0112eb7ed85c33560950be7d64
ms.sourcegitcommit: 30c7f9994cf6fcdfb580616ea8d6d251364c0cd1
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 08/18/2018
ms.locfileid: "42062080"
---
# <a name="tutorial-azure-active-directory-integration-with-confluence-saml-sso-by-microsoft"></a>Självstudier: Azure Active Directory-integration med växer samman SAML SSO av Microsoft

I den här självstudien får du lära dig hur du integrerar växer samman SAML SSO av Microsoft med Azure Active Directory (AD Azure).

Integrera växer samman SAML SSO av Microsoft med Azure AD ger dig följande fördelar:

- Du kan styra i Azure AD som har åtkomst till växer samman SAML SSO av Microsoft
- Du kan aktivera användarna att automatiskt få loggat in på växer samman SAML SSO av Microsoft (Single Sign-On) med sina Azure AD-konton
- Du kan hantera dina konton på en central plats – Azure portal

Om du vill veta mer om integrering av SaaS-app med Azure AD finns i [vad är programåtkomst och enkel inloggning med Azure Active Directory](../manage-apps/what-is-single-sign-on.md).

## <a name="description"></a>Beskrivning:

Använd ditt Microsoft Azure Active Directory-konto med Atlassians växer samman server för att aktivera enkel inloggning. På så sätt alla organisationens användare kan använda autentiseringsuppgifter för Azure AD logga in i programmet växer samman. Det här plugin-programmet använder SAML 2.0 för federation.

## <a name="prerequisites"></a>Förutsättningar

Om du vill konfigurera Azure AD-integrering med växer samman SAML SSO av Microsoft, behöver du följande objekt:

- En Azure AD-prenumeration
- Antal samverkande serverprogram som installeras på en Windows 64-bitars server (lokalt eller i molnet IaaS-infrastrukturen)
- Antal samverkande server är HTTPS aktiverat
- Observera versionerna som stöds för antal samverkande-pluginprogrammet nämns i under avsnitt.
- Antal samverkande servern kan nås på internet särskilt till Azure AD inloggningssidan för autentisering och ska kunna ta emot token från Azure AD
- Administratörsautentiseringsuppgifter ställs in i växer samman
- WebSudo har inaktiverats i växer samman
- Testa användaren som skapades i serverprogram växer samman

> [!NOTE]
> Om du vill testa stegen i den här självstudien rekommenderar vi inte använder en produktionsmiljö av växer samman. Testa integrationen först i utvecklings- eller mellanlagringsmiljön för programmet och sedan använda produktionsmiljön.

Om du vill testa stegen i den här självstudien bör du följa dessa rekommendationer:

- Använd inte din produktionsmiljö, om det inte behövs.
- Om du inte har en Azure AD-utvärderingsmiljö kan du kan få en månads utvärdering här: [– prova](https://azure.microsoft.com/pricing/free-trial/).

## <a name="supported-versions-of-confluence"></a>Antal samverkande versioner som stöds 

Från och med nu stöds följande versioner av växer samman:

- Antal samverkande: 5.0 till 5.10

## <a name="scenario-description"></a>Scenariobeskrivning
I den här självstudien kan du testa Azure AD enkel inloggning i en testmiljö. Det scenario som beskrivs i den här självstudien består av två viktigaste byggstenarna:

1. Att lägga till antal samverkande SAML SSO av Microsoft från galleriet
1. Konfigurera och testa Azure AD enkel inloggning

## <a name="adding-confluence-saml-sso-by-microsoft-from-the-gallery"></a>Att lägga till antal samverkande SAML SSO av Microsoft från galleriet
För att konfigurera integrering av växer samman SAML SSO av Microsoft i Azure AD, som du behöver lägga till antal samverkande SAML SSO av Microsoft från galleriet i din lista över hanterade SaaS-appar.

**Utför följande steg för att lägga till antal samverkande SAML SSO av Microsoft från galleriet:**

1. I den **[Azure-portalen](https://portal.azure.com)**, klicka på den vänstra navigeringspanelen **Azure Active Directory** ikon. 

    ![Active Directory][1]

1. Gå till **företagsprogram**. Gå till **alla program**.

    ![Program][2]
    
1. Lägg till nytt program, klicka på **nytt program** knappen överst i dialogrutan.

    ![Program][3]

1. I sökrutan skriver **växer samman SAML SSO av Microsoft**.

    ![Skapa en Azure AD-användare för testning](./media/confluencemicrosoft-tutorial/tutorial_confluencemicrosoft_search.png)

1. I resultatpanelen väljer **växer samman SAML SSO av Microsoft**, och klicka sedan på **Lägg till** för att lägga till programmet.

    ![Skapa en Azure AD-användare för testning](./media/confluencemicrosoft-tutorial/tutorial_confluencemicrosoft_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a>Konfigurera och testa Azure AD enkel inloggning
I det här avsnittet ska du konfigurera och testa Azure AD enkel inloggning med växer samman SAML SSO av Microsoft baserat på en testanvändare som kallas ”Britta Simon”.

För enkel inloggning att fungera, behöver Azure AD du veta vad användaren motsvarighet i antal samverkande SAML SSO av Microsoft som är till en användare i Azure AD. Med andra ord måste en länk relationen mellan en Azure AD-användare och relaterade användaren i antal samverkande SAML SSO av Microsoft upprättas.

Om du vill konfigurera och testa Azure AD enkel inloggning med växer samman SAML SSO av Microsoft, måste du utföra följande byggblock:

1. **[Konfigurera Azure AD enkel inloggning](#configuring-azure-ad-single-sign-on)**  – om du vill ge användarna använda den här funktionen.
1. **[Skapa en Azure AD-testanvändare](#creating-an-azure-ad-test-user)**  – om du vill testa Azure AD enkel inloggning med Britta Simon.
1. **[Skapa ett antal samverkande SAML SSO genom Microsoft testanvändare](#creating-a-confluence-saml-sso-by-microsoft-test-user)**  – du har en motsvarighet för Britta Simon i antal samverkande SAML SSO av Microsoft som är länkad till en Azure AD-representation av användaren.
1. **[Tilldela Azure AD-testanvändare](#assigning-the-azure-ad-test-user)**  – om du vill aktivera Britta Simon att använda Azure AD enkel inloggning.
1. **[Testa enkel inloggning](#testing-single-sign-on)**  – om du vill kontrollera om konfigurationen fungerar.

### <a name="configuring-azure-ad-single-sign-on"></a>Konfigurera Azure AD enkel inloggning

I det här avsnittet ska du aktivera Azure AD enkel inloggning i Azure-portalen och konfigurera enkel inloggning i din växer samman SAML SSO med Microsoft-program.

**Utför följande steg för att konfigurera Azure AD enkel inloggning med växer samman SAML SSO av Microsoft:**

1. I Azure-portalen på den **växer samman SAML SSO av Microsoft** program integration-sidan klickar du på **enkel inloggning**.

    ![Konfigurera enkel inloggning][4]

1. På den **enkel inloggning** dialogrutan **läge** som **SAML-baserad inloggning** att aktivera enkel inloggning.

    ![Konfigurera enkel inloggning](./media/confluencemicrosoft-tutorial/tutorial_confluencemicrosoft_samlbase.png)

1. På den **växer samman SAML SSO genom Microsoft-Domain och URL: er** avsnittet, utför följande steg:

    ![Konfigurera enkel inloggning](./media/confluencemicrosoft-tutorial/tutorial_confluencemicrosoft_url.png)

    a. I den **inloggnings-URL** textrutan anger du ett URL med hjälp av följande mönster: `https://<domain:port>/plugins/servlet/saml/auth`

    b. I den **identifierare** textrutan anger du ett URL med hjälp av följande mönster: `https://<domain:port>/`

    c. I den **svars-URL** textrutan anger du ett URL med hjälp av följande mönster: `https://<domain:port>/plugins/servlet/saml/auth`

    > [!NOTE]
    > Dessa värden är inte verkliga. Uppdatera dessa värden med de faktiska identifierare, svars-URL och inloggnings-URL. Porten är valfritt, om det är en namngiven URL. Dessa värden tas emot under konfigurationen av växer samman plugin-programmet, som beskrivs senare i självstudien.

1. På den **SAML-signeringscertifikat** klickar du på kopieringsknappen för att kopiera **Appfederationsmetadata** och klistra in den i anteckningar.
    
    ![Konfigurera enkel inloggning](./media/confluencemicrosoft-tutorial/tutorial_metadataurl.png)
     
1. Klicka på **spara** knappen.

    ![Konfigurera enkel inloggning](./media/confluencemicrosoft-tutorial/tutorial_general_400.png)

1. I ett annat webbläsarfönster logga du in på din instans av växer samman som en administratör.

1. Hovra över kugghjulet och klicka på den **tillägg**.
    
    ![Konfigurera enkel inloggning](./media/confluencemicrosoft-tutorial/addon1.png)

1. Hämta plugin-programmet från [Microsoft Download Center](https://www.microsoft.com/download/details.aspx?id=56503). Överför manuellt från Microsoft med hjälp av plugin-programmet **överföra tillägg** menyn. Täcks av hämtningen av plugin-programmet [Microsofts serviceavtal](https://www.microsoft.com/servicesagreement/). 
    
    ![Konfigurera enkel inloggning](./media/confluencemicrosoft-tutorial/addon12.png)

1. När plugin-programmet har installerats, visas det i **användarinstallerade** tillägg delen av **Hantera tillägg** avsnittet. Klicka på **konfigurera** att konfigurera nya plugin-programmet.
    
    ![Konfigurera enkel inloggning](./media/confluencemicrosoft-tutorial/addon13.png)

1. Utför följande steg på konfigurationssidan:

    ![Konfigurera enkel inloggning](./media/confluencemicrosoft-tutorial/addon52.png)

    > [!TIP]
    > Kontrollera att det finns bara ett certifikat mappas mot appen så att det finns inget fel vid matchning av metadata. Om det finns flera certifikat, hämtar administratören fel vid lösa metadata.

    a. I den **Metadata_url** textrutan klistra in **Appfederationsmetadata** värde som du har kopierat från Azure-portalen och klicka på den **lösa** knappen. Den läser metadata-URL för IDP: N och fylls informationen för fält.

    b. Kopiera den **identifierare, svars-URL och URL: en inloggning** standardvärden och klistra in dem i **identifierare, svars-URL och URL: en inloggning** textrutor i respektive **växer samman SAML SSO genom Microsoft-Domain och URL: er**  avsnittet på Azure-portalen.

    c. I **knappen inloggningsnamn** skriver du namnet på knappen som organisationen vill att användarna kan se på inloggningsskärmen.

    d. I **SAML-ID användarplatser**, väljer du antingen **användar-ID är i NameIdentifier-elementet i instruktionen ämne** eller **användar-ID är i ett attributelement**.  Detta ID måste vara växer samman användar-id. Om det användar-id inte matchas sedan kan inte användare att logga in. 

    > [!Note]
    > Standardplatsen för SAML användar-ID är namnidentifierare. Du kan ändra det till ett attributalternativ för och ange lämpliga attributets namn.
    
    e. Om du väljer **användar-ID är i ett attributelement** alternativet i **attributnamnet** textrutan skriver du namnet på attributet som där användar-Id förväntas. 

    f. Om du använder federerad domän (till exempel AD FS osv) med Azure AD, klicka sedan på den **aktivera identifiering av startsfär** och konfigurera den **domännamn**.
    
    g. I **domännamn** anger du domännamnet här vid inloggningen ADFS-baserad.

    h. Kontrollera **aktivera enkelinloggning ut** om du vill logga ut från Azure AD när en användare loggar från växer samman. 

    i. Klicka på **spara** för att spara inställningarna.

    > [!NOTE]
    > Mer information om installation och felsökning finns [administratörshandboken för MS växer samman SSO Connector](../ms-confluence-jira-plugin-adminguide.md) och det finns också [vanliga frågor och svar](../ms-confluence-jira-plugin-faq.md) på förhand

### <a name="creating-an-azure-ad-test-user"></a>Skapa en Azure AD-användare för testning
Målet med det här avsnittet är att skapa en testanvändare i Azure-portalen kallas Britta Simon.

![Skapa en Azure AD-användare][100]

**Utför följande steg för att skapa en testanvändare i Azure AD:**

1. I den **Azure-portalen**, i det vänstra navigeringsfönstret klickar du på **Azure Active Directory** ikon.

    ![Skapa en Azure AD-användare för testning](./media/confluencemicrosoft-tutorial/create_aaduser_01.png) 

1. Om du vill visa en lista över användare, gå till **användare och grupper** och klicka på **alla användare**.
    
    ![Skapa en Azure AD-användare för testning](./media/confluencemicrosoft-tutorial/create_aaduser_02.png) 

1. Öppna den **användaren** dialogrutan klickar du på **Lägg till** överst i dialogrutan.
 
    ![Skapa en Azure AD-användare för testning](./media/confluencemicrosoft-tutorial/create_aaduser_03.png) 

1. På den **användaren** dialogrutan utför följande steg:
 
    ![Skapa en Azure AD-användare för testning](./media/confluencemicrosoft-tutorial/create_aaduser_04.png) 

    a. I den **namn** textrutan typ **BrittaSimon**.

    b. I den **användarnamn** textrutan skriver den **e-postadress** av BrittaSimon.

    c. Välj **visa lösenord** och anteckna värdet för den **lösenord**.

    d. Klicka på **Skapa**.
 
### <a name="creating-a-confluence-saml-sso-by-microsoft-test-user"></a>Skapa ett antal samverkande SAML SSO genom Microsoft testanvändare

Om du vill aktivera Azure AD-användare att logga in på växer samman den lokala servern, måste de etableras i antal samverkande SAML SSO av Microsoft. Etablering är en manuell aktivitet för antal samverkande SAML SSO av Microsoft.

**Utför följande steg för att etablera ett användarkonto:**

1. Logga in till växer samman den lokala servern som administratör.

1. Hovra över kugghjulet och klicka på den **Användarhantering**.

    ![Lägg till medarbetare](./media/confluencemicrosoft-tutorial/user1.png) 

1. Under avsnittet för användare, klickar du på **lägga till användare** fliken. På den **lägga till en användare** dialogrutan utför följande steg:

    ![Lägg till medarbetare](./media/confluencemicrosoft-tutorial/user2.png) 

    a. I den **användarnamn** textrutan Skriv e-postadress för användaren som Britta Simon.

    b. I den **fullständigt namn** textrutan skriver du det fullständiga namnet på användaren som Britta Simon.

    c. I den **e-post** textrutan typ e-postadressen för användaren som Brittasimon@contoso.com.

    d. I den **lösenord** textrutan skriver du lösenordet för Britta Simon.

    e. Klicka på **Bekräfta lösenord** ange lösenordet igen.
    
    f. Klicka på **Lägg till** knappen.

### <a name="assigning-the-azure-ad-test-user"></a>Tilldela Azure AD-testanvändare

I det här avsnittet ska aktivera du Britta Simon att använda Azure enkel inloggning genom att bevilja åtkomst till växer samman SAML SSO av Microsoft.

![Tilldela användare][200] 

**Om du vill tilldela Britta Simon växer samman SAML SSO av Microsoft, utför du följande steg:**

1. Öppna vyn program i Azure-portalen och gå till vyn directory och gå till **företagsprogram** klickar **alla program**.

    ![Tilldela användare][201] 

1. I listan med program väljer **växer samman SAML SSO av Microsoft**.

    ![Konfigurera enkel inloggning](./media/confluencemicrosoft-tutorial/tutorial_confluencemicrosoft_app.png) 

1. I menyn till vänster, klickar du på **användare och grupper**.

    ![Tilldela användare][202] 

1. Klicka på **Lägg till** knappen. Välj sedan **användare och grupper** på **Lägg till tilldelning** dialogrutan.

    ![Tilldela användare][203]

1. På **användare och grupper** dialogrutan **Britta Simon** på listan användare.

1. Klicka på **Välj** knappen **användare och grupper** dialogrutan.

1. Klicka på **tilldela** knappen **Lägg till tilldelning** dialogrutan.
    
### <a name="testing-single-sign-on"></a>Testa enkel inloggning

I det här avsnittet ska testa du Azure AD enkel inloggning för konfigurationen med hjälp av åtkomstpanelen.

När du klickar på växer samman SAML SSO genom Microsoft-panelen i åtkomstpanelen du bör få automatiskt loggat in på ditt växer samman SAML SSO av Microsoft-program.
Läs mer om åtkomstpanelen [introduktion till åtkomstpanelen](../user-help/active-directory-saas-access-panel-introduction.md).

## <a name="additional-resources"></a>Ytterligare resurser

* [Lista över guider om hur du integrerar SaaS-appar med Azure Active Directory](tutorial-list.md)
* [Vad är programåtkomst och enkel inloggning med Azure Active Directory?](../manage-apps/what-is-single-sign-on.md)



<!--Image references-->

[1]: ./media/confluencemicrosoft-tutorial/tutorial_general_01.png
[2]: ./media/confluencemicrosoft-tutorial/tutorial_general_02.png
[3]: ./media/confluencemicrosoft-tutorial/tutorial_general_03.png
[4]: ./media/confluencemicrosoft-tutorial/tutorial_general_04.png

[100]: ./media/confluencemicrosoft-tutorial/tutorial_general_100.png

[200]: ./media/confluencemicrosoft-tutorial/tutorial_general_200.png
[201]: ./media/confluencemicrosoft-tutorial/tutorial_general_201.png
[202]: ./media/confluencemicrosoft-tutorial/tutorial_general_202.png
[203]: ./media/confluencemicrosoft-tutorial/tutorial_general_203.png
