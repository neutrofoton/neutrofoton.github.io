---
layout: post
title: "Jomla Encryption in C#"
date: 2012-08-06 10:21:53 +0800
comments: true
categories: [csharp, php]
---
Couple days ago I got a task to migrate user from an existing website which build based on Jomla CMS to an ASP.NET website user. Both of them use different encryption technique. The plan was that the Jomla based website would be replaced with the .NET website. In sort the scenario was :

1. Replace PHP website build on top Jomla CMS with .NET
2. Migrate users from Jomla so they can login into the new .NET website using old password
3. Migrate some roles

The first thing that come to my mind is to check whether they use the different encryption algorithm or not. And yup…they use different algorithm :( . So I decide to extend the <code>MembershipProvider</code> used by the .NET. In my case, thankfully I just need to override one method on that is <code>ValidateUser</code>. I will not talk the the detail what inside the methods is. The main point is just to validate if user login using existing password created by jomla can be validated by the .NET. To do that I create a simple class called <code>JomlaEncryption</code> to validate if user enter valid Jomla password.

``` c# encrypt with jomla algorithm
public class JomlaEncryption
{
    public static bool IsValidPassword(string password, string encryptedPassword)
    {
        return EncryptUsingJomlaAlgorithm(password, encryptedPassword) == encryptedPassword;
    }

    /// <summary>
    /// Source : http://stackoverflow.com/questions/2727043/using-php-to-create-a-joomla-user-password
    ///   From joomla Forum, that's what happen behind:
    ///
    ///   1. Generate a password
    ///   2. Generate 32 random characters
    ///   3. Concatenate 1 and 2
    ///   4. md5(3)
    ///   5. store 4:2
    ///   Example:
    ///
    ///   1. Generate a password - we'll use 'password'
    ///   2. Generate 32 random characters - we'll use 'WnvTroeiBmd5bjGmmsVUnNjppadH7giK'
    ///   3. Concatenate 1 and 2 - passwordWnvTroeiBmd5bjGmmsVUnNjppadH7giK
    ///   4. md5(3) - 3c57ebfec712312f30c3fd1981979f58
    ///   5. store 4:2 - 3c57ebfec712312f30c3fd1981979f58:WnvTroeiBmd5bjGmmsVUnNjppadH7giK
    /// </summary>
    /// <param name="password">
    /// <returns></returns>
    static string EncryptUsingJomlaAlgorithm(string password, string encryptedPassword)
    {
        if (!encryptedPassword.Contains(":"))
            return null;

        string randChar32Bit = encryptedPassword.Split(':')[1];
        if (string.IsNullOrEmpty(randChar32Bit) || randChar32Bit.Length != 32)
            return null;

        string concatePassAndRandChar32Bit = password + randChar32Bit;
        string hashedConcatePassAndRandChar32Bit = MD5(concatePassAndRandChar32Bit);

        return string.Format("{0}:{1}", hashedConcatePassAndRandChar32Bit, randChar32Bit);
    }

    static string MD5(string password)
    {
        byte[] bytes = System.Text.Encoding.Default.GetBytes(password);
        try
        {
            System.Security.Cryptography.MD5CryptoServiceProvider cryptProvider;
            cryptProvider = new System.Security.Cryptography.MD5CryptoServiceProvider();
            byte[] hash = cryptProvider.ComputeHash(bytes);
            string ret = string.Empty;
            foreach (byte a in hash)
            {
                if (a < 16)
                    ret += "0" + a.ToString("x");
                else
                    ret += a.ToString("x");
            }
            return ret;
        }
        catch
        {
            throw;
        }
    }
}
```

That's all for now and thanks for reading :)
