---
layout: post
title: 4. Python Journey
date: 2016-01-12
categories: python
tags: [python]
description: To understand python class and metaclass
---

## Python 

> [Python 3.5.1 document](https://docs.python.org/3/)

---

### 1. import most used module

    import os
    import sys
    import getopt
    import datetime
    import platform
    import logging
    import urllib
    import urllib2
    import logging.handlers

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
 1). os.path.exists
 
 2). os.makedirs
 
     if not os.path.exists(tmp_path):
        os.makedirs(tmp_path)
     self.tmp_config["tmp_path"] = tmp_path
 
 3). os.environ.get("FOO")
 
    if os.environ.get("ATOI_INSTALL_HOOK"):
 4). os.execlp("sh", "sh", "-c", cmd)   
     
---

### 4. Dictionary

    install_config = {
            "building_method": 0,
            "name":           None,
            "db_name":        None,
            "update":         1, # update composer
            "debugging":      0,
        }
    
get the value by key from dictionary

    dict.get(key, default=None)        
    
    
---

### 5. Format string
> [string methods (including str.format)](https://docs.python.org/3/library/stdtypes.html?highlight=str%20format#str.format)

    "%s, %s"%('hello', 'ray')
    "{0}, {1}, {0}".format('hello', 'ray')
    
Similar to str.format(**mapping), except that mapping is used directly and not copied to a dict. This is useful if for example mapping is a dict subclass:

    class Default(dict):
        def __missing__(self, key):
            return key
    
    '{name} was born in {country}'.format_map(Default(name='Guido'))
    'Guido was born in country'

---

### 6. Import logging & logging.handlers
     
    def sc_get_logger(log_file):
        handler = logging.handlers.RotatingFileHandler(log_file, maxBytes=1024 * 1024, backupCount=5)
        fmt = "[%(asctime)s] [%(filename)s:%(lineno)s] [%(levelname)s]: %(message)s"
    
        formatter = logging.Formatter(fmt)
        handler.setFormatter(formatter)
    
        logger = logging.getLogger('install_log')
        logger.addHandler(handler)
        logger.setLevel(logging.DEBUG)

    if not logger:
        print "fail to create a Rotating logger"
        exit(1)

    return logger
    
>     **NODE:** Rollover occurs whenever the current log file is nearly maxBytes in length. 
>     If backupCount is >= 1, the system will successively create new files with 
>     the same pathname as the base file, but with extensions ".1", ".2" etc. 
>     appended to it.

    CRITICAL : 'CRITICAL',
    ERROR : 'ERROR',
    WARNING : 'WARNING',
    INFO : 'INFO',
    DEBUG : 'DEBUG',
    NOTSET : 'NOTSET',
    
    NOTSET < DEBUG < INFO < WARNING < ERROR < CRITICAL
   
**LogRecord attributes** [wormhole](https://docs.python.org/3/library/logging.html?highlight=asctime%20lineno#logrecord-attributes)

Attribute name | Format | Description
---|---| ---
asctime | %(asctime)s | Human-readable time when the LogRecord was created. By default this is of the form ‘2003-07-08 16:49:45,896’ (the numbers after the comma are millisecond portion of the time).
filename|%(filename)s|Filename portion of pathname.
lineno | %(lineno)d |Source line number where the logging call was issued (if available).
levelname|%(levelname)s|Text logging level for the message ('DEBUG', 'INFO', 'WARNING', 'ERROR', 'CRITICAL').
message|%(message)s|The logged message, computed as msg % args. This is set when Formatter.format() is invoked.

     
     
[16.8. logging.handlers — Logging handlers](https://docs.python.org/3/library/logging.handlers.html?highlight=rotatingfilehandler#rotatingfilehandler)

<pre>
16.8.1. StreamHandler
16.8.2. FileHandler
16.8.3. NullHandler
16.8.4. WatchedFileHandler
16.8.5. BaseRotatingHandler
16.8.6. RotatingFileHandler
16.8.7. TimedRotatingFileHandler
16.8.8. SocketHandler
16.8.9. DatagramHandler
16.8.10. SysLogHandler
16.8.11. NTEventLogHandler
16.8.12. SMTPHandler
16.8.13. MemoryHandler
16.8.14. HTTPHandler
16.8.15. QueueHandler
16.8.16. QueueListener
</pre>
   
### 7. Web

    import urllib
    import urllib2
   
   
    def get_pull_info(self):
        id = self.__config__["install_info"]["id"]
        url = "https://api.github.com/repos/xxxxx/Mango/pulls/%s" % pull_number

        commons.cus_normal_print("Getting pull request [{0:s}] info...".format(pull_number))
        self.__logger__.info("Getting pull request{0:s} info from URL: {1:s}".format(pull_number, url))

        token = "token {0:s}".format(self.__install_config__["token"])
        request = urllib2.Request(url)
        request.add_header('User-Agent', 'gbyukg')
        request.add_header('Content-Type', 'application/json')
        request.add_header('Authorization', token)

        try:
            response = urllib2.urlopen(request)
        except urllib2.URLError, e:
            err_info = "Get pull request [{0:s}] info wrong: HTTP request code [{1:s}] with message [{2:s}]".format(
                str(pull_number),
                str(e.code),
                str(e.read()))

            self.__logger__.error(err_info)
            print err_info
            exit(1)
   
   
   
   
   
   
   
   
   
   
   ---
# How to get oauth-token from github api
1. Login github -> setting -> developer applications
2. redirect by visiting 
    
    ```
    https://github.com/login/oauth/authorize
    Example
    https://github.com/login/oauth/authorize?client_id=xxxxxxxx&client_secret=xxxxxxx&scope=repo
   ```
  >About [scope](https://developer.github.com/v3/oauth/#scopes)
3. get the code from redirect url   

4. send POST request https://github.com/login/oauth/access_token
   
   
   
   
   