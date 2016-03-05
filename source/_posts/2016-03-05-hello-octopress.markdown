---
layout: post
title: "Hello octopress"
date: 2016-03-05 00:07:02 +0800
comments: true
categories: "Life"
---

Sample post
<ul>
  <li>Item 1</li>
  <li>Item 2</li>
  <li>Item 3</li>
</ul>

``` csharp sample code
/// <summary>
    /// Extracting <seealso cref="System.Exception"> class include its inner exception
    /// </seealso></summary>
    public sealed class Extractor
    {
        /// <summary>
        /// Parse the specified ex.
        /// </summary>
        /// <param name="exception">Ex.
        public static string Parse(System.Exception exception)
        {
            string errMessage = Environment.NewLine;

            errMessage += FormatErrorDescription(exception);

            while (exception.InnerException != null)
            {
                errMessage += ExtractInnerException(exception.InnerException);
                exception = exception.InnerException;
            }

            errMessage += Environment.NewLine;

            return errMessage;
        }



        /// <summary>
        /// Extract exception message and its strack trace
        /// </summary>
        /// <param name="innerException">inner excepion object
        /// <returns>extracted exception info including message and strack trace</returns>
        static string ExtractInnerException(System.Exception innerException)
        {
            string errMessage = string.Empty;

            errMessage += Environment.NewLine + " InnerException ";
            errMessage += FormatErrorDescription(innerException);

            return errMessage;
        }

        /// <summary>
        /// Formating exception description format
        /// </summary>
        /// <param name="exception">
        /// <returns>Error description</returns>
        static string FormatErrorDescription(System.Exception exception)
        {
            if (exception == null)
                return string.Empty;

            return Environment.NewLine + exception.Message + Environment.NewLine + exception.StackTrace;
        }
    }
```

``` bash sample

#!/bin/bash

cd $ROOT_DIR
DOT_FILES="lastpass weechat ssh Xauthority"
for dotfile in $DOT_FILES; do conform_link "$DATA_DIR/$dotfile" ".$dotfile"; done

# TODO: refactor with suffix variables (or common cron values)

case "$PLATFORM" in
    linux)
        #conform_link "$CONF_DIR/shell/zshenv" ".zshenv"
        crontab -l > $ROOT_DIR/tmp/crontab-conflict-arch
        cd $ROOT_DIR/$CONF_DIR/cron
        if [[ "$(diff ~/tmp/crontab-conflict-arch crontab-current-arch)" == ""
            ]];
            then # no difference with current backup
                logger "$LOG_PREFIX: crontab live settings match stored "\
                    "settings; no restore required"
                rm ~/tmp/crontab-conflict-arch
            else # current crontab settings in file do not match live settings
                crontab $ROOT_DIR/$CONF_DIR/cron/crontab-current-arch
                logger "$LOG_PREFIX: crontab stored settings conflict with "\
                    "live settings; stored settings restored. "\
                    "Previous settings recorded in ~/tmp/crontab-conflict-arch."
        fi
    ;;
```

Sample picture


Hello test test contecnt



{% img left /images/ironman.png This is ironman %}
Hello test test contecnt
