

This challenge is from ZeroDays 2025 and you can download the challs from [this link](https://github.com/ZeroDaysCTF/ZeroDaysCTF_2025_Public/)


So we get get the zipfile - 

<pre lang="md">$ wget https://github.com/ZeroDaysCTF/ZeroDaysCTF_2025_Public/blob/main/Reversing/theres-cocaine-in-it/theres_cocaine_in_it.zip</pre>

Lets try to unzip it

 ```shell
 $ unzip theres_cocaine_in_it.zip
 ```


```shell
Archive:  theres_cocaine_in_it.zip
[theres_cocaine_in_it.zip] x.pdf password: 
```

Ok, so we need a password and it has not been provided. Theres a few tools you can use for this but i normally go for fcrackzip. 

```shell
$ fcrackzip -v -D -p ~/home/rockyou.txt -u theres_cocaine_in_it.zip
found file 'x.pdf', (size cp/uc   1465/  3187, flags 1, chk 7917)


PASSWORD FOUND!!!!: pw == craggy
```

Great, the password is craggy. Seems like theres also a pdf in there.  Lets try unzipping again. 

```shell
$ unzip theres_cocaine_in_it.zip
Archive:  theres_cocaine_in_it.zip
[theres_cocaine_in_it.zip] x.pdf password: 
  inflating: x.pdf        
```

Running strings on the pdf gives us this:

```
strings x.pdf
%PDF-1.1
1 0 obj
 /Type /Catalog
 /Outlines 2 0 R
 /Pages 3 0 R
 /OpenAction 7 0 R
endobj
2 0 obj
 /Type /Outlines
 /Count 0
endobj
3 0 obj
 /Type /Pages
 /Kids [4 0 R]
 /Count 1
endobj
4 0 obj
 /Type /Page
 /Parent 3 0 R
 /MediaBox [0 0 612 792]
 /Contents 5 0 R
 /Resources <<
             /ProcSet [/PDF /Text]
             /Font << /F1 6 0 R >>
            >>
endobj
5 0 obj
<< /Length 60 >>
stream
BT /F1 12 Tf 100 700 Td 15 TL (Fup off, you grasshole) Tj ET
endstream
endobj
6 0 obj
 /Type /Font
 /Subtype /Type1
 /Name /F1
 /BaseFont /Helvetica
 /Encoding /MacRomanEncoding
endobj
7 0 obj
 /Type /Action
 /S /JavaScript
 /JS (const _0x3691f3=_0x88a0;function _0x88a0(_0x3d4bd1,_0x25017f){const _0x1cac54=_0x1cac();return _0x88a0=function(_0x88a0a0,_0x56871d){_0x88a0a0=_0x88a0a0-0xd4;let _0xb57251=_0x1cac54[_0x88a0a0];return _0xb57251;},_0x88a0(_0x3d4bd1,_0x25017f);}(function(_0x14f1db,_0x571ba6){const _0xce1c0a=_0x88a0,_0x5a30d0=_0x14f1db();while(!![]){try{const _0x2f90f6=-parseInt(_0xce1c0a(0xe5))/0x1*(-parseInt(_0xce1c0a(0xdf))/0x2)+parseInt(_0xce1c0a(0xd7))/0x3+-parseInt(_0xce1c0a(0xde))/0x4+parseInt(_0xce1c0a(0xdd))/0x5*(-parseInt(_0xce1c0a(0xd5))/0x6)+-parseInt(_0xce1c0a(0xe4))/0x7*(parseInt(_0xce1c0a(0xe0))/0x8)+parseInt(_0xce1c0a(0xdc))/0x9*(parseInt(_0xce1c0a(0xe2))/0xa)+-parseInt(_0xce1c0a(0xd8))/0xb*(-parseInt(_0xce1c0a(0xdb))/0xc);if(_0x2f90f6===_0x571ba6)break;else _0x5a30d0['push'](_0x5a30d0['shift']());}catch(_0x18f420){_0x5a30d0['push'](_0x5a30d0['shift']());}}}(_0x1cac,0xd34cc));const secret=_0x3691f3(0xd9);function xrstr(_0x3f11b8,_0x5e0ab5){const _0xc78adc=_0x3691f3;return _0x3f11b8[_0xc78adc(0xe6)]('')['map'](_0x2e7b19=>String[_0xc78adc(0xd4)](_0x2e7b19[_0xc78adc(0xe3)](0x0)^_0x5e0ab5))[_0xc78adc(0xda)]('');};function gzds(_0x33d4d4){const _0x222ba9=_0x3691f3,_0x2b05fc=_0x222ba9(0xe1),_0x280c13={'Z':'Z','e':'e','r':'r','o':'o','D':'D','a':'a','y':'y','s':'s','{':'{','}':'}','?':'?','\x20':'\x20','u':'u','d':'d','n':'n','t':'7','m':'m','b':'b','w':'w','3':'3','4':'4','6':'6','7':'7','0':'0','1':'1','9':'9','l':'l','j':'j','p':'p'};let _0x22e5f6='';for(let _0x39dca3 of _0x33d4d4){if(_0x280c13[_0x39dca3]!==undefined)_0x22e5f6+=_0x280c13[_0x39dca3];else return _0x222ba9(0xd6)+_0x39dca3+'\x27\x20is\x20not\x20in\x20the\x20source\x20set.';}return _0x22e5f6;}function _0x1cac(){const _0x203bec=['54oGQCrs','170FavwDh','3719192kWzpFJ','18nlZrAY','16ITdZkV','thequickbrownfoxjumpsoverthelazydogZD0123456789\x20?{}','550490qCBSxz','charCodeAt','2217719QVfSOI','40589ILFLWD','split','fromCharCode','292836HmCwqj','Error:\x20Character\x20\x27','2540904AzLAQc','11WdnJCM','[aZun<^diTB;iXN=EK]{nfokofBxA>=eA?E#EDg{h_N?A?EvEDovBDEtnfUkiXN=oeNenDYvEK|=nTMvob<1','join','30549084wmesGd'];_0x1cac=function(){return _0x203bec;};return _0x1cac();}const xr=0xc,dec=xrstr(secret,xr),f=gzds(atob(dec));)
endobj
xref
0000000000 65535 f
0000000012 00000 n
0000000109 00000 n
0000000165 00000 n
0000000234 00000 n
0000000439 00000 n
0000000557 00000 n
0000000681 00000 n
trailer
 /Size 8
 /Root 1 0 R
startxref
2951
%%EOF

```

Seems like theres some obfuscated javascript there. Lets isolate that.

```js
/JS (const _0x3691f3=_0x88a0;function _0x88a0(_0x3d4bd1,_0x25017f){const _0x1cac54=_0x1cac();return _0x88a0=function(_0x88a0a0,_0x56871d){_0x88a0a0=_0x88a0a0-0xd4;let _0xb57251=_0x1cac54[_0x88a0a0];return _0xb57251;},_0x88a0(_0x3d4bd1,_0x25017f);}(function(_0x14f1db,_0x571ba6){const _0xce1c0a=_0x88a0,_0x5a30d0=_0x14f1db();while(!![]){try{const _0x2f90f6=-parseInt(_0xce1c0a(0xe5))/0x1*(-parseInt(_0xce1c0a(0xdf))/0x2)+parseInt(_0xce1c0a(0xd7))/0x3+-parseInt(_0xce1c0a(0xde))/0x4+parseInt(_0xce1c0a(0xdd))/0x5*(-parseInt(_0xce1c0a(0xd5))/0x6)+-parseInt(_0xce1c0a(0xe4))/0x7*(parseInt(_0xce1c0a(0xe0))/0x8)+parseInt(_0xce1c0a(0xdc))/0x9*(parseInt(_0xce1c0a(0xe2))/0xa)+-parseInt(_0xce1c0a(0xd8))/0xb*(-parseInt(_0xce1c0a(0xdb))/0xc);if(_0x2f90f6===_0x571ba6)break;else _0x5a30d0['push'](_0x5a30d0['shift']());}catch(_0x18f420){_0x5a30d0['push'](_0x5a30d0['shift']());}}}(_0x1cac,0xd34cc));const secret=_0x3691f3(0xd9);function xrstr(_0x3f11b8,_0x5e0ab5){const _0xc78adc=_0x3691f3;return _0x3f11b8[_0xc78adc(0xe6)]('')['map'](_0x2e7b19=>String[_0xc78adc(0xd4)](_0x2e7b19[_0xc78adc(0xe3)](0x0)^_0x5e0ab5))[_0xc78adc(0xda)]('');};function gzds(_0x33d4d4){const _0x222ba9=_0x3691f3,_0x2b05fc=_0x222ba9(0xe1),_0x280c13={'Z':'Z','e':'e','r':'r','o':'o','D':'D','a':'a','y':'y','s':'s','{':'{','}':'}','?':'?','\x20':'\x20','u':'u','d':'d','n':'n','t':'7','m':'m','b':'b','w':'w','3':'3','4':'4','6':'6','7':'7','0':'0','1':'1','9':'9','l':'l','j':'j','p':'p'};let _0x22e5f6='';for(let _0x39dca3 of _0x33d4d4){if(_0x280c13[_0x39dca3]!==undefined)_0x22e5f6+=_0x280c13[_0x39dca3];else return _0x222ba9(0xd6)+_0x39dca3+'\x27\x20is\x20not\x20in\x20the\x20source\x20set.';}return _0x22e5f6;}function _0x1cac(){const _0x203bec=['54oGQCrs','170FavwDh','3719192kWzpFJ','18nlZrAY','16ITdZkV','thequickbrownfoxjumpsoverthelazydogZD0123456789\x20?{}','550490qCBSxz','charCodeAt','2217719QVfSOI','40589ILFLWD','split','fromCharCode','292836HmCwqj','Error:\x20Character\x20\x27','2540904AzLAQc','11WdnJCM','[aZun<^diTB;iXN=EK]{nfokofBxA>=eA?E#EDg{h_N?A?EvEDovBDEtnfUkiXN=oeNenDYvEK|=nTMvob<1','join','30549084wmesGd'];_0x1cac=function(){return _0x203bec;};return _0x1cac();}const xr=0xc,dec=xrstr(secret,xr),f=gzds(atob(dec));

```

Now lets paste that into https://deobfuscate.io

After getting a few syntax errors, I managed to clear up the code and it succesfully deobfuscated.

```js
function xrstr(_0x3f11b8, _0x5e0ab5) {
  return _0x3f11b8.split('').map(_0x2e7b19 => String.fromCharCode(_0x2e7b19.charCodeAt(0x0) ^ _0x5e0ab5)).join('');
}
;
function gzds(_0x33d4d4) {
  const _0x280c13 = {
    'Z': 'Z',
    'e': 'e',
    'r': 'r',
    'o': 'o',
    'D': 'D',
    'a': 'a',
    'y': 'y',
    's': 's',
    '{': '{',
    '}': '}',
    '?': '?',
    " ": " ",
    'u': 'u',
    'd': 'd',
    'n': 'n',
    't': '7',
    'm': 'm',
    'b': 'b',
    'w': 'w',
    '3': '3',
    '4': '4',
    '6': '6',
    '7': '7',
    '0': '0',
    '1': '1',
    '9': '9',
    'l': 'l',
    'j': 'j',
    'p': 'p'
  };
  let _0x22e5f6 = '';
  for (let _0x39dca3 of _0x33d4d4) {
    if (_0x280c13[_0x39dca3] !== undefined) {
      _0x22e5f6 += _0x280c13[_0x39dca3];
    } else {
      return "Error: Character '" + _0x39dca3 + "' is not in the source set.";
    }
  }
  return _0x22e5f6;
}
const dec = xrstr("[aZun<^diTB;iXN=EK]{nfokofBxA>=eA?E#EDg{h_N?A?EvEDovBDEtnfUkiXN=oeNenDYvEK|=nTMvob<1", 0xc);
```


Final solution script:

```python
import base64

secret = "[aZun<^diTB;iXN=EK]{nfokofBxA>=eA?E#EDg{h_N?A?EvEDovBDEtnfUkiXN=oeNenDYvEK|=nTMvob<1"
xored = ''.join(chr(ord(c) ^ 0xC) for c in secret)
flag = base64.b64decode(xored).decode()
print(flag)
```

```
$ python3 solve.py
ZeroDays{y0u d0n7 r3m3mb3r? y0u w3r3 w34r1n6 y0ur blu3 jump3r}
```

And thats it!  The flag is 

ZeroDays{y0u d0n7 r3m3mb3r? y0u w3r3 w34r1n6 y0ur blu3 jump3r}
