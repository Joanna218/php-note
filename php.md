# **PHP**
[PHP manual](http://php.net/manual/zh/index.php)   
[PHP install](http://php.net/manual/zh/install.php)     
[PSR-2](http://www.php-fig.org/psr/psr-2/)  
[PSR coding style簡讀版](http://oomusou.io/php/php-psr2/)

## **Index**

1. [Getting Start](#Getting&nbsp;Start)
2. [Useful Script](#Useful&nbsp;Script)



---
## Getting&nbsp;Start

### PHP: Hypertext Preprocessor(超文本預處理器)
* 開源腳本語言
* 在Web可嵌入HTML
* 語法融合了C, Java, Perl

        <?php
            echo "Hi, I'm a PHP script!";
        ?>

### PHP標記

* \<?php your code ?> (建議乖乖用這種)
* \<script language="php"> your code \</script>
* \<? echo 'hello' ?> 或 \<?= your code ?> (需要在pnp.ini中啟用short\_open\_tag 或 編譯時下--enable-short-tags)
* \<% echo 'hello' %> 或 \<%= your code %> (ASP用法，需要在php.ini中啟用asp_tags)
* 只有開始標記 \<?php (PHP5.3+，並且標記後需要一個以上的空格)，如果檔案是純PHP，建議可以刪除
* PHP在輸出時會自動刪除?>後的一個換行      
[Note: A Note on Line Feeds](http://php.net/manual/en/tutorial.firstpage.php)

**※在PSR-2標準中，PHP標籤PHP必須使用\<?php ?>或\<?= ?>，不可以使用其他標籤。若PHP不包含HTML，則不使用結束標籤 ?>**

### PHP運用領域
* WEB: 最常用也是主要的應用處，需要PHP編譯器、伺服器和瀏覽器。
* CLI: 只需要PHP編譯器。
* 桌面應用: 使用PHP-GTK，可跨平台。
* PHP可以在主流OS上使用跟支援大多數Web Server(Apache, IIS...等)

### Hello World
* 輸出Hello World

        <?php echo '<p>Hello World</p>'; ?>


---
## Useful&nbsp;Script

* 顯示系統資訊(預設變數、已載入的模組、設定資訊...等)

        <?php phpinfo(); ?>

* 檢查使用者瀏覽器種類

        <?php
		    echo $_SERVER['HTTP_USER_AGENT'];
        ?>

* 判斷瀏覽器

        <?php
		    if (strpos($_SERVER['HTTP_USER_AGENT'], 'MSIE') !== FALSE) {
    		    echo 'Use Internet Explorer<br />';
		    } else {
                echo 'Not use Internet Explorer';
            }
		?>

* 混合 HTML & PHP

        <?php 
            if(strpos($_SERVER['HTTP_USER_AGENT'], 'MSIE') !== FALSE) {
        ?>

        <h3>Use Internet Explorer</h3>
        <?php
            } else {
        ?>

        <h3>Not use Internet Explorer</h3>
        <?php 
            } 
        ?>

    上面兩種邏輯一樣，但是當輸出的文檔很大的時候，不透過php可能會比較有效率。

* Form表單處理

        <form action="action.php" method="post">
            <p>Name: <input type="text" name="name" /></p>
            <p>Age : <input type="text" name="name" /></p>
            <p><input type="submit" /></p>
        </form>

    當使用者按下送出會調用action.php頁面。

        My name is <?php echo htmlspecialchars($_POST['name']); ?>.<br>
        I am <?php echo (int)$_POST['age']; ?> years old.

    $\_POST會存放post得到的資料。(還有$\_GET, $\_REQUEST)

    htmlspecialchars會將html中的特殊字元轉成可顯示的編碼。

    (int)可以將接在後面的資料轉成int形式("abc123" => 0, "123abc" => 123)

    ** 版本問題 **

    PHP4.1.0 版本後才有引入以下全域變數：
    $\_GET、$\_POST、$\_COOKIE、 $\_SERVER、$\_FILES、$\_ENV、 $\_REQUEST、$\_SESSION，

    $HTTP\_*\_VARS 已經在PHP5.4.0後棄用。


