# **PHP**
[PHP manual](http://php.net/manual/zh/index.php)

## **Index**

1. [Getting Start](#Getting&nbsp;Start)



---
## **Getting&nbsp;Start**

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
* \<? echo 'hello' ?> 或 \<?= your code ?> (需要在pnp.ini中啟用short_open_tag 或 編譯時下--enable-short-tags)
* \<% echo 'hello' %> 或 \<%= your code %> (ASP用法，需要在php.ini中啟用asp_tags)
* 只有開始標記 \<?php (PHP5.3+，並且標記後需要一個以上的空格)

### PHP運用領域
* WEB: 最常用也是主要的應用處，需要PHP編譯器、伺服器和瀏覽器。
* CLI: 只需要PHP編譯器。
* 桌面應用: 使用PHP-GTK，可跨平台。
* PHP可以在主流OS上使用跟支援大多數Web Server(Apache, IIS...等)

### Hello World
* 輸出Hello World

        <?php echo '<p>Hello World</p>'; ?>

### Useful Script

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

