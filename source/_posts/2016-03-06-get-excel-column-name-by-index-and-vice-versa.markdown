---
layout: post
title: "Get Excel column name by index and vice versa"
date: 2014-05-13 16:43:52 +0800
comments: true
categories: [csharp]
---
I used to got a need to build small and simple plugin for Excel using VSTO. But I won’t write about VSTO in this post. The thing that I want to share is just a simple code to get excel column name by index and vise versa. I felt it was very important for me cause I worked intensively with index at that project.

``` c# excel helper
public class ExcelUtility
    {
        /// <summary>
        /// Convert Excel column name into number/index. Column number start from 1. A equal to 1
        /// </summary>
        /// <param name="columnName">Column name
        /// <returns>Excel column in numeric</returns>
        public static int GetExcelColumnNumber(string columnName)
        {
            if (string.IsNullOrEmpty(columnName))
                throw new ArgumentNullException("Invalid column name parameter");

            columnName = columnName.ToUpperInvariant();

            int sum = 0;

            char ch;
            for (int i = 0; i < columnName.Length; i++)
            {
                ch = columnName[i];

                if (char.IsDigit(ch))
                    throw new ArgumentNullException("Invalid column name parameter on character " + ch);

                sum *= 26;
                sum += (ch - 'A' + 1);
                //sum += (columnName[i] - 'A');
            }

            return sum;
        }

        /// <summary>
        /// Convert Excel column index into name. Column start from 1
        /// </summary>
        /// <param name="columnNumber">Column name/number
        /// <returns>Column name</returns>
        public static string GetExcelColumnName(int columnNumber)
        {
            int dividend = columnNumber;
            string columnName = String.Empty;
            int modulo;

            while (dividend > 0)
            {
                modulo = (dividend - 1) % 26;
                columnName = Convert.ToChar(65 + modulo).ToString() + columnName;
                dividend = (int)((dividend - modulo) / 26);
            }

            return columnName;
        }
    }
```
``` c# sample code
class Program
    {
        static void Main(string[] args)
        {
            try
            {
                string columnName = "A";

                int columnIndex = ExcelUtility.GetExcelColumnNumber(columnName);

                Console.WriteLine("Index of {0} is {1} ", columnName, columnIndex);

                //increment columnIndex
                ++columnIndex;

                Console.WriteLine("Increment index = " + columnIndex);

                columnName = ExcelUtility.GetExcelColumnName(columnIndex);
                Console.WriteLine("Column name of index {0} is {1}", columnIndex, columnName);

            }
            catch (Exception ex)
            {
                Console.WriteLine(ex.Message);
            }

            Console.Read();

        }
    }
```
I write the code in my blog just to remind me in the future in case I get the same need. And it would be great if it can help others who have the same need as me :)

<h4>Reference<h4>
<ol type="1">
<li> http://stackoverflow.com/questions/181596/how-to-convert-a-column-number-eg-127-into-an-excel-column-eg-aa
</li> <li>http://stackoverflow.com/questions/667802/what-is-the-algorithm-to-convert-an-excel-column-letter-into-its-number</li>
</ol>
