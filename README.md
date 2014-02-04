Ken OKABE Tech Projects and Tutorials 
===================
[kenokabe.github.io]
==

[kenokabe.github.io]: http://kenokabe.github.io

###Brief Content Management System 
*No Server side involved. JS code on Client side does all.*

####Getting Ready

 - **/scheme.html** is a clone of the **index.html** (Auto Generated @setup).  Exclude **\<html\>** , Add **\<div id="doc"\>\</div\>**

 - **/javascripts/invoke.js** the bootstrapper

####Write Contents and Publish

 1. Edit a DOC on Dual Pane MarkDown Editor
  
 2. At the end of the file, add
 
 ```
<head><style type="text/css"><!-- body {display: none;} --></style><script src="/javascripts/invoke.js"></script></head>
 ```

 3. Obtain the HTML file via Editor, and upload to the top level of the Github repo

---
[http://kenokabe.github.io/test0] is an original html file.
[http://kenokabe.github.io/test0]: http://kenokabe.github.io/test0

[http://kenokabe.github.io/test] is a bootstrap html file modified by adding the 1 line code to call `invoke.js`.
[http://kenokabe.github.io/test]: http://kenokabe.github.io/test
