---
layout: post
title: 2. shell grouding
date: 2016-01-08
categories: shell
tags: [shell]
description: To understand python class and metaclass
---
## 1\. How does eval work
**For Example** 
    
    #!/usr/bin/env bash
    #for shname in `ls *.sh`
    #do
    #          name=`echo "$shname" | awk -F. '{print $1}'`
    #          echo $name
    #done
    readonly fun="$1"
    #readonly config="$2"
    
    setEnv()
    {
    '''
     In function, there will be an error if we read arguments
     #readonly config="$2"
     error line 11: $config: ambiguous redirect
    '''
    readConfig=false
    while read line; do
        if [[ "$line" = "[default]" ]]; then
            readConfig=true
            continue
        fi
    
        if [[ $readConfig && ${line:0:1} != "#" ]]
            then
                key=${line%=*}
                val=${line#*=}
           #    echo $key
           #    echo $val
    
                 export $key="$val"
                 '''
                 command echo will be excuted twice 
                 '''
                 eval echo "$"${key}
        fi
    
    done < $config
    }
    
    
    
    #IFS='='
    #while read k v
    #do
    #        echo $k
    #        echo $v
    #done < ./JenkinsEnv.ini
    
    
    __main()
    {
    
        if [[ "${fun}" == "All" ]]; then
            setEnv
        else
            ${fun}
        fi
    }
    
    __main
    
    
    
    
    
    
    
    ## Python 

> [Python 3.5.1 document](https://docs.python.org/3/)

---

### 1. import most used module

    import os
    import sys
    import getopt
    import datetime
    import platform

---
### 2. Method

Define a method

    def parse_command(self):
    
Call a method

    self.parse_parameters()

##### example: Parse command line options

     if sys.argv[1] == "install":
         short_opt = "hp:i:"
         long_opt = [
                "help",
                "install-dir=",
                "git-pull-request-number="
         ]
         try:
             params, args = getopt.getopt(sys.argv[2:], short_opt, long_opt)
             for key, value in params:
                if key in ("--install-dir"):

---

### 3. OS

     if not os.path.exists(tmp_path):
        os.makedirs(tmp_path)
     self.tmp_config["tmp_path"] = tmp_path
     
    