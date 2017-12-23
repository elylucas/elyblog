---
title: Add a little salt to your hashed passwords with System.Web.Helpers.Crypto
author: ely
type: post
date: 2011-05-24T17:25:25+00:00
url: /post/add-a-little-salt-to-your-hashed-passwords-with-system-web-helpers-crypto/
dsq_thread_id:
  - 1627753837

---
In the news lately, there have been many incidents of data being stolen.&#160; On sensitive information, it’s a good practice to use encryption to add an extra layer of protection in case your data does get stolen.&#160; For passwords, a cryptographic function called a hash is often used.&#160; A hash takes a string of data, and returns an (hopefully) unique value that can be used to identify the original string.&#160; Hashing is a form of one way encryption, which means that once you hash something, you can’t un-hash it.

When you store your passwords as a hash of the original password, it doesn’t seem obvious at first how you can check to see if the password the user provides is correct.&#160; If you can’t decrypt a hash, how are you suppose to check to see if the password provided is correct?&#160; Instead of trying to decrypt the hash, you would instead hash the provided password and compare the two hashes.

There is a fundamental problem with just using plain hashes, however.&#160; When using the same hashing algorithm, the same string will always compute the same hash.&#160; Therefore, if someone was to get ahold of your database full of hashed passwords, they could compare the passwords against a list of known hashes (which are readily available and known as a [Rainbow Table][1]).&#160; Using a highly complex and secure password might help ensure that its hashed equivalent will never end up in a Rainbow Table, however, that is not going to help all those winners out there using tigerblood for the password.&#160; Its your job to help protect those people by using a technique called salting when generating your stored hashes.

Salting involves generating some random bytes (the salt) and injecting them into both the original password, and then the stored hash.&#160; For instance, if the password is tigerblood and the random bytes are %34gsDz, then the password that we will actually hash will be tigerblood%34gsDz.&#160; When we get back the hash for tigerblood%34gsDz, we will then inject the salt in a pre-determined fashion into the hash (pre-determined so that we will know how to pull it out later).&#160; 

When the user tries to log in, we will pull the hash out of the database, extract the salt back out, append the salt to the provided password, then hash that password and compare the hashes.&#160; Since there are some random bits added to every password, every salted hash will produce a unique value, and therefore, render the computed hashes stored in Rainbow Tables useless.

This sounds like a complicated process, however, the new Crypto library in the System.Web.Helpers assembly (included with Webmatrix and MVC3), automatically adds a salt to the passwords hashed with the static HashPassword function (unlike the Hash function which does not use a salt).&#160; The VerifyHashedPassword function will correctly extract the salt and use it to compare the hashes.

While this libraries imply that they should be used in the presentation layer due to their name, it is safe to include a reference to System.Web.Helpers.dll in your services and repository layers.&#160; Now there is no excuse to not hash passwords, since System.Web.Helpers.Crypto makes the process a single line of code.&#160;

 [1]: http://en.wikipedia.org/wiki/Rainbow_table