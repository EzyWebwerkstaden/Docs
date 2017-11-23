# From nuspec to octopus channels
This guide will be about how to convert from using different nuspec files in dev/master/feature branch to just relaying on the branches instead and using octopus channels.

## First step: Setup VCS to listen on all branches not just default.

You will need to update so it will listen to all branches.
http://build.ezyserver.se/admin/editVcsRoot.html?action=editVcsRoot&vcsRootId=VivaaerobusAndVivaColombia_VivaNew_VivaDev&editingScope=buildType%3AVivaaerobusAndVivaColombia_VivaNew_PackageDev&cameFromUrl=%2Fadmin%2FeditBuildTypeVcsRoots.html%3Finit%3D1%26id%3DbuildType%253AVivaaerobusAndVivaColombia_VivaNew_PackageDev&cameFromTitle=Edit%20Build%20Configuration

Branch specification
```
#! escape: \
+:refs/heads/*
```

## Second step: Add another build step , so we can set correct build numbers depending on branch.

http://build.ezyserver.se/admin/editRunType.html?id=buildType:VivaaerobusAndVivaColombia_VivaNew_PackageDev&runnerId=RUNNER_13

```
# These are project build parameters in TeamCity
# Depending on the branch, we will use different major/minor versions
$majorMinorVersionMaster = "%system.MajorMinorVersion.Master%"
$majorMinorVersionDevelop = "%system.MajorMinorVersion.Develop%"

# TeamCity's auto-incrementing build counter; ensures each build is unique
$buildCounter = "%build.counter%" 

# This gets the name of the current Git branch. 
$branch = "%teamcity.build.branch%"

# Sometimes the branch will be a full path, e.g., 'refs/heads/master'. 
# If so we'll base our logic just on the last part.
if ($branch.Contains("/")) 
{
  $branch = $branch.substring($branch.lastIndexOf("/")).trim("/")
}

Write-Host "Branch: $branch"

if($branch -eq "<default>")
{
  $branch = "master"
  Write-Host "Modified branch name from default to Branch: $branch"
}

if ($branch -eq "master") 
{
  $buildNumber = "${majorMinorVersionMaster}.${buildCounter}"
}
elseif ($branch -match "release-.*") 
{
 $specificRelease = ($branch -replace 'release-(.*)','$1')
 $buildNumber = "${specificRelease}.${buildCounter}"
}
else
{
 # If the branch starts with "feature-", just use the feature name
 $branch = $branch.replace("feature-", "")
 $buildNumber = "${majorMinorVersionDevelop}.${buildCounter}-${branch}"
}

Write-Host "##teamcity[buildNumber '$buildNumber']"
```

## Step Three: Update publish triggers
We need to update the Finish build trigger , to listen on all branches. 
Branch filter: 
```
+:*
```

## Step Four: Set channels in octopus
https://octopus.ezyhosting.se/app#/projects/vivaaerobus-web-navitaire/channels