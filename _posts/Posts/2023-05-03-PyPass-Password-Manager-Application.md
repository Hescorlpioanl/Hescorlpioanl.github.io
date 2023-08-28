---
title: "PyPass - Your Password Manager and Files Vault Application"
classes: wide
header:
  teaser: /assets/images/Blogs/Posts/pypass_image.png
ribbon: ForestGreen
description: "Review the features of PyPass password manager. . ."
categories:
  - Posts
tags:
  - Python
  - Project
toc: false
---

Hello world, Hossam is here, In this article, I will review the features of a project I have been working on for a while in the past, which is a simple password manager with many features that help me remember and encrypt my traffic cams, as well as encrypt and retrieve files in a simple way.

# About PyPass

PyPass is a simple and  free opensource password manager application that manage your passwords by storing them encrypted and give you an easy way to retrieve them anytime.

PyPass isn't a password manager only, it is also handle your secret files so you can keep your secret files and hide them in our application.

Application Repository on GitHub : [0xGhazy/PyPass](https://github.com/0xGhazy/PyPass)

# PyPass Features

- Storing passwords encrypted.
- Copy passwords to clipboard and QR Codes.
- Cool icons for social media platforms 😎.
- Hiding/Encrypting files and restoring them easily.
- Import and Export your encryption key.

`!Note: exporting database records isn't implemented at the time of writing this post.`

# PyPass Screens

## `Setup.py` script

First I need to setup application password by running `setup.py` script this will install all required libraries for the application and setup database tables.
here when you see the `Password>>` prompt we start type the main password to access our application in this case I put it `0000` to be easy and fast.

![Pasted image 20230504005926](https://user-images.githubusercontent.com/60070427/236076543-30c02e74-744f-4e21-b63d-2ac0b724f173.png)

## Login Screen

when you start running the application you will see the login screen that asks you for the password you entered in setup phase. it checks if your password match the database record and incase of valid credentials you'll be redirected to main screen, and for invalid credentials you will get `Invalid Password !!` error message.

![Pasted image 20230504010429](https://user-images.githubusercontent.com/60070427/236076575-f2387740-8820-48c6-95d3-ed1e597a34c0.png)

## Main Screen / Accounts

Here we are in the main screen that contains all tabs and navigations options, this screens consists of main 2 parts, the left part and the right one. in the left part such as `Accounts`, `Files`, `Settings` this all a navigation buttons that you can use to navigate through applications sections. the right part or the tab part it contains all features that you can do under this section such as list `saved accounts`, `add/edit/delete account` features for Accounts section as the image below.

![Pasted image 20230504010736](https://user-images.githubusercontent.com/60070427/236076676-a8b689ec-78a3-44fa-8fae-003d52cbd20f.png)

in the following image, you will see the saved accounts list. by clicking on any account listed on the list the application will display the plaintext in the `QR code image` and `copy the plaintext password to clipboard`. so you can past it and use it as you want.
I hope no one try to scan the QR below and hack me 😢.

![Pasted image 20230504011449](https://user-images.githubusercontent.com/60070427/236076740-a27e7019-222b-49e5-9d6d-5bcac515b73c.png)

We can add new accounts in under the `Edit Accounts` Tab we typing the account data such as platform name to get the platform icon, email, and password. by clicking the add button  the account will be stored in our database.

![Pasted image 20230504011836](https://user-images.githubusercontent.com/60070427/236076877-9daae8aa-1329-4789-89a3-fd978a737be7.png)

We can also edit entered accounts by clicking on account from the accounts list and we will see account automatically filled and waiting for edit email/password/platform. by clicking on` update button` after changing the email data the account will be updated in the database. by clicking on the `delete button` the account will be deleted as you know :).

![Pasted image 20230504012318](https://user-images.githubusercontent.com/60070427/236076970-84160481-b8ff-4305-b39a-3f1fd2ca6a9b.png)

## Files Screen

Here we handle the file hiding functionality, we can import files from hard desk to our application. this feature is about reading files in binary format then encryption it using the security key and store the content in a encrypted file in the `saved-files` directory then store file details in the database like the following images.

![Pasted image 20230504012818](https://user-images.githubusercontent.com/60070427/236077097-37c70924-0bec-4237-b14c-f77f4e8905b4.png)

`Export File` button will ask you where to export the original file on your hard desk after decrypting it. the `Restore File` button the file will be restored to the path that imported from.

## Settings Screen

Here in `Settings` tab we have to sections `User Profile`, and `Security`. in User Profile section we have can update username of the owner and change/import a new image by clicking on the small green button that appear in the image below.

![Pasted image 20230504013700](https://user-images.githubusercontent.com/60070427/236077204-13579a4d-7031-4126-b36c-e1c68a82ddd2.png)

In the Security section here we can import our encryption key or export it to store it in a backup media or uploading it to the cloud to be able to restore the files and passwords you entered earlier.

![Pasted image 20230504013933](https://user-images.githubusercontent.com/60070427/236077257-d343b4eb-f873-4654-a8bd-785f9d10c3a0.png)

# Future Work and Acknowledgment

As you saw above, the application initially needs many important developments and additions, such as:
- [x] Importing/Exporting the database records as a backup method.
- [x] Implementing a chrome extension that able to manage passwords from the browser.
- [x] Adding the time-window ability to help the user to login one time a day then ask the user to re enter the owner password.

Since it is an open source application, I am happy to see your opinion and ideas on how I can improve this application more and more, and I am more and more happy that you participate in the development of this project if you will, even by suggesting or discovering bugs and report it by creating an issue.

Finally I wanna thank some people who contributing on PyPass application
- [y8l](https://github.com/yusufadell)
- [Corey](https://github.com/corey-new)