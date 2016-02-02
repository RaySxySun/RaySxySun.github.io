---
layout: post
title: 3. GitHub oauth-token, merge from pull request 
date: 2016-01-12
categories: CI
tags: [GitHub,CI]
description: To understand python class and metaclass
---
# 1. How to get GitHub oauth-token

> [GitHub api OAuth](https://developer.github.com/v3/oauth/#web-application-flow)

1. Login github -> setting -> [developer applications](https://raw.githubusercontent.com/RaySxySun/raysxysun.github.io/master/img/20160112/160112aboutPython1.png)

2. redirect by visiting 
    
    ```
    https://github.com/login/oauth/authorize
    Example
    https://github.com/login/oauth/authorize?client_id=xxxxxxxx&client_secret=xxxxxxx&scope=repo
   ```
   

Name | Type | Description
---|---|---
client_id | string | Required. The client ID you received from GitHub when you registered.
redirect_uri| string | The URL in your app where users will be sent after authorization. See details below about redirect urls.
scope|string|A ==comma== separated list of scopes.
state|string|An unguessable random string. It is used to protect against cross-site request forgery attacks.

   
  >About [scope](https://developer.github.com/v3/oauth/#scopes)

3. get the code from redirect url   

4. send 

    POST https://github.com/login/oauth/access_token

**Parameters**


Name | Type | Description
---|---|---
client_id| string|Required. The client ID you received from GitHub when you registered.
client_secret| string |Required. The client secret you received from GitHub when you registered.
code|string|**==Required.==** The code you received as a response to Step 1.
redirect_uri|string|The URL in your app where users will be sent after authorization. See details below about redirect urls.
state|string|The unguessable random string you optionally provided in Step 1

**Response** 
By default, the response will take the following form:

    access_token=e72e16c7e42f292c6912e7710c838347ae178b4a&scope=user%2Cgist&token_type=bearer
    

    
# 2. How to merge code based on ==pull request==

    cd foo
    git remote add [foo] git@github.com:[foo]/[foo].git
    vim .git/config
    
            [remote "foo"]
                url = git@github.com:[foo]/Mango.git
                fetch = +refs/heads/*:refs/remotes/[foo]/*
    
        #add  fetch = +refs/pull/*/head:refs/remotes/[foo]/pr/*
            [remote "foo"]
                        url = git@github.com:[foo]/Mango.git
                        fetch = +refs/heads/*:refs/remotes/[foo]/*
                        fetch = +refs/pull/*/head:refs/remotes/[foo]/pr/*
        #save
    
    git fetch remote
    
# 3. Install git (Linux)
 
    sudo apt-get install git-core openssh-server openssh-client
    ssh-keygen -t rsa
    #add ssh to github