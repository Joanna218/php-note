# **PHP**

[PHP manual](http://php.net/manual/zh/index.php)    
[PHP install](http://php.net/manual/zh/install.php)    
[PSR-2](http://www.php-fig.org/psr/psr-2/)    
[PSR coding style簡讀版](http://oomusou.io/php/php-psr2/)

Install phpcs for PSR check

        pear install PHP_CodeSniffer

Install vscode extension: phpcs

## **Index**

1. [Getting Start](#getting-start)
2. [Useful Script](#useful-script)
3. [變數型別](#變數型別)
4. [echo vs print vs print_r](#echo-vs-print-vs-print_r)
5. [isset vs empty vs is_null](#isset-vs-empty-vs-is_null)
6. [常用函數](#常用函數)

## Getting Start

### PHP: Hypertext Preprocessor(超文本預處理器)

* 開源腳本語言，物件導向
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

## Useful Script

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
    [string to number](http://php.net/manual/zh/language.types.string.php#language.types.string.conversion)

    **※版本問題**

    PHP4.1.0 版本後才有引入以下全域變數：
    $\_GET、$\_POST、$\_COOKIE、 $\_SERVER、$\_FILES、$\_ENV、 $\_REQUEST、$\_SESSION，

    $HTTP\_*\_VARS 已經在PHP5.4.0後棄用。

* 呼叫echo phpversion();或echo PHP_VERSION可以顯示php執行版本。

## 變數型別

[PHP types](http://php.net/manual/zh/language.types.intro.php)

### 純量型別

* [boolean](#boolean)
* [integer](#integer)
* [float(double)](#float(double))
* [string](#string)

### 複合型別

* [array](#array)
* [object](#object)
* [callable](#callable)

### 特殊型別

* [resource](#resource)
* [NULL](#NULL)

### 偽型別(pseudo-types)

* 用於指示參數可以使用的類型或值(非定義)

    + mixed (參數可以接受多種類型)
    + number (參數接受integer或float)
    + callback (參數接受callback, PHP5.4後使用callable)
    + array | object (參數接受array或object)
    + void (不接受參數)
    + $... (接受任意個參數)

### 查驗型別

* var_dump()

    > void var_dump ( mixed $expression [, mixed $... ] )

    回傳值和型別

* gettype()

    > string gettype ( mixed $var )

    回傳型別字串

    不要使用gettype()來測試型別，因為回傳值可能會因PHP版本不同，且需要用字串比較來測試會導致效率較差。使用is\_array(), is\_bool(), is\_null()...等來測試。

### 轉型

* 強制轉型

    [強轉](http://php.net/manual/zh/language.types.type-juggling.php#language.types.typecasting)

    加上前綴字
    + (int), (integer) 轉換成integer
    + (bool), (boolean) 轉換成boolean
    + (float), (double), (real) 轉換成float
    + (string) 轉換成 string
    + (array) 轉換成 array
    + (object) 轉換成 object
    + (unset) 轉換成 NULL (PHP 5+)
    + (binary), b 轉換成二進制字串 (不是二進制數，PHP 5.2.1+，PHP6.0+生效)
    + 變數用雙引號""括起來，強制轉換成字串

* 使用settype()

    > bool settype(mixed $var, string $type)

    轉換成功return TRUE, 失敗return FALSE

    type可填入    
    + "boolean" (PHP4.2+可使用"bool")
    + "integer" (PHP4.2+可使用"int")
    + "float" (PHP4.2+。更舊的版本使用"double")
    + "string"
    + "array"
    + "object"
    + "null" (PHP4.2+)

    PHP\_INT\_MAX = 2147483647 (PHP 5.0.5+)

### boolean

* TRUE or FALSE(不區分大小寫)

* 以下值轉換成boolean時默認為false:

    + false
    + 0
    + 0.0
    + "0"
    + 空字串, 空陣列
    + NULL

    其它值都會默認為true(包含所有resource, NAN)

### integer

* $a = 1234; (十進制)
* $a = -123; (負數)
* $a = 0123; (八進制。PHP 7+非法數字會parse error，舊版則是直接忽略包含非法數字及之後的字)
* $a = 0x1A; (十六進制)
* $a = 0b11111111 (二進制，PHP 5.4+)

* 整數範圍

    + PHP\_INT\_SIZE (integer byte size)
    + PHP\_INT\_MAX (integer 最大值，PHP4.4.0 & PHP5.0.5+)
    + PHP\_INT\_MIN (integer 最小值，PHP7.0+)
    + Windows PHP 7.0之前的版本只有32位元，所以PHP\_INT\_SIZE = 4

* 整數運算或給值超過int範圍，會return float

* 運算結果不為int時，return float。需要整數可以用強制轉型或 [round()](http://php.net/manual/zh/function.round.php) 四捨五入，round()不填第二個參數表示四捨五入到整數位，PHP5.3+引入mode參數。

### float(double)

* IEEE 754 雙精度格式，因此比較浮點數可能會有誤差。
* $a = 1.234;
* $a = 1.2e3;
* $a = 7E-10;

* NAN代表浮點數運算中未定義或無法表達的值，var\_dump(NAN) = float(NAN)，可用is\_NAN()來檢查。

### string

[PHP string](http://php.net/manual/zh/language.types.string.php)

* PHP string僅支援256字集，不支援Unicode
* PHP 7.0+ 64bits version，對string長度沒有特別限制，其它版本則有2147483647 bytes(約2GB)的限制。

* 單引號 $str = 'Hello';
    + 單引號中的特殊字元及變數不會被轉換

* 雙引號 $str = "Hello";
    + 雙引號中的特殊字元及變數會被轉換

* Heredoc結構

* Nowdoc結構(PHP 5.3+)

* 字串連接用 . (dot)

* 取得字串中的

### array

[PHP array](http://php.net/manual/zh/language.types.array.php)

* PHP的array實際上是種ordered map，有關聯性的keys和values，可以當作array, list, hash table, dictionary, collection, stack, queue...等結構來使用。

* 寫法

        $array = array(
            "foo" => "bar",
            "bar" => "foo",
        );

    PHP 5.4 + 可以使用

        $array = [
            "foo" => "bar",
            "bar" => "foo",
        ];

* key 可以是int或string，但會有一些自動轉換

    + 如果字串是合法的十進位數字會被轉成int存，"1"轉成1，"01"不會被轉。
    + 浮點數只會存整數部分(int)
    + true, false會被轉成1, 0(int)
    + Null 轉成 "" (string)
    + 重複key的話，後面的會覆蓋前面的。
    + 沒有給key的話，會取當前最大的整數index+1當作key(初始為0)，當然如果這個key有重複，前面的值就會被蓋掉。

* $array['foo'] 或 $array{'foo'} 都可以用來存取值，取得未定義的key-value會得到null並且有警告。

* 新增/修改

    + $array[key] = value; (新增一筆key-value)
    + $array[] = value; (此時會取當前最大的整數index+1當作key)
    + unset($array[key]) (刪除某個key-value)
    + unset($array) (刪除該陣列)
    + unset($array[key])並不會重建index，所以再$array[] = value;後，index仍然會從之前的key中找最大值。
    + 可以重新assign陣列，就會重建index，這時index就會再從0開始。

* array_diff()

    > array array_diff ( array $array1 , array $array2 [, array $... ] )

    可以return所有在array1中但不在其它array中的值(不比較key，且保留原key)。

* count($array) (計算$array element 個數)

* array 運算

    [array operators](http://php.net/manual/zh/language.operators.array.php)

    + $arr1 == $arr2 (若兩邊有相同的key-value，則return true)
    + $arr1 === $arr2 (若兩邊有相同的key-value，且順序型態相同，return true)
    + $arr1 != $arr2 或 $arr1 <> $arr2 (若兩邊不等於return true)
    + $arr1 !== $arr2 (兩邊不全等return true)
    + $arr1 + $arr2 (聯集，直接把$arr2內容加到$arr1後面，如果key重複只會保留$arr1的)

* [array function](http://php.net/manual/zh/ref.array.php)

* & 符號可以用來call by reference

        $arr1 = array(2, 3);
        $arr2 = $arr1;
        $arr2[] = 4; // $arr2 is changed,
                    // $arr1 is still array(2, 3)

        $arr3 = &$arr1;
        $arr3[] = 4; // now $arr1 and $arr3 are the same

* 迴圈中改值

    在PHP 5+，foreach開始時會自動從array的第一個key開始，舊版需要先reset()。    
    因為迴圈結束之後最後一個元素的$value reference仍然存在，建議用unset()來刪除。

        $arr = array(1, 2, 3, 4);
        foreach ($arr as &$value) {
            $value = $value * 2;
        }
        // $arr is now array(2, 4, 6, 8)
        unset($value);

* 使用list()來取值多維陣列(PHP 5.5.0+)

        $array = [
            [1, 2],
            [3, 4],
        ];

        foreach ($array as list($a, $b)) {
            // $a contains the first element of the nested array,
            // and $b contains the second element.
            echo "A: $a; B: $b\n";
        }

    Output:

        A: 1; B: 2
        A: 3; B: 4

* 輸出array內容 print_r($array);

### object

[PHP object](http://php.net/manual/zh/language.types.object.php)    
[PHP OOP](http://php.net/manual/en/language.oop5.php)

* Create Object and Instance

    使用 -> 存取物件中的屬性

        class foo
        {
            function do_foo()
            {
                echo "Doing foo.";
            }
        }

        $bar = new foo;
        $bar->do_foo();

* 物件轉換

    + 物件轉物件不會有任何變化
    + 純量型別變數轉物件會實體化一個物件並且包含成員變數scalar存值。

            $obj = (object) 'ciao';
            echo $obj->scalar;  // outputs 'ciao'

    + array轉物件會自動生成一個PHP基類的實例stdClass Object，並且包含array中的所有key(property)及對應值，但只能直接對string型態的key(property)存取值，int型態的需要用迴圈存取。

            $arr = [
                'a',
                'b',
                'c',
                'x' => 'x1',
                "10" => 'ten',
            ];
            print_r($arr);
            $obj = (object)$arr; //array convert to object
            print_r($obj);

        Output:

            Array
            (
                [0] => a
                [1] => b
                [2] => c
                [x] => x1
                [10] => ten
            )
            stdClass Object
            (
                [0] => a
                [1] => b
                [2] => c
                [x] => x1
                [10] => ten
            )

        $obj->x可以得到'x1'，其它的則要使用迴圈

    + (object)null會得到stdClass Object()

* 關於stdClass，是PHP預定義的基類之一，預設沒有內容，可以實體化並且定義變數。

### callable

[PHP callable](http://php.net/manual/zh/language.types.callable.php)

* call\_user\_func() 可以用來傳遞callable function

* 可以將函數用string形式傳遞

* object的method可以用array傳遞:　array($object, 'method')

* static method傳遞: 'Class::method'

* 父層method傳遞: array('ChildClass', 'parent::method')

### resource

[PHP resource](http://php.net/manual/zh/language.types.resource.php)

* 用於外部資源，一個資源通常會有專門的函數來建立和使用。

* 大部分時候Zend引擎會自動檢測不再被使用的resource，並且回收釋放。

### NULL

* NULL不區分大小寫

* 以下情況NULL

    + 直接assign NULL
    + 未給值
    + unset()

* is_null() 可用來檢測是否NULL

## echo vs print vs print_r

X | echo | print() | print_r()
---------|---------|----------|---------
型態 | 語句 | 函數 | 函數
接受參數 | 多個 | 一個 | 多個
參數型態 | string, number | string, number | mixed
回傳值 | 無 | int(1) | bool(true/false)

* print_r() 如果加上第二個參數true，則不會輸出而是return函數處理完的值。

## isset vs empty vs is_null

* 手冊說明

  [isset()官方說明](http://php.net/manual/en/function.isset.php)    
  [empty()官方說明](http://php.net/manual/en/function.empty.php)    
  [is_null()官方說明](http://php.net/manual/en/function.is-null.php)    

    + isset(): 檢查變數是否存在且不為null (可帶多個參數)    
    + empty(): 檢查變數值是否為空    
    + is_null(): 檢查變數值是否為null    

* 實驗結果

  (PHP 5.6.31)

  為統一輸出避免顯示被自動轉型，所以我用var_dump()來處理輸出

  輸出執行下方程式碼

        echo var_dump($var);
        echo var_dump(isset($var));
        echo var_dump(empty($var));
        echo var_dump(is_null($var));

    + 不宣告$var
        - $var出現未定義變數警告訊息，並回傳NULL
        - isset()回傳bool(false)
        - empty()回傳bool(true)
        - is_null()出現未定義變數警告訊息，並回傳bool(true)

    + 宣告$var = null;
        - $var輸出得到NULL
        - isset()回傳bool(false)
        - empty()回傳bool(true)
        - is_null()回傳bool(true)

    + 宣告$var = 0;
        - $var輸出得到int(0)
        - isset()回傳bool(true)
        - empty()回傳bool(true)
        - is_null()回傳bool(false)

    + 宣告$var = "0";
        - $var輸出得到string(1) "0"
        - isset()回傳bool(true)
        - empty()回傳bool(true)
        - is_null()回傳bool(false)

    + 宣告$var = "";
        - $var輸出得到string(0) ""
        - isset()回傳bool(true)
        - empty()回傳bool(true)
        - is_null()回傳bool(false)

    + 宣告$var = 1;
        - $var輸出得到int(1)
        - isset()回傳bool(true)
        - empty()回傳bool(false)
        - is_null()回傳bool(false)

* 結論
    + isset()只要變數存在不為null都會回傳true
    + empty()會把null, 0, "0", 空字串, 空陣列...等空集合的值都視為空值，回傳true
    + is_null()只在變數不存在或值為null時，回傳true

## 常用函數