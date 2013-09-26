---
layout: post
title: "Windows: Open a File from the Command Line with Sublime Text 2"
date: 2013-09-26 11:20
comments: true
categories: [work,development]
---

As I spend more time on the command line, I find myself seeking out convenience methods for doing things that usually mean a lot of clicking around, especially in a Windows environment. I'm also moving back and forth a bit between a Linux virtual machine and my regular work Windows OS, and I'm wanting certain things to exist in both places.

BTW: I'm running Windows 7 Professional 64-bit. Your mileage may vary.

## The Goal:
**```subl myfile.txt``` opens myfile.txt in Sublime Text 2 from a Windows command prompt**

## Step 1: Create a Windows batch file that will run when the command prompt is opened

* Create a file where ever you like called autorun.bat. I put mine in ```c:\dev\autorun.bat```
* Open autorun.bat in a text editor and add the following:
```
doskey subl="C:\Program Files\Sublime Text 2\sublime_text.exe" $*
```
(Replace the path to your Sublime Text executable as necessary.)

* Save the file

## Step 2: Add the autorun.bat file to your registry

* Open the Registry Editor (Start -> Type 'regedit' in the Search programs and files input)
* Click down into HKEY_CURRENT_USER\Software\Microsoft\Command Processor
* If an AutoRun value does not exist, create a new one
	* Right-click Command Processor and click New -> String Value
	* Name the new value 'AutoRun'
	* Right-click the new value and select Modify...
	* Enter the path to your autorun.bat in the Value data field. Mine says 'c:\dev\autorun.bat'
	* Click OK

## Step 3: Try it out

* Open a new command prompt
* cd to a directory with a file that can be opened with a text editor
* Type subl your-file-name.txt
* Be happy

## Resources
Without these helpful resources, none of this would have been possible.

* [Sublime Text from Command Line (Win7)](http://stackoverflow.com/questions/9440639/sublime-text-from-command-line-win7)
* [Doskey](http://www.microsoft.com/resources/documentation/windows/xp/all/proddocs/en-us/doskey.mspx?mfr=true)
* [Permanent Windows command-line aliases with doskey and AutoRun](http://darkforge.blogspot.co.uk/2010/08/permanent-windows-command-line-aliases.html)