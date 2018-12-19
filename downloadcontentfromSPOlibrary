#Custom function using PnP PowerShell to download content from a SharePoint Online Library
#Make sure you install PnP PowerShell: https://docs.microsoft.com/en-us/powershell/sharepoint/sharepoint-pnp/sharepoint-pnp-cmdlets?view=sharepoint-ps
#thanks to https://skodvinhvammen.wordpress.com/2015/10/23/download-files-and-folders-using-office-365-dev-pnp-powershell-cmdlets/
#I just updated the script with the latest PnP commandlets

function download($url, $path, [int]$levels)
{
    $root = $web.GetFolderByServerRelativeUrl($url);
    $files = $root.Files;
    $web.Context.Load($root);
    $web.Context.Load($files);
    $web.Context.ExecuteQuery();
 
    if(!(Test-Path $path))
    {   
        New-Item -ItemType Directory $path | Out-Null   
    }
 
    foreach($file in $files)
    {   
        echo "Downloading $($file.ServerRelativeUrl) to $($path)"        
        Get-PnPFile -ServerRelativeUrl $file.ServerRelativeUrl -Path $path -Filename $file.Name -AsFile
    }
 
    if($levels -gt 0)
    {
        $folders = $root.Folders;
        $web.Context.Load($folders);
        $web.Context.ExecuteQuery();
 
        foreach($folder in $folders)
        {
                         
            download $folder.ServerRelativeUrl "$($path)\$($folder.Name)" ($levels -1 )
        }    
    }
}

#Site URL
$siteUrl = "https://tenant.sharepoint.com/sites/JHKontraktHndtering"

#Connect to SPO Site
Connect-PnPOnline $SiteUrl
$web = Get-PnPWeb
  
# Download all files 
# recurse 10 levels into subfolders  
$downloads = download "$siteUrl/Shared%20Documents" "C:\Development\Temp" 10
Write-Output "Completed"
