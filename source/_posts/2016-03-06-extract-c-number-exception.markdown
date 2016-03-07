---
layout: post
title: "Extract C# exception"
date: 2014-09-08 16:49:39 +0800
comments: true
categories: [csharp]
---
Throwing or catching exception strategy on building an application is a very important to get detail exception message, inner exception and detail strack trace where or which part of line of code that fired the exception.

The following snipped code shows how to extract detail exception message, innner excepion and its detail strack trace to be recorded / written on log.

``` c# exception extractor
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
