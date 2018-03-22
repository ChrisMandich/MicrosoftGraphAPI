# MicrosoftGraphAPI
Collection of scripts using Microsofts Graph API

#### Step 1: Create Microsoft App (https://apps.dev.microsoft.com/#/appList)
Step 1a: Generate Secret (Copy secret for later)

Step 1b: Add Platform: 

	Type: Web

	Redirect URLs: http://localhost

Step 1c: Add Permissions based on API Resources you need to access (https://developer.microsoft.com/en-us/graph/docs/concepts/overview)

Step 1d: Save (Copy Application Id / Client ID)

#### Step 2: Import (Get-OAuthBearer)  

```Import-Module <Script Location>\Get-OAuthBearer.ps1```

#### Step 3: Get Bearer Access Token 

```$bearer = Get-OAuthBearer -clientId "55555555-5555-5555-5555-555555555555" -clientSecret "SuperSecret" -redirectUrl "http://localhost"```

#### Step 4: Use the Graph API with Access Token

##### Example: 

```$results = Invoke-RestMethod -Uri 'https://graph.microsoft.com/v1.0/me/mailFolders/' -Headers $bearer```

```$results.value | select displayName, id```

#### Graph API Resources:
https://developer.microsoft.com/en-us/graph/docs/concepts/overview

#### Microsoft API Resources: 
https://blogs.technet.microsoft.com/ronba/2016/05/09/using-powershell-and-the-office-365-rest-api-with-oauth/
