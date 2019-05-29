---
title: javascript之传输加密
date: 2019-03-04 19:52:29
tags: "javascript"
---
为什么要使用javascript加密呢？
服务端加密远远不够，客户端或者浏览器端也需要加密，以此保证传输信息过程的安全。

今天就我工作中说说这么几种加密算法及其对应的应用场景，如下所示:
- base64
- md5
- des

<!--more-->
### 一、Base64
Base64通常可以用于Cookie加密，比如每个用户通过相关操作，对应的用户和数据库信息会有对应的更新，为了保证对应的用户在web端看到的信息一致，我们使用Cookie，而Cookie如果是明文的话，不是特别安全，因此我们采用Base64对其进行加密。

示例代码如下:
```
<!DOCTYPE html>
<html>
<head>
<meta charset="utf-8" />
<title>base64加密解密</title>
</head>
<body>
<script>
// 创建Base64对象
var Base64 = {
 _keyStr: "ABCDEFGHIJKLMNOPQRSTUVWXYZabcdefghijklmnopqrstuvwxyz0123456789+/=",
 encode: function(e) {
  var t = "";
  var n, r, i, s, o, u, a;
  var f = 0;
  e = Base64._utf8_encode(e);
  while (f < e.length) {
   n = e.charCodeAt(f++);
   r = e.charCodeAt(f++);
   i = e.charCodeAt(f++);
   s = n >> 2;
   o = (n & 3) << 4 | r >> 4;
   u = (r & 15) << 2 | i >> 6;
   a = i & 63;
   if (isNaN(r)) {
    u = a = 64
   } else if (isNaN(i)) {
    a = 64
   }
   t = t + this._keyStr.charAt(s) + this._keyStr.charAt(o) + this._keyStr.charAt(u) + this._keyStr.charAt(a)
  }
  return t
 },
 decode: function(e) {
  var t = "";
  var n, r, i;
  var s, o, u, a;
  var f = 0;
  e=e.replace(/[^A-Za-z0-9+/=]/g,"");
  while (f < e.length) {
   s = this._keyStr.indexOf(e.charAt(f++));
   o = this._keyStr.indexOf(e.charAt(f++));
   u = this._keyStr.indexOf(e.charAt(f++));
   a = this._keyStr.indexOf(e.charAt(f++));
   n = s << 2 | o >> 4;
   r = (o & 15) << 4 | u >> 2;
   i = (u & 3) << 6 | a;
   t = t + String.fromCharCode(n);
   if (u != 64) {
    t = t + String.fromCharCode(r)
   }
   if (a != 64) {
    t = t + String.fromCharCode(i)
   }
  }
  t = Base64._utf8_decode(t);
  return t
 },
 _utf8_encode: function(e) {
  e = e.replace(/rn/g, "n");
  var t = "";
  for (var n = 0; n < e.length; n++) {
   var r = e.charCodeAt(n);
   if (r < 128) {
    t += String.fromCharCode(r)
   } else if (r > 127 && r < 2048) {
    t += String.fromCharCode(r >> 6 | 192);
    t += String.fromCharCode(r & 63 | 128)
   } else {
    t += String.fromCharCode(r >> 12 | 224);
    t += String.fromCharCode(r >> 6 & 63 | 128);
    t += String.fromCharCode(r & 63 | 128)
   }
  }
  return t
 },
 _utf8_decode: function(e) {
  var t = "";
  var n = 0;
  var r = c1 = c2 = 0;
  while (n < e.length) {
   r = e.charCodeAt(n);
   if (r < 128) {
    t += String.fromCharCode(r);
    n++
   } else if (r > 191 && r < 224) {
    c2 = e.charCodeAt(n + 1);
    t += String.fromCharCode((r & 31) << 6 | c2 & 63);
    n += 2
   } else {
    c2 = e.charCodeAt(n + 1);
    c3 = e.charCodeAt(n + 2);
    t += String.fromCharCode((r & 15) << 12 | (c2 & 63) << 6 | c3 & 63);
    n += 3
   }
  }
  return t
 }
}
// 定义字符串
var string = 'http://www.youcongtech.com!';
// 加密
var encodedString = Base64.encode(string);
console.log(encodedString);
// 解密
var decodedString = Base64.decode(encodedString);
console.log(decodedString);
</script>
</body>
</html>
```

### 二、MD5

#### 1.MDT算法特点
(1)压缩性:任意长度的数据，算出的MD5值长度都是固定的;
(2)容易计算:从原数据计算出MD5值很容易;
(3)抗修改性:对原数据进行任何改动，哪怕只修改1个字节，所得到的MD5值都有很大区别;
(4)弱抗碰撞:已知原数据和其MD5值，想找到一个具有相同MD5值的数据(既伪造数据)是非常困难的;
(5)强抗碰撞:想找到两个不同的数据，使它们具有相同的MD5值，是非常困难的;

根据以上特点衍生出来可以供我们使用的特性:
(1)方便存储:MD5加密处理都是32位的字符串，能够给定固定大小的空间存储、传输、验证;
(2)文件加密:MD5算法运用在文件加密上很有优势，因为只需要32位字符串就能对一个巨大的文件进行验证完整性;
(3)不可逆:MD5加密出来只会截取末尾32位，具有良好的安全性，如果是对于参数加密很难伪造MD5;
(4)加密损耗低:MD5算法加密对于性能的消耗微乎其微(据说0.001毫秒)

#### 2.MD5算法的实际应用
(1)用户密码
对于用户密码加密最高境界就是:别人获得你数据库的用户资料，别人也没有办法获知密码。
一般常用的规则比如:MD5(用户名+用户密码)+MD5(KEY+项目名+公司名)这样可以避免和别人碰库，不排除别人可能用MD5算法来攻击你的服务器。当然了，你还可以多包几层，可以MD5和其它加密算法混合使用(比如DES等)。

(2)请求参数校验
对于服务器而言，排除系统问题，最大的问题就是害怕请求被拦截，拦截修改之后就有很多漏洞的可能性。通常为了避免被拦截，会对请求参数进行校验，就算拦截了请求参数修改了，只要模拟不出MD5加密出来的值，服务器的过滤器会直接将其拦截。

(3)文件校验
对于一些图片或者是一些比较小的文件来说，可以不用MD5算法校验，基本上都是一次请求就完成了上传，而且显示的时候也不需要验证图片的不完整性。
如果有一个5MB的文件，客户端将其分割成5份1MB的文件，文件在上传的时候，上传两个MD5值，一个是当前上传的1MB文件流的MD5，另一个是拼接之后的MD5，通过这样的方式也能保证文件的完整性。
示例代码如下:

index.html
```
<html>
<head>
</head>
<body>
<script src="md5.js"></script>
    <script>
        var code = "123456";
        var username = "123456";
        var password = "123456";
        var str1 = hex_md5("123456");
        var str2 = b64_md5("123456");
        var str3 = str_md5("123456");
        var str4 = hex_hmac_md5(code,code);
        var str5 = b64_hmac_md5(username,username);
        var str6 = str_hmac_md5(password,password);
        console.log(str1);            // e10adc3949ba59abbe56e057f20f883e
        console.log(str2);            // 4QrcOUm6Wau+VuBX8g+IPg
        console.log(str3);            // áÜ9IºY«¾VàWò>
        console.log(str4);            // 30ce71a73bdd908c3955a90e8f7429ef
        console.log(str5);            // MM5xpzvdkIw5VakOj3Qp7w
        console.log(str6);            // 0Îq§;Ý9U©t)ï
    </script>
</body>
</html>
```

md5.js
```
/*
 * A JavaScript implementation of the RSA Data Security, Inc. MD5 Message
 * Digest Algorithm, as defined in RFC 1321.
 * Version 2.1 Copyright (C) Paul Johnston 1999 - 2002.
 * Other contributors: Greg Holt, Andrew Kepert, Ydnar, Lostinet
 * Distributed under the BSD License
 * See http://pajhome.org.uk/crypt/md5 for more info.
 */

/*
 * Configurable variables. You may need to tweak these to be compatible with
 * the server-side, but the defaults work in most cases.
 */
var hexcase = 0;  /* hex output format. 0 - lowercase; 1 - uppercase        */
var b64pad  = ""; /* base-64 pad character. "=" for strict RFC compliance   */
var chrsz   = 8;  /* bits per input character. 8 - ASCII; 16 - Unicode      */

/*
 * These are the functions you'll usually want to call
 * They take string arguments and return either hex or base-64 encoded strings
 */
function hex_md5(s){ return binl2hex(core_md5(str2binl(s), s.length * chrsz));}
function b64_md5(s){ return binl2b64(core_md5(str2binl(s), s.length * chrsz));}
function str_md5(s){ return binl2str(core_md5(str2binl(s), s.length * chrsz));}
function hex_hmac_md5(key, data) { return binl2hex(core_hmac_md5(key, data)); }
function b64_hmac_md5(key, data) { return binl2b64(core_hmac_md5(key, data)); }
function str_hmac_md5(key, data) { return binl2str(core_hmac_md5(key, data)); }

/*
 * Perform a simple self-test to see if the VM is working
 */
function md5_vm_test()
{
  return hex_md5("abc") == "900150983cd24fb0d6963f7d28e17f72";
}

/*
 * Calculate the MD5 of an array of little-endian words, and a bit length
 */
function core_md5(x, len)
{
  /* append padding */
  x[len >> 5] |= 0x80 << ((len) % 32);
  x[(((len + 64) >>> 9) << 4) + 14] = len;

  var a =  1732584193;
  var b = -271733879;
  var c = -1732584194;
  var d =  271733878;

  for(var i = 0; i < x.length; i += 16)
  {
    var olda = a;
    var oldb = b;
    var oldc = c;
    var oldd = d;

    a = md5_ff(a, b, c, d, x[i+ 0], 7 , -680876936);
    d = md5_ff(d, a, b, c, x[i+ 1], 12, -389564586);
    c = md5_ff(c, d, a, b, x[i+ 2], 17,  606105819);
    b = md5_ff(b, c, d, a, x[i+ 3], 22, -1044525330);
    a = md5_ff(a, b, c, d, x[i+ 4], 7 , -176418897);
    d = md5_ff(d, a, b, c, x[i+ 5], 12,  1200080426);
    c = md5_ff(c, d, a, b, x[i+ 6], 17, -1473231341);
    b = md5_ff(b, c, d, a, x[i+ 7], 22, -45705983);
    a = md5_ff(a, b, c, d, x[i+ 8], 7 ,  1770035416);
    d = md5_ff(d, a, b, c, x[i+ 9], 12, -1958414417);
    c = md5_ff(c, d, a, b, x[i+10], 17, -42063);
    b = md5_ff(b, c, d, a, x[i+11], 22, -1990404162);
    a = md5_ff(a, b, c, d, x[i+12], 7 ,  1804603682);
    d = md5_ff(d, a, b, c, x[i+13], 12, -40341101);
    c = md5_ff(c, d, a, b, x[i+14], 17, -1502002290);
    b = md5_ff(b, c, d, a, x[i+15], 22,  1236535329);

    a = md5_gg(a, b, c, d, x[i+ 1], 5 , -165796510);
    d = md5_gg(d, a, b, c, x[i+ 6], 9 , -1069501632);
    c = md5_gg(c, d, a, b, x[i+11], 14,  643717713);
    b = md5_gg(b, c, d, a, x[i+ 0], 20, -373897302);
    a = md5_gg(a, b, c, d, x[i+ 5], 5 , -701558691);
    d = md5_gg(d, a, b, c, x[i+10], 9 ,  38016083);
    c = md5_gg(c, d, a, b, x[i+15], 14, -660478335);
    b = md5_gg(b, c, d, a, x[i+ 4], 20, -405537848);
    a = md5_gg(a, b, c, d, x[i+ 9], 5 ,  568446438);
    d = md5_gg(d, a, b, c, x[i+14], 9 , -1019803690);
    c = md5_gg(c, d, a, b, x[i+ 3], 14, -187363961);
    b = md5_gg(b, c, d, a, x[i+ 8], 20,  1163531501);
    a = md5_gg(a, b, c, d, x[i+13], 5 , -1444681467);
    d = md5_gg(d, a, b, c, x[i+ 2], 9 , -51403784);
    c = md5_gg(c, d, a, b, x[i+ 7], 14,  1735328473);
    b = md5_gg(b, c, d, a, x[i+12], 20, -1926607734);

    a = md5_hh(a, b, c, d, x[i+ 5], 4 , -378558);
    d = md5_hh(d, a, b, c, x[i+ 8], 11, -2022574463);
    c = md5_hh(c, d, a, b, x[i+11], 16,  1839030562);
    b = md5_hh(b, c, d, a, x[i+14], 23, -35309556);
    a = md5_hh(a, b, c, d, x[i+ 1], 4 , -1530992060);
    d = md5_hh(d, a, b, c, x[i+ 4], 11,  1272893353);
    c = md5_hh(c, d, a, b, x[i+ 7], 16, -155497632);
    b = md5_hh(b, c, d, a, x[i+10], 23, -1094730640);
    a = md5_hh(a, b, c, d, x[i+13], 4 ,  681279174);
    d = md5_hh(d, a, b, c, x[i+ 0], 11, -358537222);
    c = md5_hh(c, d, a, b, x[i+ 3], 16, -722521979);
    b = md5_hh(b, c, d, a, x[i+ 6], 23,  76029189);
    a = md5_hh(a, b, c, d, x[i+ 9], 4 , -640364487);
    d = md5_hh(d, a, b, c, x[i+12], 11, -421815835);
    c = md5_hh(c, d, a, b, x[i+15], 16,  530742520);
    b = md5_hh(b, c, d, a, x[i+ 2], 23, -995338651);

    a = md5_ii(a, b, c, d, x[i+ 0], 6 , -198630844);
    d = md5_ii(d, a, b, c, x[i+ 7], 10,  1126891415);
    c = md5_ii(c, d, a, b, x[i+14], 15, -1416354905);
    b = md5_ii(b, c, d, a, x[i+ 5], 21, -57434055);
    a = md5_ii(a, b, c, d, x[i+12], 6 ,  1700485571);
    d = md5_ii(d, a, b, c, x[i+ 3], 10, -1894986606);
    c = md5_ii(c, d, a, b, x[i+10], 15, -1051523);
    b = md5_ii(b, c, d, a, x[i+ 1], 21, -2054922799);
    a = md5_ii(a, b, c, d, x[i+ 8], 6 ,  1873313359);
    d = md5_ii(d, a, b, c, x[i+15], 10, -30611744);
    c = md5_ii(c, d, a, b, x[i+ 6], 15, -1560198380);
    b = md5_ii(b, c, d, a, x[i+13], 21,  1309151649);
    a = md5_ii(a, b, c, d, x[i+ 4], 6 , -145523070);
    d = md5_ii(d, a, b, c, x[i+11], 10, -1120210379);
    c = md5_ii(c, d, a, b, x[i+ 2], 15,  718787259);
    b = md5_ii(b, c, d, a, x[i+ 9], 21, -343485551);

    a = safe_add(a, olda);
    b = safe_add(b, oldb);
    c = safe_add(c, oldc);
    d = safe_add(d, oldd);
  }
  return Array(a, b, c, d);

}

/*
 * These functions implement the four basic operations the algorithm uses.
 */
function md5_cmn(q, a, b, x, s, t)
{
  return safe_add(bit_rol(safe_add(safe_add(a, q), safe_add(x, t)), s),b);
}
function md5_ff(a, b, c, d, x, s, t)
{
  return md5_cmn((b & c) | ((~b) & d), a, b, x, s, t);
}
function md5_gg(a, b, c, d, x, s, t)
{
  return md5_cmn((b & d) | (c & (~d)), a, b, x, s, t);
}
function md5_hh(a, b, c, d, x, s, t)
{
  return md5_cmn(b ^ c ^ d, a, b, x, s, t);
}
function md5_ii(a, b, c, d, x, s, t)
{
  return md5_cmn(c ^ (b | (~d)), a, b, x, s, t);
}

/*
 * Calculate the HMAC-MD5, of a key and some data
 */
function core_hmac_md5(key, data)
{
  var bkey = str2binl(key);
  if(bkey.length > 16) bkey = core_md5(bkey, key.length * chrsz);

  var ipad = Array(16), opad = Array(16);
  for(var i = 0; i < 16; i++)
  {
    ipad[i] = bkey[i] ^ 0x36363636;
    opad[i] = bkey[i] ^ 0x5C5C5C5C;
  }

  var hash = core_md5(ipad.concat(str2binl(data)), 512 + data.length * chrsz);
  return core_md5(opad.concat(hash), 512 + 128);
}

/*
 * Add integers, wrapping at 2^32. This uses 16-bit operations internally
 * to work around bugs in some JS interpreters.
 */
function safe_add(x, y)
{
  var lsw = (x & 0xFFFF) + (y & 0xFFFF);
  var msw = (x >> 16) + (y >> 16) + (lsw >> 16);
  return (msw << 16) | (lsw & 0xFFFF);
}

/*
 * Bitwise rotate a 32-bit number to the left.
 */
function bit_rol(num, cnt)
{
  return (num << cnt) | (num >>> (32 - cnt));
}

/*
 * Convert a string to an array of little-endian words
 * If chrsz is ASCII, characters >255 have their hi-byte silently ignored.
 */
function str2binl(str)
{
  var bin = Array();
  var mask = (1 << chrsz) - 1;
  for(var i = 0; i < str.length * chrsz; i += chrsz)
    bin[i>>5] |= (str.charCodeAt(i / chrsz) & mask) << (i%32);
  return bin;
}

/*
 * Convert an array of little-endian words to a string
 */
function binl2str(bin)
{
  var str = "";
  var mask = (1 << chrsz) - 1;
  for(var i = 0; i < bin.length * 32; i += chrsz)
    str += String.fromCharCode((bin[i>>5] >>> (i % 32)) & mask);
  return str;
}

/*
 * Convert an array of little-endian words to a hex string.
 */
function binl2hex(binarray)
{
  var hex_tab = hexcase ? "0123456789ABCDEF" : "0123456789abcdef";
  var str = "";
  for(var i = 0; i < binarray.length * 4; i++)
  {
    str += hex_tab.charAt((binarray[i>>2] >> ((i%4)*8+4)) & 0xF) +
           hex_tab.charAt((binarray[i>>2] >> ((i%4)*8  )) & 0xF);
  }
  return str;
}

/*
 * Convert an array of little-endian words to a base-64 string
 */
function binl2b64(binarray)
{
  var tab = "ABCDEFGHIJKLMNOPQRSTUVWXYZabcdefghijklmnopqrstuvwxyz0123456789+/";
  var str = "";
  for(var i = 0; i < binarray.length * 4; i += 3)
  {
    var triplet = (((binarray[i   >> 2] >> 8 * ( i   %4)) & 0xFF) << 16)
                | (((binarray[i+1 >> 2] >> 8 * ((i+1)%4)) & 0xFF) << 8 )
                |  ((binarray[i+2 >> 2] >> 8 * ((i+2)%4)) & 0xFF);
    for(var j = 0; j < 4; j++)
    {
      if(i * 8 + j * 6 > binarray.length * 32) str += b64pad;
      else str += tab.charAt((triplet >> 6*(3-j)) & 0x3F);
    }
  }
  return str;
}
```
### 三、DES加密

DES是一种典型的块加密方法:将固定长度的明文通过一系列复杂的操作变成同样长度的密文，块的长度为64位。
同时，DES使用的密钥来自定义变换过程，因此算法认为只有持有加密所用的密钥的用户才能解密密文。DES的密钥表明上是64位，实际有效密钥长度为56位，其余8位可以用于奇偶校验。

DES现在已经不被视为一种安全的加密算法，主要原因是它使用的56位密钥过短。

为了提供实用所需的安全性，可以使用DES的派生算法，3DES来进行加密(虽然3DES也存在理论上的攻击方法)

示例(DES加密和解密):
index.html
```
<!DOCTYPE HTML>
<html>
<head>
<meta http-equiv="Content-Type" content="text/html; charset=UTF-8" />
	<script type="text/javascript" src="des.js"></script>
	<script>		
		function getResult(){
			//待加密字符串
			var str = document.getElementById("str").innerHTML;
			//第一个参数必须；第二个、第三个参数可选
			var key1 = "youcongtech";
			var key2 = "test001";
			var key3 = "test002"; 
			//加密方法		
			var  enResult = strEnc(str,key1,key2,key3);			
			//解密方法
			var deResult = strDec(enResult,key1,key2,key3);
			//展示结果
			document.getElementById("enStr").innerHTML = enResult; 
			document.getElementById("dnStr").innerHTML = deResult; 
		}
	</script>
</head>
<body>
<input type="button" value="获取加密结果与解密结果" onclick="getResult()" />
	<table>
		<tr>
		  <td align="left">字符串：</td>
		  <td><span id="str">admin</span></td>		 
		</tr>
		<tr>		  
		  <td>加密key：</td>
		  <td>key1=<span id="key1">1</span>;key2=<span id="key2">2</span>;key3=<span id="key3">3</span></td>
		</tr>
		<tr>
		  <td align="left">加密结果：</td>
		  <td align="left"><label id = "enStr"></label></td>
		</tr>
		<tr>
		  <td align="left">解密结果： </td>
		  <td align="left"><label id = "dnStr"></label></td>
		</tr>
	<table>
</body>
</html>

```

des.js
```
/**
* DES加密/解密
* @Copyright Copyright (c) 2006
* @author Guapo
* @see DESCore
*/

/*
* encrypt the string to string made up of hex
* return the encrypted string
*/
function strEnc(data,firstKey,secondKey,thirdKey){

 var leng = data.length;
 var encData = "";
 var firstKeyBt,secondKeyBt,thirdKeyBt,firstLength,secondLength,thirdLength;
 if(firstKey != null && firstKey != ""){    
   firstKeyBt = getKeyBytes(firstKey);
   firstLength = firstKeyBt.length;
 }
 if(secondKey != null && secondKey != ""){
   secondKeyBt = getKeyBytes(secondKey);
   secondLength = secondKeyBt.length;
 }
 if(thirdKey != null && thirdKey != ""){
   thirdKeyBt = getKeyBytes(thirdKey);
   thirdLength = thirdKeyBt.length;
 }  
 
 if(leng > 0){
   if(leng < 4){
     var bt = strToBt(data);      
     var encByte ;
     if(firstKey != null && firstKey !="" && secondKey != null && secondKey != "" && thirdKey != null && thirdKey != ""){
       var tempBt;
       var x,y,z;
       tempBt = bt;        
       for(x = 0;x < firstLength ;x ++){
         tempBt = enc(tempBt,firstKeyBt[x]);
       }
       for(y = 0;y < secondLength ;y ++){
         tempBt = enc(tempBt,secondKeyBt[y]);
       }
       for(z = 0;z < thirdLength ;z ++){
         tempBt = enc(tempBt,thirdKeyBt[z]);
       }        
       encByte = tempBt;        
     }else{
       if(firstKey != null && firstKey !="" && secondKey != null && secondKey != ""){
         var tempBt;
         var x,y;
         tempBt = bt;
         for(x = 0;x < firstLength ;x ++){
           tempBt = enc(tempBt,firstKeyBt[x]);
         }
         for(y = 0;y < secondLength ;y ++){
           tempBt = enc(tempBt,secondKeyBt[y]);
         }
         encByte = tempBt;
       }else{
         if(firstKey != null && firstKey !=""){            
           var tempBt;
           var x = 0;
           tempBt = bt;            
           for(x = 0;x < firstLength ;x ++){
             tempBt = enc(tempBt,firstKeyBt[x]);
           }
           encByte = tempBt;
         }
       }        
     }
     encData = bt64ToHex(encByte);
   }else{
     var iterator = parseInt(leng/4);
     var remainder = leng%4;
     var i=0;      
     for(i = 0;i < iterator;i++){
       var tempData = data.substring(i*4+0,i*4+4);
       var tempByte = strToBt(tempData);
       var encByte ;
       if(firstKey != null && firstKey !="" && secondKey != null && secondKey != "" && thirdKey != null && thirdKey != ""){
         var tempBt;
         var x,y,z;
         tempBt = tempByte;
         for(x = 0;x < firstLength ;x ++){
           tempBt = enc(tempBt,firstKeyBt[x]);
         }
         for(y = 0;y < secondLength ;y ++){
           tempBt = enc(tempBt,secondKeyBt[y]);
         }
         for(z = 0;z < thirdLength ;z ++){
           tempBt = enc(tempBt,thirdKeyBt[z]);
         }
         encByte = tempBt;
       }else{
         if(firstKey != null && firstKey !="" && secondKey != null && secondKey != ""){
           var tempBt;
           var x,y;
           tempBt = tempByte;
           for(x = 0;x < firstLength ;x ++){
             tempBt = enc(tempBt,firstKeyBt[x]);
           }
           for(y = 0;y < secondLength ;y ++){
             tempBt = enc(tempBt,secondKeyBt[y]);
           }
           encByte = tempBt;
         }else{
           if(firstKey != null && firstKey !=""){                      
             var tempBt;
             var x;
             tempBt = tempByte;
             for(x = 0;x < firstLength ;x ++){                
               tempBt = enc(tempBt,firstKeyBt[x]);
             }
             encByte = tempBt;              
           }
         }
       }
       encData += bt64ToHex(encByte);
     }      
     if(remainder > 0){
       var remainderData = data.substring(iterator*4+0,leng);
       var tempByte = strToBt(remainderData);
       var encByte ;
       if(firstKey != null && firstKey !="" && secondKey != null && secondKey != "" && thirdKey != null && thirdKey != ""){
         var tempBt;
         var x,y,z;
         tempBt = tempByte;
         for(x = 0;x < firstLength ;x ++){
           tempBt = enc(tempBt,firstKeyBt[x]);
         }
         for(y = 0;y < secondLength ;y ++){
           tempBt = enc(tempBt,secondKeyBt[y]);
         }
         for(z = 0;z < thirdLength ;z ++){
           tempBt = enc(tempBt,thirdKeyBt[z]);
         }
         encByte = tempBt;
       }else{
         if(firstKey != null && firstKey !="" && secondKey != null && secondKey != ""){
           var tempBt;
           var x,y;
           tempBt = tempByte;
           for(x = 0;x < firstLength ;x ++){
             tempBt = enc(tempBt,firstKeyBt[x]);
           }
           for(y = 0;y < secondLength ;y ++){
             tempBt = enc(tempBt,secondKeyBt[y]);
           }
           encByte = tempBt;
         }else{
           if(firstKey != null && firstKey !=""){            
             var tempBt;
             var x;
             tempBt = tempByte;
             for(x = 0;x < firstLength ;x ++){
               tempBt = enc(tempBt,firstKeyBt[x]);
             }
             encByte = tempBt;
           }
         }
       }
       encData += bt64ToHex(encByte);
     }                
   }
 }
 return encData;
}

/*
* decrypt the encrypted string to the original string 
*
* return  the original string  
*/
function strDec(data,firstKey,secondKey,thirdKey){
 var leng = data.length;
 var decStr = "";
 var firstKeyBt,secondKeyBt,thirdKeyBt,firstLength,secondLength,thirdLength;
 if(firstKey != null && firstKey != ""){    
   firstKeyBt = getKeyBytes(firstKey);
   firstLength = firstKeyBt.length;
 }
 if(secondKey != null && secondKey != ""){
   secondKeyBt = getKeyBytes(secondKey);
   secondLength = secondKeyBt.length;
 }
 if(thirdKey != null && thirdKey != ""){
   thirdKeyBt = getKeyBytes(thirdKey);
   thirdLength = thirdKeyBt.length;
 }
 
 var iterator = parseInt(leng/16);
 var i=0;  
 for(i = 0;i < iterator;i++){
   var tempData = data.substring(i*16+0,i*16+16);    
   var strByte = hexToBt64(tempData);    
   var intByte = new Array(64);
   var j = 0;
   for(j = 0;j < 64; j++){
     intByte[j] = parseInt(strByte.substring(j,j+1));
   }    
   var decByte;
   if(firstKey != null && firstKey !="" && secondKey != null && secondKey != "" && thirdKey != null && thirdKey != ""){
     var tempBt;
     var x,y,z;
     tempBt = intByte;
     for(x = thirdLength - 1;x >= 0;x --){
       tempBt = dec(tempBt,thirdKeyBt[x]);
     }
     for(y = secondLength - 1;y >= 0;y --){
       tempBt = dec(tempBt,secondKeyBt[y]);
     }
     for(z = firstLength - 1;z >= 0 ;z --){
       tempBt = dec(tempBt,firstKeyBt[z]);
     }
     decByte = tempBt;
   }else{
     if(firstKey != null && firstKey !="" && secondKey != null && secondKey != ""){
       var tempBt;
       var x,y,z;
       tempBt = intByte;
       for(x = secondLength - 1;x >= 0 ;x --){
         tempBt = dec(tempBt,secondKeyBt[x]);
       }
       for(y = firstLength - 1;y >= 0 ;y --){
         tempBt = dec(tempBt,firstKeyBt[y]);
       }
       decByte = tempBt;
     }else{
       if(firstKey != null && firstKey !=""){
         var tempBt;
         var x,y,z;
         tempBt = intByte;
         for(x = firstLength - 1;x >= 0 ;x --){
           tempBt = dec(tempBt,firstKeyBt[x]);
         }
         decByte = tempBt;
       }
     }
   }
   decStr += byteToString(decByte);
 }      
 return decStr;
}
/*
* chang the string into the bit array
* 
* return bit array(it's length % 64 = 0)
*/
function getKeyBytes(key){
 var keyBytes = new Array();
 var leng = key.length;
 var iterator = parseInt(leng/4);
 var remainder = leng%4;
 var i = 0;
 for(i = 0;i < iterator; i ++){
   keyBytes[i] = strToBt(key.substring(i*4+0,i*4+4));
 }
 if(remainder > 0){
   keyBytes[i] = strToBt(key.substring(i*4+0,leng));
 }    
 return keyBytes;
}

/*
* chang the string(it's length <= 4) into the bit array
* 
* return bit array(it's length = 64)
*/
function strToBt(str){  
 var leng = str.length;
 var bt = new Array(64);
 if(leng < 4){
   var i=0,j=0,p=0,q=0;
   for(i = 0;i<leng;i++){
     var k = str.charCodeAt(i);
     for(j=0;j<16;j++){      
       var pow=1,m=0;
       for(m=15;m>j;m--){
         pow *= 2;
       }        
       bt[16*i+j]=parseInt(k/pow)%2;
     }
   }
   for(p = leng;p<4;p++){
     var k = 0;
     for(q=0;q<16;q++){      
       var pow=1,m=0;
       for(m=15;m>q;m--){
         pow *= 2;
       }        
       bt[16*p+q]=parseInt(k/pow)%2;
     }
   }  
 }else{
   for(i = 0;i<4;i++){
     var k = str.charCodeAt(i);
     for(j=0;j<16;j++){      
       var pow=1;
       for(m=15;m>j;m--){
         pow *= 2;
       }        
       bt[16*i+j]=parseInt(k/pow)%2;
     }
   }  
 }
 return bt;
}

/*
* chang the bit(it's length = 4) into the hex
* 
* return hex
*/
function bt4ToHex(binary) {
 var hex;
 switch (binary) {
   case "0000" : hex = "0"; break;
   case "0001" : hex = "1"; break;
   case "0010" : hex = "2"; break;
   case "0011" : hex = "3"; break;
   case "0100" : hex = "4"; break;
   case "0101" : hex = "5"; break;
   case "0110" : hex = "6"; break;
   case "0111" : hex = "7"; break;
   case "1000" : hex = "8"; break;
   case "1001" : hex = "9"; break;
   case "1010" : hex = "A"; break;
   case "1011" : hex = "B"; break;
   case "1100" : hex = "C"; break;
   case "1101" : hex = "D"; break;
   case "1110" : hex = "E"; break;
   case "1111" : hex = "F"; break;
 }
 return hex;
}

/*
* chang the hex into the bit(it's length = 4)
* 
* return the bit(it's length = 4)
*/
function hexToBt4(hex) {
 var binary;
 switch (hex) {
   case "0" : binary = "0000"; break;
   case "1" : binary = "0001"; break;
   case "2" : binary = "0010"; break;
   case "3" : binary = "0011"; break;
   case "4" : binary = "0100"; break;
   case "5" : binary = "0101"; break;
   case "6" : binary = "0110"; break;
   case "7" : binary = "0111"; break;
   case "8" : binary = "1000"; break;
   case "9" : binary = "1001"; break;
   case "A" : binary = "1010"; break;
   case "B" : binary = "1011"; break;
   case "C" : binary = "1100"; break;
   case "D" : binary = "1101"; break;
   case "E" : binary = "1110"; break;
   case "F" : binary = "1111"; break;
 }
 return binary;
}

/*
* chang the bit(it's length = 64) into the string
* 
* return string
*/
function byteToString(byteData){
 var str="";
 for(i = 0;i<4;i++){
   var count=0;
   for(j=0;j<16;j++){        
     var pow=1;
     for(m=15;m>j;m--){
       pow*=2;
     }              
     count+=byteData[16*i+j]*pow;
   }        
   if(count != 0){
     str+=String.fromCharCode(count);
   }
 }
 return str;
}

function bt64ToHex(byteData){
 var hex = "";
 for(i = 0;i<16;i++){
   var bt = "";
   for(j=0;j<4;j++){    
     bt += byteData[i*4+j];
   }    
   hex+=bt4ToHex(bt);
 }
 return hex;
}

function hexToBt64(hex){
 var binary = "";
 for(i = 0;i<16;i++){
   binary+=hexToBt4(hex.substring(i,i+1));
 }
 return binary;
}

/*
* the 64 bit des core arithmetic
*/

function enc(dataByte,keyByte){  
 var keys = generateKeys(keyByte);    
 var ipByte   = initPermute(dataByte);  
 var ipLeft   = new Array(32);
 var ipRight  = new Array(32);
 var tempLeft = new Array(32);
 var i = 0,j = 0,k = 0,m = 0, n = 0;
 for(k = 0;k < 32;k ++){
   ipLeft[k] = ipByte[k];
   ipRight[k] = ipByte[32+k];
 }    
 for(i = 0;i < 16;i ++){
   for(j = 0;j < 32;j ++){
     tempLeft[j] = ipLeft[j];
     ipLeft[j] = ipRight[j];      
   }  
   var key = new Array(48);
   for(m = 0;m < 48;m ++){
     key[m] = keys[i][m];
   }
   var  tempRight = xor(pPermute(sBoxPermute(xor(expandPermute(ipRight),key))), tempLeft);      
   for(n = 0;n < 32;n ++){
     ipRight[n] = tempRight[n];
   }  
   
 }  
 
 
 var finalData =new Array(64);
 for(i = 0;i < 32;i ++){
   finalData[i] = ipRight[i];
   finalData[32+i] = ipLeft[i];
 }
 return finallyPermute(finalData);  
}

function dec(dataByte,keyByte){  
 var keys = generateKeys(keyByte);    
 var ipByte   = initPermute(dataByte);  
 var ipLeft   = new Array(32);
 var ipRight  = new Array(32);
 var tempLeft = new Array(32);
 var i = 0,j = 0,k = 0,m = 0, n = 0;
 for(k = 0;k < 32;k ++){
   ipLeft[k] = ipByte[k];
   ipRight[k] = ipByte[32+k];
 }  
 for(i = 15;i >= 0;i --){
   for(j = 0;j < 32;j ++){
     tempLeft[j] = ipLeft[j];
     ipLeft[j] = ipRight[j];      
   }  
   var key = new Array(48);
   for(m = 0;m < 48;m ++){
     key[m] = keys[i][m];
   }
   
   var  tempRight = xor(pPermute(sBoxPermute(xor(expandPermute(ipRight),key))), tempLeft);      
   for(n = 0;n < 32;n ++){
     ipRight[n] = tempRight[n];
   }  
 }  
 
 
 var finalData =new Array(64);
 for(i = 0;i < 32;i ++){
   finalData[i] = ipRight[i];
   finalData[32+i] = ipLeft[i];
 }
 return finallyPermute(finalData);  
}

function initPermute(originalData){
 var ipByte = new Array(64);
 for (i = 0, m = 1, n = 0; i < 4; i++, m += 2, n += 2) {
   for (j = 7, k = 0; j >= 0; j--, k++) {
     ipByte[i * 8 + k] = originalData[j * 8 + m];
     ipByte[i * 8 + k + 32] = originalData[j * 8 + n];
   }
 }    
 return ipByte;
}

function expandPermute(rightData){  
 var epByte = new Array(48);
 for (i = 0; i < 8; i++) {
   if (i == 0) {
     epByte[i * 6 + 0] = rightData[31];
   } else {
     epByte[i * 6 + 0] = rightData[i * 4 - 1];
   }
   epByte[i * 6 + 1] = rightData[i * 4 + 0];
   epByte[i * 6 + 2] = rightData[i * 4 + 1];
   epByte[i * 6 + 3] = rightData[i * 4 + 2];
   epByte[i * 6 + 4] = rightData[i * 4 + 3];
   if (i == 7) {
     epByte[i * 6 + 5] = rightData[0];
   } else {
     epByte[i * 6 + 5] = rightData[i * 4 + 4];
   }
 }      
 return epByte;
}

function xor(byteOne,byteTwo){  
 var xorByte = new Array(byteOne.length);
 for(i = 0;i < byteOne.length; i ++){      
   xorByte[i] = byteOne[i] ^ byteTwo[i];
 }  
 return xorByte;
}

function sBoxPermute(expandByte){
 
   var sBoxByte = new Array(32);
   var binary = "";
   var s1 = [
       [14, 4, 13, 1, 2, 15, 11, 8, 3, 10, 6, 12, 5, 9, 0, 7],
       [0, 15, 7, 4, 14, 2, 13, 1, 10, 6, 12, 11, 9, 5, 3, 8],
       [4, 1, 14, 8, 13, 6, 2, 11, 15, 12, 9, 7, 3, 10, 5, 0],
       [15, 12, 8, 2, 4, 9, 1, 7, 5, 11, 3, 14, 10, 0, 6, 13 ]];

       /* Table - s2 */
   var s2 = [
       [15, 1, 8, 14, 6, 11, 3, 4, 9, 7, 2, 13, 12, 0, 5, 10],
       [3, 13, 4, 7, 15, 2, 8, 14, 12, 0, 1, 10, 6, 9, 11, 5],
       [0, 14, 7, 11, 10, 4, 13, 1, 5, 8, 12, 6, 9, 3, 2, 15],
       [13, 8, 10, 1, 3, 15, 4, 2, 11, 6, 7, 12, 0, 5, 14, 9 ]];

       /* Table - s3 */
   var s3= [
       [10, 0, 9, 14, 6, 3, 15, 5, 1, 13, 12, 7, 11, 4, 2, 8],
       [13, 7, 0, 9, 3, 4, 6, 10, 2, 8, 5, 14, 12, 11, 15, 1],
       [13, 6, 4, 9, 8, 15, 3, 0, 11, 1, 2, 12, 5, 10, 14, 7],
       [1, 10, 13, 0, 6, 9, 8, 7, 4, 15, 14, 3, 11, 5, 2, 12 ]];
       /* Table - s4 */
   var s4 = [
       [7, 13, 14, 3, 0, 6, 9, 10, 1, 2, 8, 5, 11, 12, 4, 15],
       [13, 8, 11, 5, 6, 15, 0, 3, 4, 7, 2, 12, 1, 10, 14, 9],
       [10, 6, 9, 0, 12, 11, 7, 13, 15, 1, 3, 14, 5, 2, 8, 4],
       [3, 15, 0, 6, 10, 1, 13, 8, 9, 4, 5, 11, 12, 7, 2, 14 ]];

       /* Table - s5 */
   var s5 = [
       [2, 12, 4, 1, 7, 10, 11, 6, 8, 5, 3, 15, 13, 0, 14, 9],
       [14, 11, 2, 12, 4, 7, 13, 1, 5, 0, 15, 10, 3, 9, 8, 6],
       [4, 2, 1, 11, 10, 13, 7, 8, 15, 9, 12, 5, 6, 3, 0, 14],
       [11, 8, 12, 7, 1, 14, 2, 13, 6, 15, 0, 9, 10, 4, 5, 3 ]];

       /* Table - s6 */
   var s6 = [
       [12, 1, 10, 15, 9, 2, 6, 8, 0, 13, 3, 4, 14, 7, 5, 11],
       [10, 15, 4, 2, 7, 12, 9, 5, 6, 1, 13, 14, 0, 11, 3, 8],
       [9, 14, 15, 5, 2, 8, 12, 3, 7, 0, 4, 10, 1, 13, 11, 6],
       [4, 3, 2, 12, 9, 5, 15, 10, 11, 14, 1, 7, 6, 0, 8, 13 ]];

       /* Table - s7 */
   var s7 = [
       [4, 11, 2, 14, 15, 0, 8, 13, 3, 12, 9, 7, 5, 10, 6, 1],
       [13, 0, 11, 7, 4, 9, 1, 10, 14, 3, 5, 12, 2, 15, 8, 6],
       [1, 4, 11, 13, 12, 3, 7, 14, 10, 15, 6, 8, 0, 5, 9, 2],
       [6, 11, 13, 8, 1, 4, 10, 7, 9, 5, 0, 15, 14, 2, 3, 12]];

       /* Table - s8 */
   var s8 = [
       [13, 2, 8, 4, 6, 15, 11, 1, 10, 9, 3, 14, 5, 0, 12, 7],
       [1, 15, 13, 8, 10, 3, 7, 4, 12, 5, 6, 11, 0, 14, 9, 2],
       [7, 11, 4, 1, 9, 12, 14, 2, 0, 6, 10, 13, 15, 3, 5, 8],
       [2, 1, 14, 7, 4, 10, 8, 13, 15, 12, 9, 0, 3, 5, 6, 11]];
   
   for(m=0;m<8;m++){
   var i=0,j=0;
   i = expandByte[m*6+0]*2+expandByte[m*6+5];
   j = expandByte[m * 6 + 1] * 2 * 2 * 2 
     + expandByte[m * 6 + 2] * 2* 2 
     + expandByte[m * 6 + 3] * 2 
     + expandByte[m * 6 + 4];
   switch (m) {
     case 0 :
       binary = getBoxBinary(s1[i][j]);
       break;
     case 1 :
       binary = getBoxBinary(s2[i][j]);
       break;
     case 2 :
       binary = getBoxBinary(s3[i][j]);
       break;
     case 3 :
       binary = getBoxBinary(s4[i][j]);
       break;
     case 4 :
       binary = getBoxBinary(s5[i][j]);
       break;
     case 5 :
       binary = getBoxBinary(s6[i][j]);
       break;
     case 6 :
       binary = getBoxBinary(s7[i][j]);
       break;
     case 7 :
       binary = getBoxBinary(s8[i][j]);
       break;
   }      
   sBoxByte[m*4+0] = parseInt(binary.substring(0,1));
   sBoxByte[m*4+1] = parseInt(binary.substring(1,2));
   sBoxByte[m*4+2] = parseInt(binary.substring(2,3));
   sBoxByte[m*4+3] = parseInt(binary.substring(3,4));
 }
 return sBoxByte;
}

function pPermute(sBoxByte){
 var pBoxPermute = new Array(32);
 pBoxPermute[ 0] = sBoxByte[15]; 
 pBoxPermute[ 1] = sBoxByte[ 6]; 
 pBoxPermute[ 2] = sBoxByte[19]; 
 pBoxPermute[ 3] = sBoxByte[20]; 
 pBoxPermute[ 4] = sBoxByte[28]; 
 pBoxPermute[ 5] = sBoxByte[11]; 
 pBoxPermute[ 6] = sBoxByte[27]; 
 pBoxPermute[ 7] = sBoxByte[16]; 
 pBoxPermute[ 8] = sBoxByte[ 0]; 
 pBoxPermute[ 9] = sBoxByte[14]; 
 pBoxPermute[10] = sBoxByte[22]; 
 pBoxPermute[11] = sBoxByte[25]; 
 pBoxPermute[12] = sBoxByte[ 4]; 
 pBoxPermute[13] = sBoxByte[17]; 
 pBoxPermute[14] = sBoxByte[30]; 
 pBoxPermute[15] = sBoxByte[ 9]; 
 pBoxPermute[16] = sBoxByte[ 1]; 
 pBoxPermute[17] = sBoxByte[ 7]; 
 pBoxPermute[18] = sBoxByte[23]; 
 pBoxPermute[19] = sBoxByte[13]; 
 pBoxPermute[20] = sBoxByte[31]; 
 pBoxPermute[21] = sBoxByte[26]; 
 pBoxPermute[22] = sBoxByte[ 2]; 
 pBoxPermute[23] = sBoxByte[ 8]; 
 pBoxPermute[24] = sBoxByte[18]; 
 pBoxPermute[25] = sBoxByte[12]; 
 pBoxPermute[26] = sBoxByte[29]; 
 pBoxPermute[27] = sBoxByte[ 5]; 
 pBoxPermute[28] = sBoxByte[21]; 
 pBoxPermute[29] = sBoxByte[10]; 
 pBoxPermute[30] = sBoxByte[ 3]; 
 pBoxPermute[31] = sBoxByte[24];    
 return pBoxPermute;
}

function finallyPermute(endByte){    
 var fpByte = new Array(64);  
 fpByte[ 0] = endByte[39]; 
 fpByte[ 1] = endByte[ 7]; 
 fpByte[ 2] = endByte[47]; 
 fpByte[ 3] = endByte[15]; 
 fpByte[ 4] = endByte[55]; 
 fpByte[ 5] = endByte[23]; 
 fpByte[ 6] = endByte[63]; 
 fpByte[ 7] = endByte[31]; 
 fpByte[ 8] = endByte[38]; 
 fpByte[ 9] = endByte[ 6]; 
 fpByte[10] = endByte[46]; 
 fpByte[11] = endByte[14]; 
 fpByte[12] = endByte[54]; 
 fpByte[13] = endByte[22]; 
 fpByte[14] = endByte[62]; 
 fpByte[15] = endByte[30]; 
 fpByte[16] = endByte[37]; 
 fpByte[17] = endByte[ 5]; 
 fpByte[18] = endByte[45]; 
 fpByte[19] = endByte[13]; 
 fpByte[20] = endByte[53]; 
 fpByte[21] = endByte[21]; 
 fpByte[22] = endByte[61]; 
 fpByte[23] = endByte[29]; 
 fpByte[24] = endByte[36]; 
 fpByte[25] = endByte[ 4]; 
 fpByte[26] = endByte[44]; 
 fpByte[27] = endByte[12]; 
 fpByte[28] = endByte[52]; 
 fpByte[29] = endByte[20]; 
 fpByte[30] = endByte[60]; 
 fpByte[31] = endByte[28]; 
 fpByte[32] = endByte[35]; 
 fpByte[33] = endByte[ 3]; 
 fpByte[34] = endByte[43]; 
 fpByte[35] = endByte[11]; 
 fpByte[36] = endByte[51]; 
 fpByte[37] = endByte[19]; 
 fpByte[38] = endByte[59]; 
 fpByte[39] = endByte[27]; 
 fpByte[40] = endByte[34]; 
 fpByte[41] = endByte[ 2]; 
 fpByte[42] = endByte[42]; 
 fpByte[43] = endByte[10]; 
 fpByte[44] = endByte[50]; 
 fpByte[45] = endByte[18]; 
 fpByte[46] = endByte[58]; 
 fpByte[47] = endByte[26]; 
 fpByte[48] = endByte[33]; 
 fpByte[49] = endByte[ 1]; 
 fpByte[50] = endByte[41]; 
 fpByte[51] = endByte[ 9]; 
 fpByte[52] = endByte[49]; 
 fpByte[53] = endByte[17]; 
 fpByte[54] = endByte[57]; 
 fpByte[55] = endByte[25]; 
 fpByte[56] = endByte[32]; 
 fpByte[57] = endByte[ 0]; 
 fpByte[58] = endByte[40]; 
 fpByte[59] = endByte[ 8]; 
 fpByte[60] = endByte[48]; 
 fpByte[61] = endByte[16]; 
 fpByte[62] = endByte[56]; 
 fpByte[63] = endByte[24];
 return fpByte;
}

function getBoxBinary(i) {
 var binary = "";
 switch (i) {
   case 0 :binary = "0000";break;
   case 1 :binary = "0001";break;
   case 2 :binary = "0010";break;
   case 3 :binary = "0011";break;
   case 4 :binary = "0100";break;
   case 5 :binary = "0101";break;
   case 6 :binary = "0110";break;
   case 7 :binary = "0111";break;
   case 8 :binary = "1000";break;
   case 9 :binary = "1001";break;
   case 10 :binary = "1010";break;
   case 11 :binary = "1011";break;
   case 12 :binary = "1100";break;
   case 13 :binary = "1101";break;
   case 14 :binary = "1110";break;
   case 15 :binary = "1111";break;
 }
 return binary;
}
/*
* generate 16 keys for xor
*
*/
function generateKeys(keyByte){    
 var key   = new Array(56);
 var keys = new Array();  
 
 keys[ 0] = new Array();
 keys[ 1] = new Array();
 keys[ 2] = new Array();
 keys[ 3] = new Array();
 keys[ 4] = new Array();
 keys[ 5] = new Array();
 keys[ 6] = new Array();
 keys[ 7] = new Array();
 keys[ 8] = new Array();
 keys[ 9] = new Array();
 keys[10] = new Array();
 keys[11] = new Array();
 keys[12] = new Array();
 keys[13] = new Array();
 keys[14] = new Array();
 keys[15] = new Array();  
 var loop = [1,1,2,2,2,2,2,2,1,2,2,2,2,2,2,1];

 for(i=0;i<7;i++){
   for(j=0,k=7;j<8;j++,k--){
     key[i*8+j]=keyByte[8*k+i];
   }
 }    
 
 var i = 0;
 for(i = 0;i < 16;i ++){
   var tempLeft=0;
   var tempRight=0;
   for(j = 0; j < loop[i];j ++){          
     tempLeft = key[0];
     tempRight = key[28];
     for(k = 0;k < 27 ;k ++){
       key[k] = key[k+1];
       key[28+k] = key[29+k];
     }  
     key[27]=tempLeft;
     key[55]=tempRight;
   }
   var tempKey = new Array(48);
   tempKey[ 0] = key[13];
   tempKey[ 1] = key[16];
   tempKey[ 2] = key[10];
   tempKey[ 3] = key[23];
   tempKey[ 4] = key[ 0];
   tempKey[ 5] = key[ 4];
   tempKey[ 6] = key[ 2];
   tempKey[ 7] = key[27];
   tempKey[ 8] = key[14];
   tempKey[ 9] = key[ 5];
   tempKey[10] = key[20];
   tempKey[11] = key[ 9];
   tempKey[12] = key[22];
   tempKey[13] = key[18];
   tempKey[14] = key[11];
   tempKey[15] = key[ 3];
   tempKey[16] = key[25];
   tempKey[17] = key[ 7];
   tempKey[18] = key[15];
   tempKey[19] = key[ 6];
   tempKey[20] = key[26];
   tempKey[21] = key[19];
   tempKey[22] = key[12];
   tempKey[23] = key[ 1];
   tempKey[24] = key[40];
   tempKey[25] = key[51];
   tempKey[26] = key[30];
   tempKey[27] = key[36];
   tempKey[28] = key[46];
   tempKey[29] = key[54];
   tempKey[30] = key[29];
   tempKey[31] = key[39];
   tempKey[32] = key[50];
   tempKey[33] = key[44];
   tempKey[34] = key[32];
   tempKey[35] = key[47];
   tempKey[36] = key[43];
   tempKey[37] = key[48];
   tempKey[38] = key[38];
   tempKey[39] = key[55];
   tempKey[40] = key[33];
   tempKey[41] = key[52];
   tempKey[42] = key[45];
   tempKey[43] = key[41];
   tempKey[44] = key[49];
   tempKey[45] = key[35];
   tempKey[46] = key[28];
   tempKey[47] = key[31];
   switch(i){
     case 0: for(m=0;m < 48 ;m++){ keys[ 0][m] = tempKey[m]; } break;
     case 1: for(m=0;m < 48 ;m++){ keys[ 1][m] = tempKey[m]; } break;
     case 2: for(m=0;m < 48 ;m++){ keys[ 2][m] = tempKey[m]; } break;
     case 3: for(m=0;m < 48 ;m++){ keys[ 3][m] = tempKey[m]; } break;
     case 4: for(m=0;m < 48 ;m++){ keys[ 4][m] = tempKey[m]; } break;
     case 5: for(m=0;m < 48 ;m++){ keys[ 5][m] = tempKey[m]; } break;
     case 6: for(m=0;m < 48 ;m++){ keys[ 6][m] = tempKey[m]; } break;
     case 7: for(m=0;m < 48 ;m++){ keys[ 7][m] = tempKey[m]; } break;
     case 8: for(m=0;m < 48 ;m++){ keys[ 8][m] = tempKey[m]; } break;
     case 9: for(m=0;m < 48 ;m++){ keys[ 9][m] = tempKey[m]; } break;
     case 10: for(m=0;m < 48 ;m++){ keys[10][m] = tempKey[m]; } break;
     case 11: for(m=0;m < 48 ;m++){ keys[11][m] = tempKey[m]; } break;
     case 12: for(m=0;m < 48 ;m++){ keys[12][m] = tempKey[m]; } break;
     case 13: for(m=0;m < 48 ;m++){ keys[13][m] = tempKey[m]; } break;
     case 14: for(m=0;m < 48 ;m++){ keys[14][m] = tempKey[m]; } break;
     case 15: for(m=0;m < 48 ;m++){ keys[15][m] = tempKey[m]; } break;
   }
 }
 return keys;  
}
//end-------------------------------------------------------------------------------------------------------------
/*
function test() {
 
 var msg = "abcdefgh";
 var bt = strToBt(msg);
 
 var key = "12345678";
 var keyB = strToBt(key);
   
 var encByte = enc(bt,keyB);
     
 var enchex  = bt64ToHex(encByte);  
 endata.value=enchex;
 
 var encStr = hexToBt64(enchex);
 alert("encStr="+encStr);
 var eByte = new Array();
 for(m=0;m<encStr.length;m++){
   eByte[m] = parseInt(encStr.substring(m,m+1));
 }
 var decbyte= dec(eByte,keyB)
 var decmsg= byteToString(decbyte);
 alert("decbyte="+decbyte);
 alert("decmsg="+decmsg);  
}*/
```


DES.java(对应的Java代码):
```
package com.blog.springboot.utils;

import java.util.ArrayList;
import java.util.List;
 
/**
 * DES加密/解密
 * 
 * @Copyright Copyright (c) 2015
 * @author liuyazhuang
 * @see DESCore
 */
public class Des {
    public Des() {
    }
    public static void main(String[] args) {
        Des desObj = new Des();
        String key1 = "1";
        String key2 = "2";
        String key3 = "3";
        String data = "admin";
        String str = desObj.strEnc(data, key1, key2, key3);
        System.out.println(str);
        String dec = desObj.strDec(str, key1, key2, key3);
        System.out.println(dec);
    }
 
    /**
     * DES加密/解密
     * 
     * @Copyright Copyright (c) 2015
     * @author liuyazhuang
     * @see DESCore
     */
 
    /*
     * encrypt the string to string made up of hex return the encrypted string
     */
    public String strEnc(String data, String firstKey, String secondKey,
            String thirdKey) {
 
        int leng = data.length();
        String encData = "";
        List firstKeyBt = null, secondKeyBt = null, thirdKeyBt = null;
        int firstLength = 0, secondLength = 0, thirdLength = 0;
        if (firstKey != null && firstKey != "") {
            firstKeyBt = getKeyBytes(firstKey);
            firstLength = firstKeyBt.size();
        }
        if (secondKey != null && secondKey != "") {
            secondKeyBt = getKeyBytes(secondKey);
            secondLength = secondKeyBt.size();
        }
        if (thirdKey != null && thirdKey != "") {
            thirdKeyBt = getKeyBytes(thirdKey);
            thirdLength = thirdKeyBt.size();
        }
 
        if (leng > 0) {
            if (leng < 4) {
                int[] bt = strToBt(data);
                int[] encByte = null;
                if (firstKey != null && firstKey != "" && secondKey != null
                        && secondKey != "" && thirdKey != null
                        && thirdKey != "") {
                    int[] tempBt;
                    int x, y, z;
                    tempBt = bt;
                    for (x = 0; x < firstLength; x++) {
                        tempBt = enc(tempBt, (int[]) firstKeyBt.get(x));
                    }
                    for (y = 0; y < secondLength; y++) {
                        tempBt = enc(tempBt, (int[]) secondKeyBt.get(y));
                    }
                    for (z = 0; z < thirdLength; z++) {
                        tempBt = enc(tempBt, (int[]) thirdKeyBt.get(z));
                    }
                    encByte = tempBt;
                } else {
                    if (firstKey != null && firstKey != "" && secondKey != null
                            && secondKey != "") {
                        int[] tempBt;
                        int x, y;
                        tempBt = bt;
                        for (x = 0; x < firstLength; x++) {
                            tempBt = enc(tempBt, (int[]) firstKeyBt.get(x));
                        }
                        for (y = 0; y < secondLength; y++) {
                            tempBt = enc(tempBt, (int[]) secondKeyBt.get(y));
                        }
                        encByte = tempBt;
                    } else {
                        if (firstKey != null && firstKey != "") {
                            int[] tempBt;
                            int x = 0;
                            tempBt = bt;
                            for (x = 0; x < firstLength; x++) {
                                tempBt = enc(tempBt, (int[]) firstKeyBt.get(x));
                            }
                            encByte = tempBt;
                        }
                    }
                }
                encData = bt64ToHex(encByte);
            } else {
                int iterator = (leng / 4);
                int remainder = leng % 4;
                int i = 0;
                for (i = 0; i < iterator; i++) {
                    String tempData = data.substring(i * 4 + 0, i * 4 + 4);
                    int[] tempByte = strToBt(tempData);
                    int[] encByte = null;
                    if (firstKey != null && firstKey != "" && secondKey != null
                            && secondKey != "" && thirdKey != null
                            && thirdKey != "") {
                        int[] tempBt;
                        int x, y, z;
                        tempBt = tempByte;
                        for (x = 0; x < firstLength; x++) {
                            tempBt = enc(tempBt, (int[]) firstKeyBt.get(x));
                        }
                        for (y = 0; y < secondLength; y++) {
                            tempBt = enc(tempBt, (int[]) secondKeyBt.get(y));
                        }
                        for (z = 0; z < thirdLength; z++) {
                            tempBt = enc(tempBt, (int[]) thirdKeyBt.get(z));
                        }
                        encByte = tempBt;
                    } else {
                        if (firstKey != null && firstKey != ""
                                && secondKey != null && secondKey != "") {
                            int[] tempBt;
                            int x, y;
                            tempBt = tempByte;
                            for (x = 0; x < firstLength; x++) {
                                tempBt = enc(tempBt, (int[]) firstKeyBt.get(x));
                            }
                            for (y = 0; y < secondLength; y++) {
                                tempBt = enc(tempBt, (int[]) secondKeyBt.get(y));
                            }
                            encByte = tempBt;
                        } else {
                            if (firstKey != null && firstKey != "") {
                                int[] tempBt;
                                int x;
                                tempBt = tempByte;
                                for (x = 0; x < firstLength; x++) {
                                    tempBt = enc(tempBt, (int[]) firstKeyBt
                                            .get(x));
                                }
                                encByte = tempBt;
                            }
                        }
                    }
                    encData += bt64ToHex(encByte);
                }
                if (remainder > 0) {
                    String remainderData = data.substring(iterator * 4 + 0,
                            leng);
                    int[] tempByte = strToBt(remainderData);
                    int[] encByte = null;
                    if (firstKey != null && firstKey != "" && secondKey != null
                            && secondKey != "" && thirdKey != null
                            && thirdKey != "") {
                        int[] tempBt;
                        int x, y, z;
                        tempBt = tempByte;
                        for (x = 0; x < firstLength; x++) {
                            tempBt = enc(tempBt, (int[]) firstKeyBt.get(x));
                        }
                        for (y = 0; y < secondLength; y++) {
                            tempBt = enc(tempBt, (int[]) secondKeyBt.get(y));
                        }
                        for (z = 0; z < thirdLength; z++) {
                            tempBt = enc(tempBt, (int[]) thirdKeyBt.get(z));
                        }
                        encByte = tempBt;
                    } else {
                        if (firstKey != null && firstKey != ""
                                && secondKey != null && secondKey != "") {
                            int[] tempBt;
                            int x, y;
                            tempBt = tempByte;
                            for (x = 0; x < firstLength; x++) {
                                tempBt = enc(tempBt, (int[]) firstKeyBt.get(x));
                            }
                            for (y = 0; y < secondLength; y++) {
                                tempBt = enc(tempBt, (int[]) secondKeyBt.get(y));
                            }
                            encByte = tempBt;
                        } else {
                            if (firstKey != null && firstKey != "") {
                                int[] tempBt;
                                int x;
                                tempBt = tempByte;
                                for (x = 0; x < firstLength; x++) {
                                    tempBt = enc(tempBt, (int[]) firstKeyBt
                                            .get(x));
                                }
                                encByte = tempBt;
                            }
                        }
                    }
                    encData += bt64ToHex(encByte);
                }
            }
        }
        return encData;
    }
 
    /*
     * decrypt the encrypted string to the original string
     * return the original string
     */
    public String strDec(String data, String firstKey, String secondKey,
            String thirdKey) {
        int leng = data.length();
        String decStr = "";
        List firstKeyBt = null, secondKeyBt = null, thirdKeyBt = null;
        int firstLength = 0, secondLength = 0, thirdLength = 0;
        if (firstKey != null && firstKey != "") {
            firstKeyBt = getKeyBytes(firstKey);
            firstLength = firstKeyBt.size();
        }
        if (secondKey != null && secondKey != "") {
            secondKeyBt = getKeyBytes(secondKey);
            secondLength = secondKeyBt.size();
        }
        if (thirdKey != null && thirdKey != "") {
            thirdKeyBt = getKeyBytes(thirdKey);
            thirdLength = thirdKeyBt.size();
        }
 
        int iterator = leng / 16;
        int i = 0;
        for (i = 0; i < iterator; i++) {
            String tempData = data.substring(i * 16 + 0, i * 16 + 16);
            String strByte = hexToBt64(tempData);
            int[] intByte = new int[64];
            int j = 0;
            for (j = 0; j < 64; j++) {
                intByte[j] = Integer.parseInt(strByte.substring(j, j + 1));
            }
            int[] decByte = null;
            if (firstKey != null && firstKey != "" && secondKey != null
                    && secondKey != "" && thirdKey != null && thirdKey != "") {
                int[] tempBt;
                int x, y, z;
                tempBt = intByte;
                for (x = thirdLength - 1; x >= 0; x--) {
                    tempBt = dec(tempBt, (int[]) thirdKeyBt.get(x));
                }
                for (y = secondLength - 1; y >= 0; y--) {
                    tempBt = dec(tempBt, (int[]) secondKeyBt.get(y));
                }
                for (z = firstLength - 1; z >= 0; z--) {
                    tempBt = dec(tempBt, (int[]) firstKeyBt.get(z));
                }
                decByte = tempBt;
            } else {
                if (firstKey != null && firstKey != "" && secondKey != null
                        && secondKey != "") {
                    int[] tempBt;
                    int x, y, z;
                    tempBt = intByte;
                    for (x = secondLength - 1; x >= 0; x--) {
                        tempBt = dec(tempBt, (int[]) secondKeyBt.get(x));
                    }
                    for (y = firstLength - 1; y >= 0; y--) {
                        tempBt = dec(tempBt, (int[]) firstKeyBt.get(y));
                    }
                    decByte = tempBt;
                } else {
                    if (firstKey != null && firstKey != "") {
                        int[] tempBt;
                        int x, y, z;
                        tempBt = intByte;
                        for (x = firstLength - 1; x >= 0; x--) {
                            tempBt = dec(tempBt, (int[]) firstKeyBt.get(x));
                        }
                        decByte = tempBt;
                    }
                }
            }
            decStr += byteToString(decByte);
        }
        return decStr;
    }
 
    /*
     * chang the string into the bit array
     * 
     * return bit array(it's length % 64 = 0)
     */
    public List getKeyBytes(String key) {
        List keyBytes = new ArrayList();
        int leng = key.length();
        int iterator = (leng / 4);
        int remainder = leng % 4;
        int i = 0;
        for (i = 0; i < iterator; i++) {
            keyBytes.add(i, strToBt(key.substring(i * 4 + 0, i * 4 + 4)));
        }
        if (remainder > 0) {
            // keyBytes[i] = strToBt(key.substring(i*4+0,leng));
            keyBytes.add(i, strToBt(key.substring(i * 4 + 0, leng)));
        }
        return keyBytes;
    }
 
    /*
     * chang the string(it's length <= 4) into the bit array
     * 
     * return bit array(it's length = 64)
     */
    public int[] strToBt(String str) {
        int leng = str.length();
        int[] bt = new int[64];
        if (leng < 4) {
            int i = 0, j = 0, p = 0, q = 0;
            for (i = 0; i < leng; i++) {
                int k = str.charAt(i);
                for (j = 0; j < 16; j++) {
                    int pow = 1, m = 0;
                    for (m = 15; m > j; m--) {
                        pow *= 2;
                    }
                    // bt.set(16*i+j,""+(k/pow)%2));
                    bt[16 * i + j] = (k / pow) % 2;
                }
            }
            for (p = leng; p < 4; p++) {
                int k = 0;
                for (q = 0; q < 16; q++) {
                    int pow = 1, m = 0;
                    for (m = 15; m > q; m--) {
                        pow *= 2;
                    }
                    // bt[16*p+q]=parseInt(k/pow)%2;
                    // bt.add(16*p+q,""+((k/pow)%2));
                    bt[16 * p + q] = (k / pow) % 2;
                }
            }
        } else {
            for (int i = 0; i < 4; i++) {
                int k = str.charAt(i);
                for (int j = 0; j < 16; j++) {
                    int pow = 1;
                    for (int m = 15; m > j; m--) {
                        pow *= 2;
                    }
                    // bt[16*i+j]=parseInt(k/pow)%2;
                    // bt.add(16*i+j,""+((k/pow)%2));
                    bt[16 * i + j] = (k / pow) % 2;
                }
            }
        }
        return bt;
    }
 
    /*
     * chang the bit(it's length = 4) into the hex
     * 
     * return hex
     */
    public String bt4ToHex(String binary) {
        String hex = "";
        if (binary.equalsIgnoreCase("0000")) {
            hex = "0";
        } else if (binary.equalsIgnoreCase("0001")) {
            hex = "1";
        } else if (binary.equalsIgnoreCase("0010")) {
            hex = "2";
        } else if (binary.equalsIgnoreCase("0011")) {
            hex = "3";
        } else if (binary.equalsIgnoreCase("0100")) {
            hex = "4";
        } else if (binary.equalsIgnoreCase("0101")) {
            hex = "5";
        } else if (binary.equalsIgnoreCase("0110")) {
            hex = "6";
        } else if (binary.equalsIgnoreCase("0111")) {
            hex = "7";
        } else if (binary.equalsIgnoreCase("1000")) {
            hex = "8";
        } else if (binary.equalsIgnoreCase("1001")) {
            hex = "9";
        } else if (binary.equalsIgnoreCase("1010")) {
            hex = "A";
        } else if (binary.equalsIgnoreCase("1011")) {
            hex = "B";
        } else if (binary.equalsIgnoreCase("1100")) {
            hex = "C";
        } else if (binary.equalsIgnoreCase("1101")) {
            hex = "D";
        } else if (binary.equalsIgnoreCase("1110")) {
            hex = "E";
        } else if (binary.equalsIgnoreCase("1111")) {
            hex = "F";
        }
 
        return hex;
    }
 
    /*
     * chang the hex into the bit(it's length = 4)
     * 
     * return the bit(it's length = 4)
     */
    public String hexToBt4(String hex) {
        String binary = "";
        if (hex.equalsIgnoreCase("0")) {
            binary = "0000";
        } else if (hex.equalsIgnoreCase("1")) {
            binary = "0001";
        }
        if (hex.equalsIgnoreCase("2")) {
            binary = "0010";
        }
        if (hex.equalsIgnoreCase("3")) {
            binary = "0011";
        }
        if (hex.equalsIgnoreCase("4")) {
            binary = "0100";
        }
        if (hex.equalsIgnoreCase("5")) {
            binary = "0101";
        }
        if (hex.equalsIgnoreCase("6")) {
            binary = "0110";
        }
        if (hex.equalsIgnoreCase("7")) {
            binary = "0111";
        }
        if (hex.equalsIgnoreCase("8")) {
            binary = "1000";
        }
        if (hex.equalsIgnoreCase("9")) {
            binary = "1001";
        }
        if (hex.equalsIgnoreCase("A")) {
            binary = "1010";
        }
        if (hex.equalsIgnoreCase("B")) {
            binary = "1011";
        }
        if (hex.equalsIgnoreCase("C")) {
            binary = "1100";
        }
        if (hex.equalsIgnoreCase("D")) {
            binary = "1101";
        }
        if (hex.equalsIgnoreCase("E")) {
            binary = "1110";
        }
        if (hex.equalsIgnoreCase("F")) {
            binary = "1111";
        }
        return binary;
    }
 
    /*
     * chang the bit(it's length = 64) into the string
     * 
     * return string
     */
    public String byteToString(int[] byteData) {
        String str = "";
        for (int i = 0; i < 4; i++) {
            int count = 0;
            for (int j = 0; j < 16; j++) {
                int pow = 1;
                for (int m = 15; m > j; m--) {
                    pow *= 2;
                }
                count += byteData[16 * i + j] * pow;
            }
            if (count != 0) {
                str += "" + (char) (count);
            }
        }
        return str;
    }
 
    public String bt64ToHex(int[] byteData) {
        String hex = "";
        for (int i = 0; i < 16; i++) {
            String bt = "";
            for (int j = 0; j < 4; j++) {
                bt += byteData[i * 4 + j];
            }
            hex += bt4ToHex(bt);
        }
        return hex;
    }
 
    public String hexToBt64(String hex) {
        String binary = "";
        for (int i = 0; i < 16; i++) {
            binary += hexToBt4(hex.substring(i, i + 1));
        }
        return binary;
    }
 
    /*
     * the 64 bit des core arithmetic
     */
 
    public int[] enc(int[] dataByte, int[] keyByte) {
        int[][] keys = generateKeys(keyByte);
        int[] ipByte = initPermute(dataByte);
        int[] ipLeft = new int[32];
        int[] ipRight = new int[32];
        int[] tempLeft = new int[32];
        int i = 0, j = 0, k = 0, m = 0, n = 0;
        for (k = 0; k < 32; k++) {
            ipLeft[k] = ipByte[k];
            ipRight[k] = ipByte[32 + k];
        }
        for (i = 0; i < 16; i++) {
            for (j = 0; j < 32; j++) {
                tempLeft[j] = ipLeft[j];
                ipLeft[j] = ipRight[j];
            }
            int[] key = new int[48];
            for (m = 0; m < 48; m++) {
                key[m] = keys[i][m];
            }
            int[] tempRight = xor(pPermute(sBoxPermute(xor(
                    expandPermute(ipRight), key))), tempLeft);
            for (n = 0; n < 32; n++) {
                ipRight[n] = tempRight[n];
            }
 
        }
 
        int[] finalData = new int[64];
        for (i = 0; i < 32; i++) {
            finalData[i] = ipRight[i];
            finalData[32 + i] = ipLeft[i];
        }
        return finallyPermute(finalData);
    }
 
    public int[] dec(int[] dataByte, int[] keyByte) {
        int[][] keys = generateKeys(keyByte);
        int[] ipByte = initPermute(dataByte);
        int[] ipLeft = new int[32];
        int[] ipRight = new int[32];
        int[] tempLeft = new int[32];
        int i = 0, j = 0, k = 0, m = 0, n = 0;
        for (k = 0; k < 32; k++) {
            ipLeft[k] = ipByte[k];
            ipRight[k] = ipByte[32 + k];
        }
        for (i = 15; i >= 0; i--) {
            for (j = 0; j < 32; j++) {
                tempLeft[j] = ipLeft[j];
                ipLeft[j] = ipRight[j];
            }
            int[] key = new int[48];
            for (m = 0; m < 48; m++) {
                key[m] = keys[i][m];
            }
 
            int[] tempRight = xor(pPermute(sBoxPermute(xor(
                    expandPermute(ipRight), key))), tempLeft);
            for (n = 0; n < 32; n++) {
                ipRight[n] = tempRight[n];
            }
        }
 
        int[] finalData = new int[64];
        for (i = 0; i < 32; i++) {
            finalData[i] = ipRight[i];
            finalData[32 + i] = ipLeft[i];
        }
        return finallyPermute(finalData);
    }
 
    public int[] initPermute(int[] originalData) {
        int[] ipByte = new int[64];
        int i = 0, m = 1, n = 0, j, k;
        for (i = 0, m = 1, n = 0; i < 4; i++, m += 2, n += 2) {
            for (j = 7, k = 0; j >= 0; j--, k++) {
                ipByte[i * 8 + k] = originalData[j * 8 + m];
                ipByte[i * 8 + k + 32] = originalData[j * 8 + n];
            }
        }
        return ipByte;
    }
 
    public int[] expandPermute(int[] rightData) {
        int[] epByte = new int[48];
        int i, j;
        for (i = 0; i < 8; i++) {
            if (i == 0) {
                epByte[i * 6 + 0] = rightData[31];
            } else {
                epByte[i * 6 + 0] = rightData[i * 4 - 1];
            }
            epByte[i * 6 + 1] = rightData[i * 4 + 0];
            epByte[i * 6 + 2] = rightData[i * 4 + 1];
            epByte[i * 6 + 3] = rightData[i * 4 + 2];
            epByte[i * 6 + 4] = rightData[i * 4 + 3];
            if (i == 7) {
                epByte[i * 6 + 5] = rightData[0];
            } else {
                epByte[i * 6 + 5] = rightData[i * 4 + 4];
            }
        }
        return epByte;
    }
 
    public int[] xor(int[] byteOne, int[] byteTwo) {
        // var xorByte = new Array(byteOne.length);
        // for(int i = 0;i < byteOne.length; i ++){
        // xorByte[i] = byteOne[i] ^ byteTwo[i];
        // }
        // return xorByte;
        int[] xorByte = new int[byteOne.length];
        for (int i = 0; i < byteOne.length; i++) {
            xorByte[i] = byteOne[i] ^ byteTwo[i];
        }
        return xorByte;
    }
 
    public int[] sBoxPermute(int[] expandByte) {
 
        // var sBoxByte = new Array(32);
        int[] sBoxByte = new int[32];
        String binary = "";
        int[][] s1 = {
                { 14, 4, 13, 1, 2, 15, 11, 8, 3, 10, 6, 12, 5, 9, 0, 7 },
                { 0, 15, 7, 4, 14, 2, 13, 1, 10, 6, 12, 11, 9, 5, 3, 8 },
                { 4, 1, 14, 8, 13, 6, 2, 11, 15, 12, 9, 7, 3, 10, 5, 0 },
                { 15, 12, 8, 2, 4, 9, 1, 7, 5, 11, 3, 14, 10, 0, 6, 13 } };
 
        /* Table - s2 */
        int[][] s2 = {
                { 15, 1, 8, 14, 6, 11, 3, 4, 9, 7, 2, 13, 12, 0, 5, 10 },
                { 3, 13, 4, 7, 15, 2, 8, 14, 12, 0, 1, 10, 6, 9, 11, 5 },
                { 0, 14, 7, 11, 10, 4, 13, 1, 5, 8, 12, 6, 9, 3, 2, 15 },
                { 13, 8, 10, 1, 3, 15, 4, 2, 11, 6, 7, 12, 0, 5, 14, 9 } };
 
        /* Table - s3 */
        int[][] s3 = {
                { 10, 0, 9, 14, 6, 3, 15, 5, 1, 13, 12, 7, 11, 4, 2, 8 },
                { 13, 7, 0, 9, 3, 4, 6, 10, 2, 8, 5, 14, 12, 11, 15, 1 },
                { 13, 6, 4, 9, 8, 15, 3, 0, 11, 1, 2, 12, 5, 10, 14, 7 },
                { 1, 10, 13, 0, 6, 9, 8, 7, 4, 15, 14, 3, 11, 5, 2, 12 } };
        /* Table - s4 */
        int[][] s4 = {
                { 7, 13, 14, 3, 0, 6, 9, 10, 1, 2, 8, 5, 11, 12, 4, 15 },
                { 13, 8, 11, 5, 6, 15, 0, 3, 4, 7, 2, 12, 1, 10, 14, 9 },
                { 10, 6, 9, 0, 12, 11, 7, 13, 15, 1, 3, 14, 5, 2, 8, 4 },
                { 3, 15, 0, 6, 10, 1, 13, 8, 9, 4, 5, 11, 12, 7, 2, 14 } };
 
        /* Table - s5 */
        int[][] s5 = {
                { 2, 12, 4, 1, 7, 10, 11, 6, 8, 5, 3, 15, 13, 0, 14, 9 },
                { 14, 11, 2, 12, 4, 7, 13, 1, 5, 0, 15, 10, 3, 9, 8, 6 },
                { 4, 2, 1, 11, 10, 13, 7, 8, 15, 9, 12, 5, 6, 3, 0, 14 },
                { 11, 8, 12, 7, 1, 14, 2, 13, 6, 15, 0, 9, 10, 4, 5, 3 } };
 
        /* Table - s6 */
        int[][] s6 = {
                { 12, 1, 10, 15, 9, 2, 6, 8, 0, 13, 3, 4, 14, 7, 5, 11 },
                { 10, 15, 4, 2, 7, 12, 9, 5, 6, 1, 13, 14, 0, 11, 3, 8 },
                { 9, 14, 15, 5, 2, 8, 12, 3, 7, 0, 4, 10, 1, 13, 11, 6 },
                { 4, 3, 2, 12, 9, 5, 15, 10, 11, 14, 1, 7, 6, 0, 8, 13 } };
 
        /* Table - s7 */
        int[][] s7 = {
                { 4, 11, 2, 14, 15, 0, 8, 13, 3, 12, 9, 7, 5, 10, 6, 1 },
                { 13, 0, 11, 7, 4, 9, 1, 10, 14, 3, 5, 12, 2, 15, 8, 6 },
                { 1, 4, 11, 13, 12, 3, 7, 14, 10, 15, 6, 8, 0, 5, 9, 2 },
                { 6, 11, 13, 8, 1, 4, 10, 7, 9, 5, 0, 15, 14, 2, 3, 12 } };
 
        /* Table - s8 */
        int[][] s8 = {
                { 13, 2, 8, 4, 6, 15, 11, 1, 10, 9, 3, 14, 5, 0, 12, 7 },
                { 1, 15, 13, 8, 10, 3, 7, 4, 12, 5, 6, 11, 0, 14, 9, 2 },
                { 7, 11, 4, 1, 9, 12, 14, 2, 0, 6, 10, 13, 15, 3, 5, 8 },
                { 2, 1, 14, 7, 4, 10, 8, 13, 15, 12, 9, 0, 3, 5, 6, 11 } };
 
        for (int m = 0; m < 8; m++) {
            int i = 0, j = 0;
            i = expandByte[m * 6 + 0] * 2 + expandByte[m * 6 + 5];
            j = expandByte[m * 6 + 1] * 2 * 2 * 2 + expandByte[m * 6 + 2] * 2
                    * 2 + expandByte[m * 6 + 3] * 2 + expandByte[m * 6 + 4];
            switch (m) {
            case 0:
                binary = getBoxBinary(s1[i][j]);
                break;
            case 1:
                binary = getBoxBinary(s2[i][j]);
                break;
            case 2:
                binary = getBoxBinary(s3[i][j]);
                break;
            case 3:
                binary = getBoxBinary(s4[i][j]);
                break;
            case 4:
                binary = getBoxBinary(s5[i][j]);
                break;
            case 5:
                binary = getBoxBinary(s6[i][j]);
                break;
            case 6:
                binary = getBoxBinary(s7[i][j]);
                break;
            case 7:
                binary = getBoxBinary(s8[i][j]);
                break;
            }
            sBoxByte[m * 4 + 0] = Integer.parseInt(binary.substring(0, 1));
            sBoxByte[m * 4 + 1] = Integer.parseInt(binary.substring(1, 2));
            sBoxByte[m * 4 + 2] = Integer.parseInt(binary.substring(2, 3));
            sBoxByte[m * 4 + 3] = Integer.parseInt(binary.substring(3, 4));
        }
        return sBoxByte;
    }
 
    public int[] pPermute(int[] sBoxByte) {
        int[] pBoxPermute = new int[32];
        pBoxPermute[0] = sBoxByte[15];
        pBoxPermute[1] = sBoxByte[6];
        pBoxPermute[2] = sBoxByte[19];
        pBoxPermute[3] = sBoxByte[20];
        pBoxPermute[4] = sBoxByte[28];
        pBoxPermute[5] = sBoxByte[11];
        pBoxPermute[6] = sBoxByte[27];
        pBoxPermute[7] = sBoxByte[16];
        pBoxPermute[8] = sBoxByte[0];
        pBoxPermute[9] = sBoxByte[14];
        pBoxPermute[10] = sBoxByte[22];
        pBoxPermute[11] = sBoxByte[25];
        pBoxPermute[12] = sBoxByte[4];
        pBoxPermute[13] = sBoxByte[17];
        pBoxPermute[14] = sBoxByte[30];
        pBoxPermute[15] = sBoxByte[9];
        pBoxPermute[16] = sBoxByte[1];
        pBoxPermute[17] = sBoxByte[7];
        pBoxPermute[18] = sBoxByte[23];
        pBoxPermute[19] = sBoxByte[13];
        pBoxPermute[20] = sBoxByte[31];
        pBoxPermute[21] = sBoxByte[26];
        pBoxPermute[22] = sBoxByte[2];
        pBoxPermute[23] = sBoxByte[8];
        pBoxPermute[24] = sBoxByte[18];
        pBoxPermute[25] = sBoxByte[12];
        pBoxPermute[26] = sBoxByte[29];
        pBoxPermute[27] = sBoxByte[5];
        pBoxPermute[28] = sBoxByte[21];
        pBoxPermute[29] = sBoxByte[10];
        pBoxPermute[30] = sBoxByte[3];
        pBoxPermute[31] = sBoxByte[24];
        return pBoxPermute;
    }
 
    public int[] finallyPermute(int[] endByte) {
        int[] fpByte = new int[64];
        fpByte[0] = endByte[39];
        fpByte[1] = endByte[7];
        fpByte[2] = endByte[47];
        fpByte[3] = endByte[15];
        fpByte[4] = endByte[55];
        fpByte[5] = endByte[23];
        fpByte[6] = endByte[63];
        fpByte[7] = endByte[31];
        fpByte[8] = endByte[38];
        fpByte[9] = endByte[6];
        fpByte[10] = endByte[46];
        fpByte[11] = endByte[14];
        fpByte[12] = endByte[54];
        fpByte[13] = endByte[22];
        fpByte[14] = endByte[62];
        fpByte[15] = endByte[30];
        fpByte[16] = endByte[37];
        fpByte[17] = endByte[5];
        fpByte[18] = endByte[45];
        fpByte[19] = endByte[13];
        fpByte[20] = endByte[53];
        fpByte[21] = endByte[21];
        fpByte[22] = endByte[61];
        fpByte[23] = endByte[29];
        fpByte[24] = endByte[36];
        fpByte[25] = endByte[4];
        fpByte[26] = endByte[44];
        fpByte[27] = endByte[12];
        fpByte[28] = endByte[52];
        fpByte[29] = endByte[20];
        fpByte[30] = endByte[60];
        fpByte[31] = endByte[28];
        fpByte[32] = endByte[35];
        fpByte[33] = endByte[3];
        fpByte[34] = endByte[43];
        fpByte[35] = endByte[11];
        fpByte[36] = endByte[51];
        fpByte[37] = endByte[19];
        fpByte[38] = endByte[59];
        fpByte[39] = endByte[27];
        fpByte[40] = endByte[34];
        fpByte[41] = endByte[2];
        fpByte[42] = endByte[42];
        fpByte[43] = endByte[10];
        fpByte[44] = endByte[50];
        fpByte[45] = endByte[18];
        fpByte[46] = endByte[58];
        fpByte[47] = endByte[26];
        fpByte[48] = endByte[33];
        fpByte[49] = endByte[1];
        fpByte[50] = endByte[41];
        fpByte[51] = endByte[9];
        fpByte[52] = endByte[49];
        fpByte[53] = endByte[17];
        fpByte[54] = endByte[57];
        fpByte[55] = endByte[25];
        fpByte[56] = endByte[32];
        fpByte[57] = endByte[0];
        fpByte[58] = endByte[40];
        fpByte[59] = endByte[8];
        fpByte[60] = endByte[48];
        fpByte[61] = endByte[16];
        fpByte[62] = endByte[56];
        fpByte[63] = endByte[24];
        return fpByte;
    }
 
    public String getBoxBinary(int i) {
        String binary = "";
        switch (i) {
        case 0:
            binary = "0000";
            break;
        case 1:
            binary = "0001";
            break;
        case 2:
            binary = "0010";
            break;
        case 3:
            binary = "0011";
            break;
        case 4:
            binary = "0100";
            break;
        case 5:
            binary = "0101";
            break;
        case 6:
            binary = "0110";
            break;
        case 7:
            binary = "0111";
            break;
        case 8:
            binary = "1000";
            break;
        case 9:
            binary = "1001";
            break;
        case 10:
            binary = "1010";
            break;
        case 11:
            binary = "1011";
            break;
        case 12:
            binary = "1100";
            break;
        case 13:
            binary = "1101";
            break;
        case 14:
            binary = "1110";
            break;
        case 15:
            binary = "1111";
            break;
        }
        return binary;
    }
 
    /*
     * generate 16 keys for xor
     * 
     */
    public int[][] generateKeys(int[] keyByte) {
        int[] key = new int[56];
        int[][] keys = new int[16][48];
 
        // keys[ 0] = new Array();
        // keys[ 1] = new Array();
        // keys[ 2] = new Array();
        // keys[ 3] = new Array();
        // keys[ 4] = new Array();
        // keys[ 5] = new Array();
        // keys[ 6] = new Array();
        // keys[ 7] = new Array();
        // keys[ 8] = new Array();
        // keys[ 9] = new Array();
        // keys[10] = new Array();
        // keys[11] = new Array();
        // keys[12] = new Array();
        // keys[13] = new Array();
        // keys[14] = new Array();
        // keys[15] = new Array();
        int[] loop = new int[] { 1, 1, 2, 2, 2, 2, 2, 2, 1, 2, 2, 2, 2, 2, 2, 1 };
 
        for (int i = 0; i < 7; i++) {
            for (int j = 0, k = 7; j < 8; j++, k--) {
                key[i * 8 + j] = keyByte[8 * k + i];
            }
        }
 
        int i = 0;
        for (i = 0; i < 16; i++) {
            int tempLeft = 0;
            int tempRight = 0;
            for (int j = 0; j < loop[i]; j++) {
                tempLeft = key[0];
                tempRight = key[28];
                for (int k = 0; k < 27; k++) {
                    key[k] = key[k + 1];
                    key[28 + k] = key[29 + k];
                }
                key[27] = tempLeft;
                key[55] = tempRight;
            }
            // var tempKey = new Array(48);
            int[] tempKey = new int[48];
            tempKey[0] = key[13];
            tempKey[1] = key[16];
            tempKey[2] = key[10];
            tempKey[3] = key[23];
            tempKey[4] = key[0];
            tempKey[5] = key[4];
            tempKey[6] = key[2];
            tempKey[7] = key[27];
            tempKey[8] = key[14];
            tempKey[9] = key[5];
            tempKey[10] = key[20];
            tempKey[11] = key[9];
            tempKey[12] = key[22];
            tempKey[13] = key[18];
            tempKey[14] = key[11];
            tempKey[15] = key[3];
            tempKey[16] = key[25];
            tempKey[17] = key[7];
            tempKey[18] = key[15];
            tempKey[19] = key[6];
            tempKey[20] = key[26];
            tempKey[21] = key[19];
            tempKey[22] = key[12];
            tempKey[23] = key[1];
            tempKey[24] = key[40];
            tempKey[25] = key[51];
            tempKey[26] = key[30];
            tempKey[27] = key[36];
            tempKey[28] = key[46];
            tempKey[29] = key[54];
            tempKey[30] = key[29];
            tempKey[31] = key[39];
            tempKey[32] = key[50];
            tempKey[33] = key[44];
            tempKey[34] = key[32];
            tempKey[35] = key[47];
            tempKey[36] = key[43];
            tempKey[37] = key[48];
            tempKey[38] = key[38];
            tempKey[39] = key[55];
            tempKey[40] = key[33];
            tempKey[41] = key[52];
            tempKey[42] = key[45];
            tempKey[43] = key[41];
            tempKey[44] = key[49];
            tempKey[45] = key[35];
            tempKey[46] = key[28];
            tempKey[47] = key[31];
            int m;
            switch (i) {
            case 0:
                for (m = 0; m < 48; m++) {
                    keys[0][m] = tempKey[m];
                }
                break;
            case 1:
                for (m = 0; m < 48; m++) {
                    keys[1][m] = tempKey[m];
                }
                break;
            case 2:
                for (m = 0; m < 48; m++) {
                    keys[2][m] = tempKey[m];
                }
                break;
            case 3:
                for (m = 0; m < 48; m++) {
                    keys[3][m] = tempKey[m];
                }
                break;
            case 4:
                for (m = 0; m < 48; m++) {
                    keys[4][m] = tempKey[m];
                }
                break;
            case 5:
                for (m = 0; m < 48; m++) {
                    keys[5][m] = tempKey[m];
                }
                break;
            case 6:
                for (m = 0; m < 48; m++) {
                    keys[6][m] = tempKey[m];
                }
                break;
            case 7:
                for (m = 0; m < 48; m++) {
                    keys[7][m] = tempKey[m];
                }
                break;
            case 8:
                for (m = 0; m < 48; m++) {
                    keys[8][m] = tempKey[m];
                }
                break;
            case 9:
                for (m = 0; m < 48; m++) {
                    keys[9][m] = tempKey[m];
                }
                break;
            case 10:
                for (m = 0; m < 48; m++) {
                    keys[10][m] = tempKey[m];
                }
                break;
            case 11:
                for (m = 0; m < 48; m++) {
                    keys[11][m] = tempKey[m];
                }
                break;
            case 12:
                for (m = 0; m < 48; m++) {
                    keys[12][m] = tempKey[m];
                }
                break;
            case 13:
                for (m = 0; m < 48; m++) {
                    keys[13][m] = tempKey[m];
                }
                break;
            case 14:
                for (m = 0; m < 48; m++) {
                    keys[14][m] = tempKey[m];
                }
                break;
            case 15:
                for (m = 0; m < 48; m++) {
                    keys[15][m] = tempKey[m];
                }
                break;
            }
        }
        return keys;
    }
}

```

示例(DES3加密和解密):
index.html
```
<html xmlns="http://www.w3.org/1999/xhtml">
<head>
  <title>DES3</title>
  <meta http-equiv="Content-Type" content="text/html; charset=utf-8" />
  <script type="text/javascript" src="DES3.js"></script>
</head>
<body>
<script type="text/javascript">

var str= "123456.";
var key = "qXSdHWfbSZaaLeHBRhLgxBiG";
//alert(decrypt_3des);
var des3en = DES3.encrypt(key,str);
document.write("</br>des3加密:</br>"+des3en);
document.write("</br>des3解密:</br>"+DES3.decrypt(key,des3en));
</script>
</body>
```

DES3.js
```
/** 
 * DES 加密算法 
 * 
 * 该函数接受一个 8 字节字符串作为普通 DES 算法的密钥（也就是 64 位，但是算法只使用 56 位），或者接受一个 24 字节字符串作为 3DES 
 * 算法的密钥；第二个参数是要加密或解密的信息字符串；第三个布尔值参数用来说明信息是加密还是解密；接下来的可选参数 mode 如果是 0 表示 ECB 
 * 模式，1 表示 CBC 模式，默认是 ECB 模式；最后一个可选项是一个 8 字节的输入向量字符串（在 ECB 模式下不使用）。返回的密文是字符串。 
 * 
 * 参数： <br> 
 * key: 8字节字符串作为普通 DES 算法的密钥,或 24 字节字符串作为 3DES <br> 
 * message： 加密或解密的信息字符串<br> 
 * encrypt: 布尔值参数用来说明信息是加密还是解密<br> 
 * mode: 1:CBC模式，0:ECB模式(默认)<br> 
 * iv:<br> 
 * padding: 可选项, 8字节的输入向量字符串（在 ECB 模式下不使用） 
 */
//this takes the key, the message, and whether to encrypt or decrypt
function des (key, message, encrypt, mode, iv, padding) {
  if(encrypt) //如果是加密的话，首先转换编码
    message = unescape(encodeURIComponent(message));
  //declaring this locally speeds things up a bit
  var spfunction1 = new Array (0x1010400,0,0x10000,0x1010404,0x1010004,0x10404,0x4,0x10000,0x400,0x1010400,0x1010404,0x400,0x1000404,0x1010004,0x1000000,0x4,0x404,0x1000400,0x1000400,0x10400,0x10400,0x1010000,0x1010000,0x1000404,0x10004,0x1000004,0x1000004,0x10004,0,0x404,0x10404,0x1000000,0x10000,0x1010404,0x4,0x1010000,0x1010400,0x1000000,0x1000000,0x400,0x1010004,0x10000,0x10400,0x1000004,0x400,0x4,0x1000404,0x10404,0x1010404,0x10004,0x1010000,0x1000404,0x1000004,0x404,0x10404,0x1010400,0x404,0x1000400,0x1000400,0,0x10004,0x10400,0,0x1010004);
  var spfunction2 = new Array (-0x7fef7fe0,-0x7fff8000,0x8000,0x108020,0x100000,0x20,-0x7fefffe0,-0x7fff7fe0,-0x7fffffe0,-0x7fef7fe0,-0x7fef8000,-0x80000000,-0x7fff8000,0x100000,0x20,-0x7fefffe0,0x108000,0x100020,-0x7fff7fe0,0,-0x80000000,0x8000,0x108020,-0x7ff00000,0x100020,-0x7fffffe0,0,0x108000,0x8020,-0x7fef8000,-0x7ff00000,0x8020,0,0x108020,-0x7fefffe0,0x100000,-0x7fff7fe0,-0x7ff00000,-0x7fef8000,0x8000,-0x7ff00000,-0x7fff8000,0x20,-0x7fef7fe0,0x108020,0x20,0x8000,-0x80000000,0x8020,-0x7fef8000,0x100000,-0x7fffffe0,0x100020,-0x7fff7fe0,-0x7fffffe0,0x100020,0x108000,0,-0x7fff8000,0x8020,-0x80000000,-0x7fefffe0,-0x7fef7fe0,0x108000);
  var spfunction3 = new Array (0x208,0x8020200,0,0x8020008,0x8000200,0,0x20208,0x8000200,0x20008,0x8000008,0x8000008,0x20000,0x8020208,0x20008,0x8020000,0x208,0x8000000,0x8,0x8020200,0x200,0x20200,0x8020000,0x8020008,0x20208,0x8000208,0x20200,0x20000,0x8000208,0x8,0x8020208,0x200,0x8000000,0x8020200,0x8000000,0x20008,0x208,0x20000,0x8020200,0x8000200,0,0x200,0x20008,0x8020208,0x8000200,0x8000008,0x200,0,0x8020008,0x8000208,0x20000,0x8000000,0x8020208,0x8,0x20208,0x20200,0x8000008,0x8020000,0x8000208,0x208,0x8020000,0x20208,0x8,0x8020008,0x20200);
  var spfunction4 = new Array (0x802001,0x2081,0x2081,0x80,0x802080,0x800081,0x800001,0x2001,0,0x802000,0x802000,0x802081,0x81,0,0x800080,0x800001,0x1,0x2000,0x800000,0x802001,0x80,0x800000,0x2001,0x2080,0x800081,0x1,0x2080,0x800080,0x2000,0x802080,0x802081,0x81,0x800080,0x800001,0x802000,0x802081,0x81,0,0,0x802000,0x2080,0x800080,0x800081,0x1,0x802001,0x2081,0x2081,0x80,0x802081,0x81,0x1,0x2000,0x800001,0x2001,0x802080,0x800081,0x2001,0x2080,0x800000,0x802001,0x80,0x800000,0x2000,0x802080);
  var spfunction5 = new Array (0x100,0x2080100,0x2080000,0x42000100,0x80000,0x100,0x40000000,0x2080000,0x40080100,0x80000,0x2000100,0x40080100,0x42000100,0x42080000,0x80100,0x40000000,0x2000000,0x40080000,0x40080000,0,0x40000100,0x42080100,0x42080100,0x2000100,0x42080000,0x40000100,0,0x42000000,0x2080100,0x2000000,0x42000000,0x80100,0x80000,0x42000100,0x100,0x2000000,0x40000000,0x2080000,0x42000100,0x40080100,0x2000100,0x40000000,0x42080000,0x2080100,0x40080100,0x100,0x2000000,0x42080000,0x42080100,0x80100,0x42000000,0x42080100,0x2080000,0,0x40080000,0x42000000,0x80100,0x2000100,0x40000100,0x80000,0,0x40080000,0x2080100,0x40000100);
  var spfunction6 = new Array (0x20000010,0x20400000,0x4000,0x20404010,0x20400000,0x10,0x20404010,0x400000,0x20004000,0x404010,0x400000,0x20000010,0x400010,0x20004000,0x20000000,0x4010,0,0x400010,0x20004010,0x4000,0x404000,0x20004010,0x10,0x20400010,0x20400010,0,0x404010,0x20404000,0x4010,0x404000,0x20404000,0x20000000,0x20004000,0x10,0x20400010,0x404000,0x20404010,0x400000,0x4010,0x20000010,0x400000,0x20004000,0x20000000,0x4010,0x20000010,0x20404010,0x404000,0x20400000,0x404010,0x20404000,0,0x20400010,0x10,0x4000,0x20400000,0x404010,0x4000,0x400010,0x20004010,0,0x20404000,0x20000000,0x400010,0x20004010);
  var spfunction7 = new Array (0x200000,0x4200002,0x4000802,0,0x800,0x4000802,0x200802,0x4200800,0x4200802,0x200000,0,0x4000002,0x2,0x4000000,0x4200002,0x802,0x4000800,0x200802,0x200002,0x4000800,0x4000002,0x4200000,0x4200800,0x200002,0x4200000,0x800,0x802,0x4200802,0x200800,0x2,0x4000000,0x200800,0x4000000,0x200800,0x200000,0x4000802,0x4000802,0x4200002,0x4200002,0x2,0x200002,0x4000000,0x4000800,0x200000,0x4200800,0x802,0x200802,0x4200800,0x802,0x4000002,0x4200802,0x4200000,0x200800,0,0x2,0x4200802,0,0x200802,0x4200000,0x800,0x4000002,0x4000800,0x800,0x200002);
  var spfunction8 = new Array (0x10001040,0x1000,0x40000,0x10041040,0x10000000,0x10001040,0x40,0x10000000,0x40040,0x10040000,0x10041040,0x41000,0x10041000,0x41040,0x1000,0x40,0x10040000,0x10000040,0x10001000,0x1040,0x41000,0x40040,0x10040040,0x10041000,0x1040,0,0,0x10040040,0x10000040,0x10001000,0x41040,0x40000,0x41040,0x40000,0x10041000,0x1000,0x40,0x10040040,0x1000,0x41040,0x10001000,0x40,0x10000040,0x10040000,0x10040040,0x10000000,0x40000,0x10001040,0,0x10041040,0x40040,0x10000040,0x10040000,0x10001000,0x10001040,0,0x10041040,0x41000,0x41000,0x1040,0x1040,0x40040,0x10000000,0x10041000);
  //create the 16 or 48 subkeys we will need
  var keys = des_createKeys (key);
  var m=0, i, j, temp, temp2, right1, right2, left, right, looping;
  var cbcleft, cbcleft2, cbcright, cbcright2
  var endloop, loopinc;
  var len = message.length;
  var chunk = 0;
  //set up the loops for single and triple des
  var iterations = keys.length == 32 ? 3 : 9; //single or triple des
  if (iterations == 3) {looping = encrypt ? new Array (0, 32, 2) : new Array (30, -2, -2);}
  else {looping = encrypt ? new Array (0, 32, 2, 62, 30, -2, 64, 96, 2) : new Array (94, 62, -2, 32, 64, 2, 30, -2, -2);}
  //pad the message depending on the padding parameter
  if (padding == 2) message += "    "; //pad the message with spaces
  else if (padding == 1) {
    if(encrypt) {
      temp = 8-(len%8);
      message += String.fromCharCode(temp,temp,temp,temp,temp,temp,temp,temp);
      if (temp===8) len+=8;
    }
  } //PKCS7 padding
  else if (!padding) message += "\0\0\0\0\0\0\0\0"; //pad the message out with null bytes
  //store the result here
  var result = "";
  var tempresult = "";
  if (mode == 1) { //CBC mode
    cbcleft = (iv.charCodeAt(m++) << 24) | (iv.charCodeAt(m++) << 16) | (iv.charCodeAt(m++) << 8) | iv.charCodeAt(m++);
    cbcright = (iv.charCodeAt(m++) << 24) | (iv.charCodeAt(m++) << 16) | (iv.charCodeAt(m++) << 8) | iv.charCodeAt(m++);
    m=0;
  }
  //loop through each 64 bit chunk of the message
  while (m < len) {
    left = (message.charCodeAt(m++) << 24) | (message.charCodeAt(m++) << 16) | (message.charCodeAt(m++) << 8) | message.charCodeAt(m++);
    right = (message.charCodeAt(m++) << 24) | (message.charCodeAt(m++) << 16) | (message.charCodeAt(m++) << 8) | message.charCodeAt(m++);
    //for Cipher Block Chaining mode, xor the message with the previous result
    if (mode == 1) {if (encrypt) {left ^= cbcleft; right ^= cbcright;} else {cbcleft2 = cbcleft; cbcright2 = cbcright; cbcleft = left; cbcright = right;}}
    //first each 64 but chunk of the message must be permuted according to IP
    temp = ((left >>> 4) ^ right) & 0x0f0f0f0f; right ^= temp; left ^= (temp << 4);
    temp = ((left >>> 16) ^ right) & 0x0000ffff; right ^= temp; left ^= (temp << 16);
    temp = ((right >>> 2) ^ left) & 0x33333333; left ^= temp; right ^= (temp << 2);
    temp = ((right >>> 8) ^ left) & 0x00ff00ff; left ^= temp; right ^= (temp << 8);
    temp = ((left >>> 1) ^ right) & 0x55555555; right ^= temp; left ^= (temp << 1);
    left = ((left << 1) | (left >>> 31));
    right = ((right << 1) | (right >>> 31));
    //do this either 1 or 3 times for each chunk of the message
    for (j=0; j<iterations; j+=3) {
      endloop = looping[j+1];
      loopinc = looping[j+2];
      //now go through and perform the encryption or decryption
      for (i=looping[j]; i!=endloop; i+=loopinc) { //for efficiency
        right1 = right ^ keys[i];
        right2 = ((right >>> 4) | (right << 28)) ^ keys[i+1];
        //the result is attained by passing these bytes through the S selection functions
        temp = left;
        left = right;
        right = temp ^ (spfunction2[(right1 >>> 24) & 0x3f] | spfunction4[(right1 >>> 16) & 0x3f]
          | spfunction6[(right1 >>> 8) & 0x3f] | spfunction8[right1 & 0x3f]
          | spfunction1[(right2 >>> 24) & 0x3f] | spfunction3[(right2 >>> 16) & 0x3f]
          | spfunction5[(right2 >>> 8) & 0x3f] | spfunction7[right2 & 0x3f]);
      }
      temp = left; left = right; right = temp; //unreverse left and right
    } //for either 1 or 3 iterations
    //move then each one bit to the right
    left = ((left >>> 1) | (left << 31));
    right = ((right >>> 1) | (right << 31));
    //now perform IP-1, which is IP in the opposite direction
    temp = ((left >>> 1) ^ right) & 0x55555555; right ^= temp; left ^= (temp << 1);
    temp = ((right >>> 8) ^ left) & 0x00ff00ff; left ^= temp; right ^= (temp << 8);
    temp = ((right >>> 2) ^ left) & 0x33333333; left ^= temp; right ^= (temp << 2);
    temp = ((left >>> 16) ^ right) & 0x0000ffff; right ^= temp; left ^= (temp << 16);
    temp = ((left >>> 4) ^ right) & 0x0f0f0f0f; right ^= temp; left ^= (temp << 4);
    //for Cipher Block Chaining mode, xor the message with the previous result
    if (mode == 1) {if (encrypt) {cbcleft = left; cbcright = right;} else {left ^= cbcleft2; right ^= cbcright2;}}
    tempresult += String.fromCharCode ((left>>>24), ((left>>>16) & 0xff), ((left>>>8) & 0xff), (left & 0xff), (right>>>24), ((right>>>16) & 0xff), ((right>>>8) & 0xff), (right & 0xff));
    chunk += 8;
    if (chunk == 512) {result += tempresult; tempresult = ""; chunk = 0;}
  } //for every 8 characters, or 64 bits in the message
  //return the result as an array
  result += tempresult;
  result = result.replace(/\0*$/g, "");
  if(!encrypt ) { //如果是解密的话，解密结束后对PKCS7 padding进行解码，并转换成utf-8编码
    if(padding === 1) { //PKCS7 padding解码
      var len = result.length, paddingChars = 0;
      len && (paddingChars = result.charCodeAt(len-1));
      (paddingChars <= 8) && (result = result.substring(0, len - paddingChars));
    }
    //转换成UTF-8编码
    result = decodeURIComponent(escape(result));
  }
  return result;
} //end of des
//des_createKeys
//this takes as input a 64 bit key (even though only 56 bits are used)
//as an array of 2 integers, and returns 16 48 bit keys
function des_createKeys (key) {
  //declaring this locally speeds things up a bit
  var pc2bytes0 = new Array (0,0x4,0x20000000,0x20000004,0x10000,0x10004,0x20010000,0x20010004,0x200,0x204,0x20000200,0x20000204,0x10200,0x10204,0x20010200,0x20010204);
  var pc2bytes1 = new Array (0,0x1,0x100000,0x100001,0x4000000,0x4000001,0x4100000,0x4100001,0x100,0x101,0x100100,0x100101,0x4000100,0x4000101,0x4100100,0x4100101);
  var pc2bytes2 = new Array (0,0x8,0x800,0x808,0x1000000,0x1000008,0x1000800,0x1000808,0,0x8,0x800,0x808,0x1000000,0x1000008,0x1000800,0x1000808);
  var pc2bytes3 = new Array (0,0x200000,0x8000000,0x8200000,0x2000,0x202000,0x8002000,0x8202000,0x20000,0x220000,0x8020000,0x8220000,0x22000,0x222000,0x8022000,0x8222000);
  var pc2bytes4 = new Array (0,0x40000,0x10,0x40010,0,0x40000,0x10,0x40010,0x1000,0x41000,0x1010,0x41010,0x1000,0x41000,0x1010,0x41010);
  var pc2bytes5 = new Array (0,0x400,0x20,0x420,0,0x400,0x20,0x420,0x2000000,0x2000400,0x2000020,0x2000420,0x2000000,0x2000400,0x2000020,0x2000420);
  var pc2bytes6 = new Array (0,0x10000000,0x80000,0x10080000,0x2,0x10000002,0x80002,0x10080002,0,0x10000000,0x80000,0x10080000,0x2,0x10000002,0x80002,0x10080002);
  var pc2bytes7 = new Array (0,0x10000,0x800,0x10800,0x20000000,0x20010000,0x20000800,0x20010800,0x20000,0x30000,0x20800,0x30800,0x20020000,0x20030000,0x20020800,0x20030800);
  var pc2bytes8 = new Array (0,0x40000,0,0x40000,0x2,0x40002,0x2,0x40002,0x2000000,0x2040000,0x2000000,0x2040000,0x2000002,0x2040002,0x2000002,0x2040002);
  var pc2bytes9 = new Array (0,0x10000000,0x8,0x10000008,0,0x10000000,0x8,0x10000008,0x400,0x10000400,0x408,0x10000408,0x400,0x10000400,0x408,0x10000408);
  var pc2bytes10 = new Array (0,0x20,0,0x20,0x100000,0x100020,0x100000,0x100020,0x2000,0x2020,0x2000,0x2020,0x102000,0x102020,0x102000,0x102020);
  var pc2bytes11 = new Array (0,0x1000000,0x200,0x1000200,0x200000,0x1200000,0x200200,0x1200200,0x4000000,0x5000000,0x4000200,0x5000200,0x4200000,0x5200000,0x4200200,0x5200200);
  var pc2bytes12 = new Array (0,0x1000,0x8000000,0x8001000,0x80000,0x81000,0x8080000,0x8081000,0x10,0x1010,0x8000010,0x8001010,0x80010,0x81010,0x8080010,0x8081010);
  var pc2bytes13 = new Array (0,0x4,0x100,0x104,0,0x4,0x100,0x104,0x1,0x5,0x101,0x105,0x1,0x5,0x101,0x105);
  //how many iterations (1 for des, 3 for triple des)
  var iterations = key.length > 8 ? 3 : 1; //changed by Paul 16/6/2007 to use Triple DES for 9+ byte keys
  //stores the return keys
  var keys = new Array (32 * iterations);
  //now define the left shifts which need to be done
  var shifts = new Array (0, 0, 1, 1, 1, 1, 1, 1, 0, 1, 1, 1, 1, 1, 1, 0);
  //other variables
  var lefttemp, righttemp, m=0, n=0, temp;
  for (var j=0; j<iterations; j++) { //either 1 or 3 iterations
    var left = (key.charCodeAt(m++) << 24) | (key.charCodeAt(m++) << 16) | (key.charCodeAt(m++) << 8) | key.charCodeAt(m++);
    var right = (key.charCodeAt(m++) << 24) | (key.charCodeAt(m++) << 16) | (key.charCodeAt(m++) << 8) | key.charCodeAt(m++);
    temp = ((left >>> 4) ^ right) & 0x0f0f0f0f; right ^= temp; left ^= (temp << 4);
    temp = ((right >>> -16) ^ left) & 0x0000ffff; left ^= temp; right ^= (temp << -16);
    temp = ((left >>> 2) ^ right) & 0x33333333; right ^= temp; left ^= (temp << 2);
    temp = ((right >>> -16) ^ left) & 0x0000ffff; left ^= temp; right ^= (temp << -16);
    temp = ((left >>> 1) ^ right) & 0x55555555; right ^= temp; left ^= (temp << 1);
    temp = ((right >>> 8) ^ left) & 0x00ff00ff; left ^= temp; right ^= (temp << 8);
    temp = ((left >>> 1) ^ right) & 0x55555555; right ^= temp; left ^= (temp << 1);
    //the right side needs to be shifted and to get the last four bits of the left side
    temp = (left << 8) | ((right >>> 20) & 0x000000f0);
    //left needs to be put upside down
    left = (right << 24) | ((right << 8) & 0xff0000) | ((right >>> 8) & 0xff00) | ((right >>> 24) & 0xf0);
    right = temp;
    //now go through and perform these shifts on the left and right keys
    for (var i=0; i < shifts.length; i++) {
      //shift the keys either one or two bits to the left
      if (shifts[i]) {left = (left << 2) | (left >>> 26); right = (right << 2) | (right >>> 26);}
      else {left = (left << 1) | (left >>> 27); right = (right << 1) | (right >>> 27);}
      left &= -0xf; right &= -0xf;
      //now apply PC-2, in such a way that E is easier when encrypting or decrypting
      //this conversion will look like PC-2 except only the last 6 bits of each byte are used
      //rather than 48 consecutive bits and the order of lines will be according to
      //how the S selection functions will be applied: S2, S4, S6, S8, S1, S3, S5, S7
      lefttemp = pc2bytes0[left >>> 28] | pc2bytes1[(left >>> 24) & 0xf]
        | pc2bytes2[(left >>> 20) & 0xf] | pc2bytes3[(left >>> 16) & 0xf]
        | pc2bytes4[(left >>> 12) & 0xf] | pc2bytes5[(left >>> 8) & 0xf]
        | pc2bytes6[(left >>> 4) & 0xf];
      righttemp = pc2bytes7[right >>> 28] | pc2bytes8[(right >>> 24) & 0xf]
        | pc2bytes9[(right >>> 20) & 0xf] | pc2bytes10[(right >>> 16) & 0xf]
        | pc2bytes11[(right >>> 12) & 0xf] | pc2bytes12[(right >>> 8) & 0xf]
        | pc2bytes13[(right >>> 4) & 0xf];
      temp = ((righttemp >>> 16) ^ lefttemp) & 0x0000ffff;
      keys[n++] = lefttemp ^ temp; keys[n++] = righttemp ^ (temp << 16);
    }
  } //for each iterations
  //return the keys we've created
  return keys;
} //end of des_createKeys
function genkey(key, start, end) {
  //8 byte / 64 bit Key (DES) or 192 bit Key
  return {key:pad(key.slice(start, end)),vector: 1};
}
function pad(key) {
  for (var i = key.length; i<24; i++) {
    key+="0";
  }
  return key;
}
var des3iv = '12345678';

var DES3 = {
  //3DES加密，CBC/PKCS5Padding
  encrypt:function(key,input){
    var genKey = genkey(key, 0, 24);
    return btoa(des(genKey.key, input, 1, 1, des3iv, 1));
  },
  ////3DES解密，CBC/PKCS5Padding
  decrypt:function(key,input){
    var genKey = genkey(key, 0, 24); 
    return des(genKey.key, atob(input), 0, 1, des3iv, 1); 
  }
};
```
参考链接:
MD5算法的必要性以及实际应用:http://www.jiamisoft.com/blog/23015-qdgs.html
DES、AES、RSA、MD5加密算法辨析与应用场景:https://blog.csdn.net/kegebo_h/article/details/78056536
md5.js加密:https://www.cnblogs.com/CooLLYP/p/8628467.html
