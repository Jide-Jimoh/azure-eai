Overview
========

In this document we will be demonstrating how to register your application in
Azure AD and how to configure another Azure AD application as client application
so that your frontend application was authenticated before it can access to your
Azure AD protected backend API.

Steps to accomplish this are divided into several steps

-   Register Backend API in Azure AD and configure Azure AD authentication of
    Backend API

-   Register Frontend App in Azure AD and grant Backend access to this
    application

-   Write custom code to verify result

![](media/c0fe55e51b01d0cdc1afa4126ec40869.png)

This document will not include instruction to deploy sample Backend API to Azure
API App, we assume you have done this.

Prerequisites
=============

-   Azure Subscription

-   Deployed Backend API to Azure as a API App or Web App

# Create a Backend API App

-   For simplicity sake, we’ve prepare a backend [REST
    API](../source/EAIBackendAPI). It only echoes back fixed string in this lab,
    you can modify it to fit your requirement.

-   Open the solution file with Visual Studio and deploy to you Azure
    Subscription

-   Make sure you see below results.

![](media/e86d951fba3e07b81576ffbf59970cd9.png)

# Configure Azure AD authentication for your Backend API


In this section we will be configuring our API App to use Azure AD as its
authentication identity provider. So that access to this application requires
Azure AD authentication.

-   Go back to Azure Portal, open the Backend API’s API Service essential blade.
    Go to Authentication, then Enable Authentication and click Azure Active
    Directory

![](media/1a603dee9e844c616a0df1b01c192bb5.png)

-   Switch to “Express” mode and create a new application

![](media/339cdff7b09d6bbbaf62743fbda7df69.png)

-   Click OK to go back to previous blade, then switch Action to “Log in with
    Azure Active Directory” then click Save to save changes

![](media/21cdaf1732efffb7596c9b37f163f0ef.png)

-   At this point you have successfully configured Azure AD authentication for
    your backend application. To verify this, open your browser and navigate to
    your backend API, you will be redirected to default AD login page.

![](media/0059805a4cdee0baae11f68a3cfc274c.png)

-   Go back to Azure Portal, Open Azure AD management blade

![](media/e487d4531301b398dca38eb93aa46aec.png)

-   Go to Application Registration, search for the application we created above

![](media/7be07c41e8fb5923a7165a2031b3201d.png)

-   Open up application essential blade, properties, and note down Application
    ID. We will need this value so that our frontend application knows to which
    Azure AD application it need to request access.

![](media/227c26d0fcf53d5f21aa4211822e97bf.png)

# Register Frontend application

Now we are good to register an Azure AD application for our frontend
application. You don’t have to have an actual application to register one, this
step is to configure required access permission for future uses.

-   Go to Azure AD management console

-   Create a new Application registration

    -   Name is a unique friendly name

    -   Application Type should be Web app/API

    -   Sign-on URL need to be a valid URL but not necessary an actual URL.

![](media/95d0f99e221b69ba648c57a39ab62616.png)

-   Now we need to grant backend API access to our frontend application. Go to
    Settings, Required Permissions.

![](media/76c7d40889d24320835522d6c77eb2f2.png)

-   Choose the backend API application we created above

![](media/501f148a40fc2e7fa8ea449ddcd1b915.png)

-   Make sure you check required permission

![](media/eec4b40620b0556b8ce08a7616abd734.png)

-   Grant permission to our applications

Details of Grant permission :
<https://docs.microsoft.com/en-us/azure/active-directory/develop/active-directory-integrating-applications>

![](media/49674849b53b5e12ebe5251c49bece52.png)

-   Create a Key for Frontend App, this key is referred as Client Secret in your
    frontend application. Note that this key will only be shown once after you
    click “Save” so be sure to note it down.

![](media/193e179d4c3cc9fd6b13396e020323b7.png)

-   Note down Key and Application Id, this will be refered as Client ID in your
    frontend application

![](media/77ec6565f00e5e37f0d0e753a80e08f3.png)

Verify Azure AD Authentication with Frontend App

-   I have a frontend application ready [here](../source/EAIFrontEndApp)

-   Open Program.cs and modify placeholder to corresponding values

    -   Authority is the authority URL that will authenticate users, it is in
        the form of
        [https://loing.microsoftonline.com/\<Tenant](https://loing.microsoftonline.com/%3cTenant)
        ID\>

>   A tenant ID is a GUID that represents your Azure AD tenant, it can be found
>   in Azure AD management console -\> Properties -\> Directory ID

![](media/c275bcaaee45684370c3633f20bfa4ab.png)

-   FRONT_APP_ID is the application Id we copied from Frontend application
    registration step

-   FRONT_APP_KEY is the client secret we generated in Frontend application
    registration step

-   BACKEND_APP_ID is the application Id we copied from Backend application
    registration step.

![](media/e4b7b9f24ae4d1813ba99dbff90e988f.png)

-   Go to Main(), modify Url to your API URL

![](media/5e53da9b8b7411a6b41f4d8fe83b68e1.png)

-   Hit F5 to run.
