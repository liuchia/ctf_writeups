# Hack Trinity '18

## Reverse Engineering - Jar of Java (100)
### Description
Hey, check out this password checker that I wrote for my programming class! I think it's very secure but I'm not sure. Can you reverse-engineer it to make sure it can't be broken?

You can run the program using the command java -jar password.jar

### Solution
As .jar files can be unzipped, I decided that might be a good thing to do. So I did : `unzip password.jar`

This gave a *PasswordCheck.class* file.

By running `strings PasswordCheck.class | grep HackTrinity{`, I found the flag **HackTrinity{When_You_Lose_The_Code_You_Lose_Your_Secrets}**

## Reverse Engineering - Hugs and Kisses (315)
### Description
Get some hugs and kisses from this binary file.

### Solution
I ran `file haxor` to find it was an 64 bit ELF file. `./haxor` told me to run it with the flag as an argument. One early thing I tried was `strings haxor` but of course tha didn't give us anything useful. So I disassembled the binary using `objdump -d haxor`. Essentially, the program was computing the flag in a function using some weird arithmetic and comparing that to the argument supplied. After browsing through the objdump, 0x4006bb is the location just before *computeFlag* function ends and the result is stored at %rax, which was an array of 30 characters. To find the contents of these characters, I ran `gdb haxor`. I set a breakpoint with `b *0x4006bb`. Then `r hello` to run with hello as the argument of the program. With `i r`, I can see the contents of the registers. I found that rax was holding the address 0x602010. I can print a null-terminated string starting at that address using `x/s $rax` which gave the string "OTYDIHMRWBAFKPUZYDINSRWBGLKPUZ". I tested the program with `./haxor OTYDIHMRWBAFKPUZYDINSRWBGLKPUZ` and it printed 'Nice Job. XOXO'. So the flag was of course **HackTrinity{OTYDIHMRWBAFKPUZYDINSRWBGLKPUZ}**

## Reverse Engineering - Lenny (350)
### Description
You're a TA assigned with the task of correcting programming assignments. For this particular assignment the students were allowed to use any language of their choosing. Lenny has decided to use some obscure language and refuses to give details.

See if you can run it.
### Solution
Using good ol' Google, I searched (♥ ͜ʖ♥) which was the first emoticon in the file. This directed me to [here]( https://esolangs.org/wiki/(_%CD%A1%C2%B0_%CD%9C%CA%96_%CD%A1%C2%B0)fuck ). It was essentially brainfuck with each symbol replaced by some emoticon. Using the Replace functionality of Notepad, I replaced the symbols as described on the esolangs page which resulted in this brainfuck script `-[------->+<]>-.[--->++++<]>+.++.++++++++.>-[--->+<]>-.-[--->+<]>+.---------.+++++.-----.+++++++++++.+++++.++.--[->+++<]>-.+.------------.[--->+++++<]>.[----->+++<]>.+++++.+++++.-----.-----.+++++++++++++++.+.+++++.+[->+++<]>.+++++.[-->+<]>--.[->++<]>-.[--->+<]>-.------------.[-->+<]>-.---[->++<]>-.++.--[--->+<]>--..+++[++>---<]>.+[--->+<]>+.+++++++.-.--------.+++++++++.[-->+<]>.>--[-->+++<]>.
`. Using an [online brainfuck interpreter](https://copy.sh/brainfuck/), I got the flag **HackTrinity{jk_I_did_not_d0_th3_ass1gnmen7}** 

## Reverse Engineering - Unfurlme (450)
### Description
Can you reverse engineer this obfuscated PHP source code?

https://unfurlme.hacktrinity.me/

### Solution
![](https://image.prntscr.com/image/zHN_QCIFS8_yOY0LruHpqg.png)

Clicking the view source button led me to this php script : 
```php
<?php 
if(isset($_GET['src'])){show_source(__FILE__);die();}
$wowzers="b\x61\x73e\x364_de\x63ode";$pandas="\x63o\x6ev\x65rt_\x75\x75de\x63\x6fde";$jenga="g\x7a\x75\x6ecomp\x72\x65ss";$a=$pandas(strrev($wowzers('CmAKSURCKCg5JzpIXUQzV11SNVMxNzkvClpZRDZBISc5Iik3O0otNTNQTVYzTClTOU9AJCxXODc2VCwmNFNAVzRWWCQyUS02NkElRDVJPVcrVzE1OU0KWl00MDRJRjxGOTQxVTFFOjdRRDAtPSM1QU1CLDoxRjMuTSQ1VTkjNjgxVTglQUUxKylXNUswNS5EUTQ2TQpSRCYtKFlGMjpRVjQsRTU1JyFVLUExIy5SSUY7NyVTOFRBJC4vXSYtTC1DNUQ9RDMwSUcxI0ElM0s0IztNCkYpRS0oMUY8WUBFODNdJjNSVFY6VigzNk8xRzwmNVQrOVE2PEtURjtQMUc9OEVXMUdNQjVYPVQ1UTkzPk0KNEk2PjNBJzlMLSM2Uy03PTY1RTNPSDc4NiU2NTZBRDpPNEc2UEEzPjIxRTBPKVMsUyw0NE0xVzlDKVY7TQooMTcuLkFFMS9NIjRWTFYxU0gkLCZBUyxYNVQ5WDQnM1lERDJUWTQsS0E3Lio9Rjk5PVY9WTlDPkUhJS1NCjJdIjQ5OUQ0Rj1XOSg5My44STYzUik2MFUtVz1LNFM1WClDMEFBVzNWMVYwRSknPFZIND5QTEQtKT0jLU0KTFVWLTUxNDhTLEcxOTVENktBVztXOTU5LD1GMlhANTQ2KUMyOkFGMzIxNjZYSFQ9SzFFPFIwRjxQLUc4TQpZTDQ5USglNE8xRzNLSEc8L0klPlYpNTwnWVQ7TDFUM1BgIzRWTUQ7IVFEPFlJJi5UTUYxMyUzPVEpJjJNCjktNz1TXDQ9R11GMVMsRjFGISU5JklUMFEoVzVCVVYtIU0kNFMsMzhUWEQ9UlRWPU4tRjpIUVQ4UDFEO00KV0FEODBFUzEuJTUzJkFVM0g1Nyw1MUc0RlFEOjpBUzsyLUMsRFk0PkotRTEhRVc6UC1TLC05UypEJUM9TQpSNVU7LEEzO0Q1RDtELTY9VzxTPDpBNjtFJSU9Vyk0OFoxRThCLTc0U0xSNCxNRD5aLTQ+Vzk0OSJdMjZNClM9JDRRRDYxNS0jPFJZRjolQUM8TlE2OFRVVjtUREY5WElGMUU1JS1KVSQ8SF1CNSxdJjQmQVYxJUk1MU0KT0RXLVQ0NT4tNUYxMVk0MVUlJjxYPVY4SDFHOi5NND0kMVMsU0VVOTFZJCxUWTQzWDFVMktMMjpSRFctTQooNTQsOCUjPFM5VSo1TUI7Mz1FOlYsNTU3NSY0MzFWKystJj1ZNFQ8JikzLFAkRDApISMtNz01OyspVSxNCiFNQjw4IUcsT0BUOE8oRzRKOUcsMlUmNEQ1VypSMSYzMylGNiNJJzxLQDMwJ0EjLCtdIi5QQVc9Ii1ENU0KOClHLVUhRTxLSSQtOTFUMkEpNiwyQSMsNi0lLDdFJTkjOVYzVyEzM0ItVy0lNVY6QjFWMFhdVCopJTYxTQpQISUxTDFVMyJBVTQwWTY7TEUmMiElJjpULTUuVjQzPDdBJT06QUMzLEk3PSRBRDpHIVU4Uy1XNTEtVjtNCkk1NTkwWUY+WkE2OFRBRTU3XVIqUD1ELFRVNDRJJVc6UiVTMyQtJjooUTYzJUlUPSlFVDQiVUQtVFw2LU0KLCU1LjhBNjRPSVY7QilTOktZVD0qNUYoSDQmOU8tNjlEXSUtVjRWPEEpJipTLTc5UiE3O08tRjtVSVc5TQ==')));eval("eval($wowzers($jenga($pandas($wowzers($a)))));");```

Running that source code through a [php deobfuscator](https://www.unphp.net/) resulted in 
```php
<?php
if (isset($_GET['src'])) {
    show_source(__FILE__);
    die();
}
$wowzers = "base64_decode";
$pandas = "convert_uudecode";
$jenga = "gzuncompress";
$a = $pandas(strrev($wowzers('CmAKSURCKCg5JzpIXUQzV11SNVMxNzkvClpZRDZBISc5Iik3O0otNTNQTVYzTClTOU9AJCxXODc2VCwmNFNAVzRWWCQyUS02NkElRDVJPVcrVzE1OU0KWl00MDRJRjxGOTQxVTFFOjdRRDAtPSM1QU1CLDoxRjMuTSQ1VTkjNjgxVTglQUUxKylXNUswNS5EUTQ2TQpSRCYtKFlGMjpRVjQsRTU1JyFVLUExIy5SSUY7NyVTOFRBJC4vXSYtTC1DNUQ9RDMwSUcxI0ElM0s0IztNCkYpRS0oMUY8WUBFODNdJjNSVFY6VigzNk8xRzwmNVQrOVE2PEtURjtQMUc9OEVXMUdNQjVYPVQ1UTkzPk0KNEk2PjNBJzlMLSM2Uy03PTY1RTNPSDc4NiU2NTZBRDpPNEc2UEEzPjIxRTBPKVMsUyw0NE0xVzlDKVY7TQooMTcuLkFFMS9NIjRWTFYxU0gkLCZBUyxYNVQ5WDQnM1lERDJUWTQsS0E3Lio9Rjk5PVY9WTlDPkUhJS1NCjJdIjQ5OUQ0Rj1XOSg5My44STYzUik2MFUtVz1LNFM1WClDMEFBVzNWMVYwRSknPFZIND5QTEQtKT0jLU0KTFVWLTUxNDhTLEcxOTVENktBVztXOTU5LD1GMlhANTQ2KUMyOkFGMzIxNjZYSFQ9SzFFPFIwRjxQLUc4TQpZTDQ5USglNE8xRzNLSEc8L0klPlYpNTwnWVQ7TDFUM1BgIzRWTUQ7IVFEPFlJJi5UTUYxMyUzPVEpJjJNCjktNz1TXDQ9R11GMVMsRjFGISU5JklUMFEoVzVCVVYtIU0kNFMsMzhUWEQ9UlRWPU4tRjpIUVQ4UDFEO00KV0FEODBFUzEuJTUzJkFVM0g1Nyw1MUc0RlFEOjpBUzsyLUMsRFk0PkotRTEhRVc6UC1TLC05UypEJUM9TQpSNVU7LEEzO0Q1RDtELTY9VzxTPDpBNjtFJSU9Vyk0OFoxRThCLTc0U0xSNCxNRD5aLTQ+Vzk0OSJdMjZNClM9JDRRRDYxNS0jPFJZRjolQUM8TlE2OFRVVjtUREY5WElGMUU1JS1KVSQ8SF1CNSxdJjQmQVYxJUk1MU0KT0RXLVQ0NT4tNUYxMVk0MVUlJjxYPVY4SDFHOi5NND0kMVMsU0VVOTFZJCxUWTQzWDFVMktMMjpSRFctTQooNTQsOCUjPFM5VSo1TUI7Mz1FOlYsNTU3NSY0MzFWKystJj1ZNFQ8JikzLFAkRDApISMtNz01OyspVSxNCiFNQjw4IUcsT0BUOE8oRzRKOUcsMlUmNEQ1VypSMSYzMylGNiNJJzxLQDMwJ0EjLCtdIi5QQVc9Ii1ENU0KOClHLVUhRTxLSSQtOTFUMkEpNiwyQSMsNi0lLDdFJTkjOVYzVyEzM0ItVy0lNVY6QjFWMFhdVCopJTYxTQpQISUxTDFVMyJBVTQwWTY7TEUmMiElJjpULTUuVjQzPDdBJT06QUMzLEk3PSRBRDpHIVU4Uy1XNTEtVjtNCkk1NTkwWUY+WkE2OFRBRTU3XVIqUD1ELFRVNDRJJVc6UiVTMyQtJjooUTYzJUlUPSlFVDQiVUQtVFw2LU0KLCU1LjhBNjRPSVY7QilTOktZVD0qNUYoSDQmOU8tNjlEXSUtVjRWPEEpJipTLTc5UiE3O08tRjtVSVc5TQ==')));
eval("eval($wowzers($jenga($pandas($wowzers($a)))));"); ?>
```

Recreating this step by step and echoing each step constructed this script to find a readable version of the php:
```php
<?php
$data = 'CmAKSURCKCg5JzpIXUQzV11SNVMxNzkvClpZRDZBISc5Iik3O0otNTNQTVYzTClTOU9AJCxXODc2VCwmNFNAVzRWWCQyUS02NkElRDVJPVcrVzE1OU0KWl00MDRJRjxGOTQxVTFFOjdRRDAtPSM1QU1CLDoxRjMuTSQ1VTkjNjgxVTglQUUxKylXNUswNS5EUTQ2TQpSRCYtKFlGMjpRVjQsRTU1JyFVLUExIy5SSUY7NyVTOFRBJC4vXSYtTC1DNUQ9RDMwSUcxI0ElM0s0IztNCkYpRS0oMUY8WUBFODNdJjNSVFY6VigzNk8xRzwmNVQrOVE2PEtURjtQMUc9OEVXMUdNQjVYPVQ1UTkzPk0KNEk2PjNBJzlMLSM2Uy03PTY1RTNPSDc4NiU2NTZBRDpPNEc2UEEzPjIxRTBPKVMsUyw0NE0xVzlDKVY7TQooMTcuLkFFMS9NIjRWTFYxU0gkLCZBUyxYNVQ5WDQnM1lERDJUWTQsS0E3Lio9Rjk5PVY9WTlDPkUhJS1NCjJdIjQ5OUQ0Rj1XOSg5My44STYzUik2MFUtVz1LNFM1WClDMEFBVzNWMVYwRSknPFZIND5QTEQtKT0jLU0KTFVWLTUxNDhTLEcxOTVENktBVztXOTU5LD1GMlhANTQ2KUMyOkFGMzIxNjZYSFQ9SzFFPFIwRjxQLUc4TQpZTDQ5USglNE8xRzNLSEc8L0klPlYpNTwnWVQ7TDFUM1BgIzRWTUQ7IVFEPFlJJi5UTUYxMyUzPVEpJjJNCjktNz1TXDQ9R11GMVMsRjFGISU5JklUMFEoVzVCVVYtIU0kNFMsMzhUWEQ9UlRWPU4tRjpIUVQ4UDFEO00KV0FEODBFUzEuJTUzJkFVM0g1Nyw1MUc0RlFEOjpBUzsyLUMsRFk0PkotRTEhRVc6UC1TLC05UypEJUM9TQpSNVU7LEEzO0Q1RDtELTY9VzxTPDpBNjtFJSU9Vyk0OFoxRThCLTc0U0xSNCxNRD5aLTQ+Vzk0OSJdMjZNClM9JDRRRDYxNS0jPFJZRjolQUM8TlE2OFRVVjtUREY5WElGMUU1JS1KVSQ8SF1CNSxdJjQmQVYxJUk1MU0KT0RXLVQ0NT4tNUYxMVk0MVUlJjxYPVY4SDFHOi5NND0kMVMsU0VVOTFZJCxUWTQzWDFVMktMMjpSRFctTQooNTQsOCUjPFM5VSo1TUI7Mz1FOlYsNTU3NSY0MzFWKystJj1ZNFQ8JikzLFAkRDApISMtNz01OyspVSxNCiFNQjw4IUcsT0BUOE8oRzRKOUcsMlUmNEQ1VypSMSYzMylGNiNJJzxLQDMwJ0EjLCtdIi5QQVc9Ii1ENU0KOClHLVUhRTxLSSQtOTFUMkEpNiwyQSMsNi0lLDdFJTkjOVYzVyEzM0ItVy0lNVY6QjFWMFhdVCopJTYxTQpQISUxTDFVMyJBVTQwWTY7TEUmMiElJjpULTUuVjQzPDdBJT06QUMzLEk3PSRBRDpHIVU4Uy1XNTEtVjtNCkk1NTkwWUY+WkE2OFRBRTU3XVIqUD1ELFRVNDRJJVc6UiVTMyQtJjooUTYzJUlUPSlFVDQiVUQtVFw2LU0KLCU1LjhBNjRPSVY7QilTOktZVD0qNUYoSDQmOU8tNjlEXSUtVjRWPEEpJipTLTc5UiE3O08tRjtVSVc5TQ==';
$step1 = strrev(base64_decode($data));
$step2 = convert_uudecode($step1);
$step2 = gzuncompress(base64_decode("eJwNkk2bojoQhX9QL5o46MBSIIwJEMlHhcDO1rkqiQMt2Gp+/WVXtahzznPeUiocQWsscPgjHDuzLN8ZtXWq569SthaAHilmnPSXBOTlDPpEaI+OxCdbkeE7sbM0wOfCdYW0SV08R1baKDY4JkrPu6rXVCBwxp8/K08GA8+pzCZbSLdr+udPmR2vjRr/cH/2pXr+A3RKmWW40IBA012FsE9tcK/dSPeWUS6jWSn+U+Vsp1X1EH7y2i++KTxMNt0NQgYs34DuKNjthcgxpauENQFeMyU47y/EZEGhFPoLV/hpMj4UeFjxfi4omtalnr8Ejnrp3UEi1PGsY/BeFwyCzzKLS+3QsbbTzaBwtQemhZs77ucdnEdm8LoUrv1d+6M33pkyAFSjyNd23Ro8ZjLfRtU1uhOXFMQNG9PbHwnDpcLhjcnwm2vN4a33PKA7mbWr1CJFdPfFc3FoguO3usYHbqu1SFkt8jyrLAnKvP00ODloNGqRvxZOrz+NtoPR1eK9bsprd2rTkwJ8YdRNhZJ2VQX8JgLeVwoxkZEYFs3aDU7ml47I6K0yJ6preCdvOxaB2xW5+wsuAbrMjX96HgwfRFYP/R4Pez6ywgYfgJ9xk1NtJI9Lu8gEx38F0J3Gk6P+OFXN9tHobcgtmQC332oBTRy8pZu/jHVUaVaz/NUVussX3ldxSyjTy6qWGxV+gGyXvtpnm+qlY/EFrtoY26km2LoSbX9rdH6Rfl5+LXCFzPNGdV3l4oO8Htc1Wnjr84a7PGUYLSlZJnH4i2YLd9T+WrKFXEcTXX6uTKNNdZ2+aT7MBLWjTuEFfrjTAOzeTw/wiVAaYcqHN6Sx3Pc4Yv70H/g2lOkpMSjmrBdpaZNzetsW/wNOhhvH"));
$step3 = base64_decode($step2);
$step4 = convert_uudecode($step3);
$step5 = gzuncompress($step4);
$step6 = base64_decode($step5);
echo $step6;

/* Outputs this: 
	if(isset($_REQUEST['src'])){
		show_source(__FILE__);
		die();
	}
	if(isset($_REQUEST['pass']) && $_REQUEST['pass']=="\x75\x6e\x66\x75\x72\x6c\x69\x6e\x67\x61\x6c\x6d\x6f\x73\x74\x63\x6f\x6d\x70\x6c\x65\x74\x65"){
		die("Congratulations the flag is: ".file_get_contents('/flag/flag.txt'));
	}else{
		die('<html><title>UnfurlMe.php</title><body><form method="post">Enter the password:<input type="password" name="pass"/><input type="submit" value="submit"/></form><br/><a href="?src">View Source</a></body></html>');
	}
*/
?>
```

The password is evidently a hexstring which when converted to ascii gives *unfurlingalmostcomplete*. Entering this into the password field of the page show the flag **HackTrinity{Functi0nsW1th1nFunc7i0ns}**.

## Crypto - Start Here (50)
### Description
Welcome to HackTrinity '18.

If you've never played a Capture-The-Flag before, the objective is to obtain flags of the form HackTrinity{<some_text>} by solving various challenges. That might involve exploiting a vulnerability in a website or finding it hidden in a file. Every time you submit a correct flag you get points. You can check your answer in the field below the challenge description.

Enter the flag HackTrinity{beginners_welcome} in the field below for 50 points.

![](https://ctf.hacktrinity.me/files/2c6f243a4297cf840207aa2c7990b1e5/freebie.gif)

### Solution
I'll leave this as an exercise to the reader (\*hint\* it might be **HackTrinity{beginners_welcome}**).

## Crypto - Netopian (100)
### Description
What's my WiFi password?

Hint: I haven't changed it since 2005

![](https://ctf.hacktrinity.me/files/97d9413215c56593b1fab1e315210a46/netopia.jpg)

![](https://ctf.hacktrinity.me/files/05b81cf58d1f712b1b74fe413750c214/wifi.jpg )

Enter the flag as HackTrinity{&ltwifi password&gt}

### Solution
A few google searches along the lines of 'Netopia 2005 Eircom Wifi Password' led me to find out the Eircom had a policy of setting the default wifi password as some function on the SSID. Using this [site](http://s4dd.yore.ma/eircom/) and the SSID supplied from the image I got the flag **HackTrinity{C8CBE3D384E8F0F0124978FF00}**

## Crypto - Secure Locker (200)
### Description
Trinity have launched their new SecureLocker™ document storage facility which uses high-tech cryptography (AES-CBC + PBKDFv2) to securely store documents. Can you break into the vault and retrieve the flag?

https://locker.hacktrinity.me

### Solution
![](https://image.prntscr.com/image/FndmvMmFSZmX2SOQLqC4_g.png)

Opening up the webpage and checking the page source shows that everything is done client-side in a script:
```javascript
    var pinEntry = document.getElementById("pinEntry");
    var resultsDiv = document.getElementById("results");

    var encryptedData = `{"iv":"Qh7QeZn7WMqccakZb6pvSg==","v":1,"iter":10000,"ks":128,"ts":64,"mode":"ccm","adata":"","cipher":"aes","salt":"RIxEs3CLWBg=","ct":"4LvJPsk45RGyuzajUs/6lc3HtUGlWCMkJQUgxpWm/AT8LYn9NK+Ga/yhP1cUH78nV7hwJieZMOKh8ep9qhjuxvZHYcz2GIo+"}`;

    function attemptDecrypt() {
      var pinGuess = pinEntry.value;

      var result = tryPinCode(pinGuess);
      
      if(result) {
        showSuccess(result);
      } else {
        showFail();
      }
    }

    function tryPinCode(pinGuess) {
      //NB: pinGuess must be in ASCII form, not integer
      try {
        //decrypt using Stanford Javascript Crypto library
        var plaintext = sjcl.decrypt(pinGuess, encryptedData);
        return plaintext;
      } catch (e) {
        //return null if decryption failed
        return null
      }
    }

    function showSuccess(plaintext) {
		resultsDiv.innerHTML = `<div class="alert alert-success">` + plaintext + `</div>`;
    }

    function showFail() {
      //clear the results page
      resultsDiv.innerHTML = "";
      //show result 100 ms later
      window.setTimeout(function() {
        resultsDiv.innerHTML = `<div class="alert alert-danger">Decryption failed</div>`;
      }, 100);
    }
```

So I opened up Chrome's console and brute forced all 10000 combinations of PINs using this quick snippet of code:
``` javascript
for (var a = 0; a < 10; a++) {
  for (var b = 0; b < 10; b++) {
    for (var c = 0; c < 10; c++) {
      for (var d = 0; d < 10; d++) {
        var pin = a.toString() + b.toString() + c.toString() + d.toString();
        var result = tryPinCode(pin);
        if (result) console.log(result); else console.log("fail");
      }
    }
  }
}
```

...and eventually got **HackTrinity{your_encryption_is_only_as_strong_as_your_key_space}**

## Crypto - Lotto (400)
### Description
In an effort to raise funds, Trinity have launched a lottery!

See if you can predict the next lotto numbers and win the jackpot.

https://lotto.hacktrinity.me

### Solution
![](https://image.prntscr.com/image/x6momgRDQh_EANSQyrbF3w.png )

The page allows you 2 guesses to guess the next 32 bit integer. The *How the draw works* button informs us that the numbers are generated using Java's *java.util.Random* library. So the goal is to figure out the seed based on earlier guesses. Luckily a Google search of 'How to get seed from java.util.Random' gives us this useful StackOverflow [page](https://stackoverflow.com/questions/8936891/is-there-a-way-to-generate-a-seed-out-of-a-sequence-of-numbers). Copy pasting the function to get the seed and applying it on the first two random numbers of the page if sufficient to get us **HackTrinity{they_call_it_pseudo_random_for_a_reason}**.

## Recon - PGP (217)
### Description
I received the attached vaguely insulting email from an anonymous sender. Can you help me track them down and exact revenge?

Flag is HackTrinity{&lt;name of sender>}

### Solution
First I ran `gpg email.txt.asc` which gave:
```
gpg: assuming signed data in `email.txt'
gpg: Signature made Thu 01 Feb 2018 11:42:00 STD using RSA key ID D8460642
gpg: Can't check signature: public key not found
```

The key is 0xD8460642. I went to https://pgp.key-server.io/ which is a pgp key database and searched that key.

![](https://image.prntscr.com/image/qzLjRDl3T9OwEkV4lUBPGw.png) 

So the flag is **HackTrinity{Eve Heinrichtson}**

## Recon - NotNotPetya (250)
### Description
Help! I've been hacked by some ransomware! I just turned on my Windows XP machine this morning and this message appeared on the screen:

![](https://ctf.hacktrinity.me/files/3199e1651f2b2b28be433b83f976cd62/ransomware.jpg )

Who hacked me? The NSA? CIA? Russia? I must know, please help.

Input the flag as HackTrinity{&lt;name_of_hacker>}

### Solution
On the bottom of the image there is a url which leads to this page :

![](https://image.prntscr.com/image/PiCHfVVPSuy9gjMExWAiVA.png )

We can see a bitcoin address at the very bottom. Searching for that address in Google leads us to [this](https://twitter.com/jonasandresen12) twitter page. With his name visible, the flag can be determined to be **HackTrinity{Jonas Andresen}**

## Recon - To Catch a Cheater (315)
### Description
College has been made aware of an exam paper leak. A copy of the printed paper was obtained and IT Services has been tasked with tracking the cheater. They believe it was printed on college printers. Can you help give the culprit a dose of r e a l i t y ?

### Solution
Copy pasting the description into Google somehow gave a relevant [link](https://lifehacker.com/nsa-leaker-outed-thanks-to-modern-printer-technology-1795854132) which led into an even more useful [link](https://w2.eff.org/Privacy/printers/docucolor/#program).

After screenshotting the top left corner of the *ExamPaper.pdf* and adjusting the levels of the image in Krita the dots mentioned in the above articles were visible: 

![](https://image.prntscr.com/image/elS67SwbTOGBuBPD_Ukz0Q.png)

Inputting the dots into the second link mentioned gave us the details necessary to identify the person from the log.

![](https://image.prntscr.com/image/2h7leFAxSLiAoFtd7JzPHw.png )

After unzipping *printerlogs.zip*, entering *Printer_2_318008* and opening *1st_of_April_log.txt*, we find two names under 13:37:
```
[01/Apr/2017:13:37:03] wen-huat
[01/Apr/2017:13:37:40] FerrisBueller
```

FerrisBueller was in CamelCase so it was obviously the better choice. **HackTrinity{FerrisBueller}**

## Web - Whiteboard (100)
### Description 
Say goodbye to Blackboard.... and hello to Whiteboard, the new Learning Management System for Trinity! All your grades are now available to view in a beautiful responsive interface.

Unfortunately you failed Ethical Hacking, so you don't get a Flag! Although if you manage to hack the system, your professor might give you some bonus marks...

https://whiteboard.hacktrinity.me

### Solution
![](https://image.prntscr.com/image/kYVu7OtlS2_Fu8eLm1e6DA.png)

They've kindly given us a username/password combination.

![](https://image.prntscr.com/image/Y-yM_KwgTcukD7hF1TmhDw.png )

It seems we need to pass all modules to get the flag, so I went to the Staff Area.

![](https://image.prntscr.com/image/5jOBy-ZfReWXxJrBMSfocQ.png )

Unfortunately, we're not staff. So I opened up Chrome's Developer Tools. Under Application -> Cookies -> auth, the value `student1%3Apassword1%3Astaff%3Dfalse` can be seen. I changed staff to true and refreshed the page.

![](https://image.prntscr.com/image/unR_QiTcT4CGtqpKAi7CBw.png)

After changing the grades in the Staff Area, I returned to My Grades and was greeted with the flag.

![](https://image.prntscr.com/image/7RBRNdHVTJmmPctBkv1WKw.png)

**HackTrinity{ThisWontWorkOnBlackboardSoDontEvenThinkAboutIt}** 

## Web - EyeTee (200)
### Description
Trinity EyeTee have launched a new knowledge-base website! But can you get access to the super-secret password?

https://eyeteehelpdesk.hacktrinity.me/

Enter the flag as HackTrinity{&lt;password>}

### Solution
![](https://image.prntscr.com/image/R2fHKglwRBi8JaVqfpbbUA.png )

After a few random searches, I searched the word *Step* and noticed *Student Computer Buyers Guide 2018* was included even though its accompanying text did not have the word *Step* visible.

![](https://image.prntscr.com/image/wdrVWuLzR16Gm9HNHFDUkA.png )

Also among the article previews was a password document containing a cut-off password which was probably the password mentioned in the challenge description. It also mentioned that it was 9 characters long and only consisted on lower case letters or digits.

![](https://image.prntscr.com/image/ZmiiTiKDQ0Sw4fU3ZLCDJQ.png ).

So I began depth first searching through combinations manually. I searched *a9ja* with no results. But *a9jb* however did result display a page.

![](https://image.prntscr.com/image/zycOnntuTbSQquaeUbew2A.png ).

![](https://image.prntscr.com/image/mJPB08oYQv_PT7a0yvxgtw.png ).

This meant that the password must begin with *a9jb*. Thus, I slowly began building up the password until it was 9 letters : **HackTrinity{a9jb7ib39}**

## Web - Koinex (200)
### Description
Being a white-hat hacker, you're invited by Koinex a cypto-exchange company to test their security, who knows if you are successful you might get some of that sweet Ethereum that you've been looking for.

https://koinex.hacktrinity.me

### Solution
![](https://image.prntscr.com/image/obypvigTRQKe5tLrEc_krA.png)

This website like an earlier challenge, did the authentication client-side:

```javascript
	var _0xad25 = ["\x23\x75\x73\x65\x72\x6E\x61\x6D\x65", "\x71\x75\x65\x72\x79\x53\x65\x6C\x65\x63\x74\x6F\x72", "\x23\x70\x61\x73\x73", "\x72\x65\x73\x75\x6C\x74\x73", "\x67\x65\x74\x45\x6C\x65\x6D\x65\x6E\x74\x42\x79\x49\x64", "\x61\x75\x74\x68\x2D\x62\x75\x74\x74\x6F\x6E", "\x63\x6C\x69\x63\x6B", "\x70\x72\x65\x76\x65\x6E\x74\x44\x65\x66\x61\x75\x6C\x74", "\x76\x61\x6C\x75\x65", "\x61\x64\x64\x45\x76\x65\x6E\x74\x4C\x69\x73\x74\x65\x6E\x65\x72", "\x55\x73\x65\x72\x6E\x61\x6D\x65\x20\x6F\x72\x20\x50\x61\x73\x73\x77\x6F\x72\x64\x20\x63\x61\x6E\x6E\x6F\x74\x20\x62\x65\x20\x65\x6D\x70\x74\x79", "\x70\x69\x72\x61\x74\x65\x73", "\x59\x6F\x75\x27\x72\x65\x20\x6C\x6F\x67\x67\x65\x64\x20\x69\x6E\x21\x0A\x46\x6C\x61\x67\x20\x69\x73\x20\x48\x61\x63\x6B\x54\x72\x69\x6E\x69\x74\x79\x7B\x26\x6C\x74\x3B\x70\x61\x73\x73\x77\x6F\x72\x64\x26\x67\x74\x3B\x7D", "\x57\x72\x6F\x6E\x67\x20\x43\x72\x65\x64\x65\x6E\x74\x69\x61\x6C\x73", "\x64\x38\x61\x39\x37\x39\x34\x65\x36\x35\x38\x62\x38\x32\x64\x38\x30\x35\x62\x38\x37\x31\x39\x61\x62\x64\x34\x32\x64\x32\x63\x32\x63\x65\x39\x35\x36\x64\x31\x64", "\x69\x6E\x6E\x65\x72\x48\x54\x4D\x4C", "", "\x73\x65\x74\x54\x69\x6D\x65\x6F\x75\x74"];
	var userField = document[_0xad25[1]](_0xad25[0]);
	var passField = document[_0xad25[1]](_0xad25[2]);
	var resultsDiv = document[_0xad25[4]](_0xad25[3]);
	var submitButton = document[_0xad25[4]](_0xad25[5]);
	submitButton[_0xad25[9]](_0xad25[6], function(_0xe58cx5) {
	    _0xe58cx5[_0xad25[7]]();
	    tryAuth(userField[_0xad25[8]], passField[_0xad25[8]])
	});
	function tryAuth(_0xe58cx7, _0xe58cx8) {
	    if (!_0xe58cx7 && !_0xe58cx8) {
	        invalidCredentials(_0xad25[10]);
	        return
	    }
	    ;if (_0xe58cx7 === _0xad25[11] && isValid(_0xe58cx8)) {
	        showSuccess(_0xad25[12])
	    } else {
	        invalidCredentials(_0xad25[13])
	    }
	}
	function isValid(_0xe58cx8) {
	    return sha1(_0xe58cx8) === _0xad25[14]
	}
	function invalidCredentials(_0xe58cxb) {
	    resultsDiv[_0xad25[15]] = _0xad25[16];
	    window[_0xad25[17]](function() {
	        resultsDiv[_0xad25[15]] = `<div class="alert alert-danger">` + _0xe58cxb + `</div>`
	    }, 100)
	}
	function showSuccess(_0xe58cxd) {
	    resultsDiv[_0xad25[15]] = `<div class="alert alert-success">` + _0xe58cxd + `</div>`
	}
```

In *showSuccess*, we can see it is displaying its argument which is supplied by *tryAuth* as array element 12. Entering `console.log(_0xad25[12])` to the console gives us `You're logged in! Flag is HackTrinity{&lt;password&gt;}` so we need to find the password. The second arg of *tryAuth* supplies the password and is checked in *isValid*. The SHA1 hash it needs to mash is array element 14 which has the value `d8a9794e658b82d805b8719abd42d2c2ce956d1d`. Google searching the hash gets us the password banter. **HackTrinity{banter}**

## Web - Unhackable (275)
### Dsecription
According to the owner, this site is literally unhackable.

https://unhackable.hacktrinity.me/

![](https://ctf.hacktrinity.me/files/49a036de04b437c954c579755d3381d0/surejan.gif )

### Solution
![](https://image.prntscr.com/image/1DvjkvFSQJ_uIa1KSYbixw.png )

It was indeed a static site. So I tried finding other files in the same directory as *index.html* such as *robots.txt*, resulting in many 404 errors.

However, *https://unhackable.hacktrinity.me/.git* didn't return a page or respond so I began following [this](https://github.com/bl4de/research/blob/master/hidden_directories_leaks/README.md#git) guide on how to extract the contents of that git repository.

Firstly, *https://unhackable.hacktrinity.me/.git/logs/HEAD* was placed into a folder called log in a new git repository. The file itself contained the following information : `0000000000000000000000000000000000000000 aebaab81b38bdb74cdc2e111118621d2f7dd001a Rory Flynn <roryflynn@users.noreply.github.com> 1517251255 +0000 commit (initial): initial commit of flags and index.html
aebaab81b38bdb74cdc2e111118621d2f7dd001a 4ae239c8f94d1bee157117a2c74720f51d16a1bf Rory Flynn <roryflynn@users.noreply.github.com> 1517251354 +0000 commit: Oops, remove those flags.txt. That was a close one`

To get the first commit I accessed *https://unhackable.hacktrinity.me/.git/objects/ae/baab81b38bdb74cdc2e111118621d2f7dd001a* and moved it into out local repository's object folder under a new subdirectory called *ae*. This new file's contents were : 
`tree 0a3725964eaad4d19e4b422a8bbe1cf40e5a451e
author Rory Flynn <roryflynn@users.noreply.github.com> 1517251255 +0000
committer Rory Flynn <roryflynn@users.noreply.github.com> 1517251255 +0000`

To download the tree, I went to *https://unhackable.hacktrinity.me/.git/objects/0a/3725964eaad4d19e4b422a8bbe1cf40e5a451e* and then moved it to a new subdirectory called *0a* in .git/objects. This file contained 
`100644 blob c2440c908a67b4266b6be5690fab86ee25977d80    flags.txt
100644 blob be6392988371f35dcc5dd6603e567e8153a70920    index.html`

We repeat the steps above for the blob of flags.txt. Now when we read the new file we get: **HackTrinity{hidden_git_directorys_gonna_git_ya}**

## Forensics - Tayto Smugglers (100)
### Description
In the post-Brexit economic apocalypse of the distant future (2019), Tayto crisps are a highly sought after luxury. They are now subject to high customs and charges when they are exported to the UK. An illegal Tayto smuggling ring has cropped up, and the EU would like to crack down on it.

You've been tasked with searching all digital files at the (hard) border and have come across this innocuous-looking picture of the Campanile. Can you ensure it contains no hidden bag of Tayto's crisps?

![](https://ctf.hacktrinity.me/files/28ab4f6552688a0adb6b5b660d562a5c/campanile.png)

Photo: Imelda Casey

### Solution
By running `binwalk campanile.png`, we get this output: 
`DECIMAL       HEXADECIMAL     DESCRIPTION
--------------------------------------------------------------------------------
16            0x10            JPEG image data, JFIF standard 1.01
124077        0x1E4AD         Zlib compressed data, best compression
129212        0x1F8BC         Zlib compressed data, best compression
1245203       0x130013        Zlib compressed data, best compression`

To extract this hidden jpeg file, I run `foremost campanile.png` and here it is:

![](https://image.prntscr.com/image/KQTr363PR-ms1WXG3l5nDg.png ).

**HackTrinity{you_can_pry_my_taytos_from_my_cold_dead_hands}**

## Forensics - Capture the Packet (100)
### Description
We (an unnamed three-letter agency) have managed to install a wire tap on a target's internet connection and have created the attached Packet Capture (PCAP) file.

Can you retrieve any useful information? It's probably all encrypted nowadays anyways thanks to those dratted tech companies.

![](http://i.imgur.com/XLQIsnH.gif )

### Solution
I used an online pcap analyser (https://packettotal.com) and checked the HTTP requests of *out.pcap*. There was one with the URI login.php. So I downloaded the sender and recipient files and in one of the files was `username=l33th4x0r&password=HackTrinity{this_is_what_the_NSA_sees_when_you_dont_encrypt}` so the flag is naturally **HackTrinity{this_is_what_the_NSA_sees_when_you_dont_encrypt}**

## Forensics - Biccies (150)
### Description
You are an investigative reporter with the Student Tribune and have filed a Freedom of Information Request for details of government spending on biscuits. However, the authorities have redacted the documents, citing National Security concerns. Can you find out how much they spent on Rich Tea?

Enter the flag as
HackTrinity{&lt;amount>} , including currency and commas where appropriate

### Solution
I went to http://www.extractpdf.com/ to upload biscuitdoc.pdf and extract the images in it so the black bars no longer obscure the figures. This gave me what I wanted :)

![](https://image.prntscr.com/image/KP6swyXUSpCIQ8Tgjj3efQ.png )

So the flag is **HackTrinity{€17,278,289}**
