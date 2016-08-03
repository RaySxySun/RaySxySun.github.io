---
layout: post
title: JS Hands-on Experience
date: 2016-10-24
weather: cloudy
categories: JS
tags: [JS]
description:
---

# Check Browsers

          function myBrowser(){
              var userAgent = navigator.userAgent;
              var isOpera = userAgent.indexOf("Opera") > -1; //Opera
              var isIE = userAgent.indexOf("compatible") > -1 && userAgent.indexOf("MSIE") > -1 && !isOpera; //IE
              var isFF = userAgent.indexOf("Firefox") > -1; //Firefox
              var isSafari = userAgent.indexOf("Safari") > -1; //Safari
              if (isIE) {
                  var IE5 = IE55 = IE6 = IE7 = IE8 = false;
                  var reIE = new RegExp("MSIE (\\d+\\.\\d+);");
                  reIE.test(userAgent);
                  var fIEVersion = parseFloat(RegExp["$1"]);
                  IE55 = fIEVersion == 5.5;
                  IE6 = fIEVersion == 6.0;
                  IE7 = fIEVersion == 7.0;
                  IE8 = fIEVersion == 8.0;
                  if (IE55) {
                      return "IE55";
                  }
                  if (IE6) {
                      return "IE6";
                  }
                  if (IE7) {
                      return "IE7";
                  }
                  if (IE8) {
                      return "IE8";
                  }
              }//isIE end
              if (isFF) {
                  return "FF";
              }
              if (isOpera) {
                  return "Opera";
              }
          }
