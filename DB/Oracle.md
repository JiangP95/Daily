## 一、Lpad函数
    lpad函数将左边的字符串填充一些特定的字符其语法格式如下：lpad(string,n,[pad_string])
    string:字符或者参数
    n:字符的长度，是返回的字符串的数量，如果这个数量比原字符串的长度要短，lpad函数将会把字符串截取成从左到右的n个字符;
    pad_string:可选参数，这个字符串是要粘贴到string的左边，若这个参数未写，lpad函数将会在string的左边粘贴空格。
    lpad('tech', 7); 将返回' tech'
    lpad('tech', 2); 将返回'te'
    lpad('tech', 8, '0'); 将返回'0000tech'
    lpad('tech on the net', 15, 'z'); 将返回'tech on the net'
    lpad('tech on the net', 16, 'z'); 将返回'ztech on the net'

## 二、Rpad函数
    rpad函数将右边的字符串填充一些特定的字符其语法格式如下：rpad(string,n,[pad_string])
    string:字符或者参数
    n:字符的长度，是返回的字符串的数量，如果这个数量比原字符串的长度要短，lpad函数将会把字符串截取成从左到右的n个字符;
    pad_string:可选参数，这个字符串是要粘贴到string的右边，如果这个参数未写，lpad函数将会在string的右边粘贴空格。
    rpad('tech', 7); 将返回' tech'
    rpad('tech', 2); 将返回'te'
    rpad('tech', 8, '0'); 将返回'tech0000'
    rpad('tech on the net', 15, 'z'); 将返回'tech on the net'
    rpad('tech on the net', 16, 'z'); 将返回'tech on the netz'