#### 比较操作

一个数字和一个字符串进行比较，PHP会把字符串转换成数字再进行比较。PHP转换的规则的是：若字符串以数字开头，则取开头数字作为转换结果，若无则输出0。例如：123abc转换后应该是123，而abc则为0，0==0这当然是成立的啦！所以，0 =='abc'是成立的。当有一个对比参数是整数的时候，会把另外一个参数强制转换为整数。

#### Hash比较

`
"0e132456789"=="0e7124511451155" //true
"0e123456abc"=="0e1dddada" //false
"0e1abc"=="0"     //true
`

在进行比较运算时，如果遇到了0e\d+这种字符串，就会将这种字符串解析为科学计数法。所以上面例子中2个数的值都是0因而就相等了。如果不满足0e\d+这种模式就不会相等。

##### md5比较

<?php
$a = md5('240610708'); // = 0e462097431906509019562988736854

$b = md5('QNKCDZO'); // = 0e830400451993494058024219903391

var_dump($a == $b);
?>


返回结果bool(true)

240610708 和 QNKCDZO md5值类型相似，但并不相同，在“==”相等操作符的运算下，结果返回了true。这是个经典的漏洞，只需要找到md5值为0exxx（xxx全为数字,共30位），这里我提供4个都可以通过的值：240610708、QNKCDZO、aabg7XSs、aabC9RqS

扩展：
先注册密码为240610708的用户A。
然后用密码QNKCDZO尝试登录用户A。
倘若成功登录，则证明此网站采用了不完备的加密体制md5一次加密。

先注册密码为0E33455555的用户A。
然后用密码0E234230570345尝试登录用户A。
倘若成功登录，则证明此网站采用了明文进行存储密码！