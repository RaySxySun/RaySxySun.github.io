---
layout: post
title: 6. shell tools
date: 2016-01-12
categories: shell
tags: [shell]
description: To understand python class and metaclass
---

#1. logging method
    
    logging()
    {
        local FUN_NAME="${1}"
        local LINE_NO="${2}"
        local LEVEL="${3}"
        local MSG="${4}"
        echo "[$(date '+%F %T')] [$0:${FUN_NAME}:${LINE_NO}] [${LEVEL}] ${MSG}" >> "${log_file}"
    }
    
    logging "$FUNCNAME" "$LINENO" "INFO" "Run hook [after_run_install]"

> **Build-in Keywords:** $FUNCNAME & $LINENO

---

# 2. print string with diff color

    stars="********************"
    cus_echo()
    {
        echo ""
        printf "\n\e[35m%s [ %s ] %s\e[0m\n" "${stars}" "$1" "${stars}"
    }
    
    red_echo()
    {
        printf "\n\e[31m%s\e[0m\n" "$@"
    }
    
    green_echo()
    {
        printf "\n\e[32m%s\e[0m\n" "$@"
    }
    
    yellow_echo()
    {
        printf "\n\e[33m%s\e[0m\n" "$@"
    }
    
    blue_echo()
    {
        printf "\n\e[34m%s\e[0m\n" "$@"
    }
    
    light_magenta_echo()
    {
        printf "\n\e[35m%s\e[0m\n" "$@"
    }
    
    cyan_echo()
    {
        printf "\n\e[36m%s\e[0m\n" "$@"
    }
    
> **Notice:** the color code is filled between "\e[" and "m"

    \e[30m -- \e[37m   foreground color

Code | Color
---|---
echo -e "\e[30m" | grey
echo -e "\e[31m" | red
echo -e "\e[32m" | green
echo -e "\e[33m" | yellow
echo -e "\e[34m" | blue
echo -e "\e[35m" | purple
echo -e "\e[36m" | light blue
echo -e "\e[37m" | white
   
    \e[40m -- \e[47m    background color 
 
 Code | Color
---|---  
echo -e "\e[40m"  |  grey
echo -e "\e[41m"  |  red
echo -e "\e[42m"  |  green
echo -e "\e[43m"  |  yellow
echo -e "\e[44m"  |  blue
echo -e "\e[45m"  |  purple
echo -e "\e[46m"  |  light blue
echo -e "\e[47m"  |  white

Code | function
---|---
  \033[0m  | close all attr   
  \033[1m  | set brightness   
  \03[4m   | underline   
  \033[5m  | blink   
  \033[7m  | reverse display   
  \033[8m  | blanking   
  \033[30m | --   \033[37m   foreground color   
  \033[40m |  --   \033[47m   background color   
  \033[nA  | move up N lines   
  \03[nB   | move down N lines   
  \033[nC  | move right N lines   
  \033[nD  | move left N lines   
  \033[y;xH |set cursor position   
  \033[2J   |clear screen   
  \033[K    |clear text from current position to the end   
  \033[s    |save cursor position   
  \033[u    |reset cursor position   
  \033[?25l | hide cursor   
  \33[?25h  | show cursor
  
 # 3. 
    
    2>&1  
    local n_br_nm="${INSTALL_NAME}"_$(date "+%s")
    curl -sS https://getcomposer.org/installer | php -- --filename=composer --install-dir=bin
    
     Problem 1
    - Installation request for onelogin/php-saml 2.5.0 -> satisfiable by onelogin/php-saml[2.5.0].
    - onelogin/php-saml 2.5.0 requires ext-mcrypt * -> the requested PHP extension mcrypt is missing from your system.
    
    for file in index.php install.php; do
        sed -i "1a ini_set('log_errors_max_len', 1024);" "${WEB_PATH}/${INSTALL_NAME}/${file}"
        sed -i "1a ini_set('display_errors', 0);" "${WEB_PATH}/${INSTALL_NAME}/${file}"
        sed -i "1a ini_set('error_log', '${WEB_PATH}/${INSTALL_NAME}/php_error.txt');" "${WEB_PATH}/${INSTALL_NAME}/${file}"
        sed -i "1a ini_set('log_errors', 1);" "${WEB_PATH}/${INSTALL_NAME}/${file}"
        sed -i "1a error_reporting(E_ALL & ~E_NOTICE & ~E_STRICT & ~E_DEPRECATED);" "${WEB_PATH}/${INSTALL_NAME}/${file}"
    done
    
    instance name number
    
    cron job  remove files  