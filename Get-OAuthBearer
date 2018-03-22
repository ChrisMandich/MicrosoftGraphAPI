Function Get-OAuthBearer{
    <#
    .SYNOPSIS
    This is a simple Powershell Script to retrieve an Auth Token from Microsoft. 
    .EXAMPLE
    Get-OAuthBearer -clientId "55555555-5555-5555-5555-555555555555" -clientSecret "SuperSecret" -redirectUrl "http://localhost"
    .LINK
    https://github.com/ChrisMandich/RandomPowershellScripts
    #>

    # This article was used as a reference point to create this script
    # https://blogs.technet.microsoft.com/ronba/2016/05/09/using-powershell-and-the-office-365-rest-api-with-oauth/

    Param (
        $clientId = $(throw "-clientId is required."),
        $clientSecret = $(throw "-clientSecret is required."),
        $redirectUrl = $(throw "-redirectUrl is required.")
    )

    #Variables
    $httpUtility = "https://graph.microsoft.com" #set to Graph API since we are accessing this httpUtility

    #### Start ####
    #This piece of code was taken from the referenced document in the comments
    Function Show-OAuthWindow
    {
        param(
            [System.Uri]$Url
        )


        Add-Type -AssemblyName System.Windows.Forms
 
        $form = New-Object -TypeName System.Windows.Forms.Form -Property @{Width=440;Height=640}
        $web  = New-Object -TypeName System.Windows.Forms.WebBrowser -Property @{Width=420;Height=600;Url=($url ) }
        $DocComp  = {
            $Global:uri = $web.Url.AbsoluteUri
            if ($Global:Uri -match "error=[^&]*|code=[^&]*") {$form.Close() }
        }
        $web.ScriptErrorsSuppressed = $true
        $web.Add_DocumentCompleted($DocComp)
        $form.Controls.Add($web)
        $form.Add_Shown({$form.Activate()})
        $form.ShowDialog() | Out-Null

        $queryOutput = [System.Web.HttpUtility]::ParseQueryString($web.Url.Query)
        $output = @{}
        foreach($key in $queryOutput.Keys){
            $output["$key"] = $queryOutput[$key]
        }
    
        $output
    }

    Add-Type -AssemblyName System.Web

    $loginUrl = "https://login.microsoftonline.com/common/oauth2/authorize?response_type=code&redirect_uri=" + 
                [System.Web.HttpUtility]::UrlEncode($redirectUrl) + 
                "&client_id=$clientId" + "&scope=Mail.Read+Mail.ReadWrite" +
                "&prompt=login"

    $queryOutput = Show-OAuthWindow -Url $loginUrl

    $AuthorizationPostRequest = 
        "grant_type=authorization_code" + "&" +
        "redirect_uri=" + [System.Web.HttpUtility]::UrlEncode($redirectUrl) + "&" +
        "client_id=$clientId" + "&" +
        "client_secret=" + [System.Web.HttpUtility]::UrlEncode("$clientSecret") + "&" +
        "code=" + $queryOutput["code"] + "&" +
        "resource=" + [System.Web.HttpUtility]::UrlEncode($httpUtility)

    $Authorization = 
        Invoke-RestMethod   -Method Post `
                            -ContentType application/x-www-form-urlencoded `
                            -Uri https://login.microsoftonline.com/common/oauth2/token `
                            -Body $AuthorizationPostRequest
    #### END ####

    return @{Authorization =("Bearer "+ $Authorization.access_token)}

}
