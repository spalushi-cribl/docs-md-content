# Masking and Obfuscation


---

## Masking and Anonymization of Data in Motion

To mask patterns in real time, we use the out-of-the-box [Mask Function](mask-function) . This is similar to `sed`, but with much more powerful functionality.

## Masking Capabilities  

The Mask Function accepts multiple replacement rules, and accepts multiple fields to apply them to. 

**Match Regex** is a JS regex pattern that describes the content to be replaced. It can optionally contain matching groups. By default, it will stop after the first match, but using `/g` will make the Function replace all matches.

**Replace Expression** is a JS expression or literal to replace matched content. 

**Matching groups** can be referenced in the **Replace Expression** as `g1`, `g2`... `gN`, and the entire match as `g0`. 

There are several masking methods that are available under `C.Mask.`:
 
`C.Mask.random`: Generates a random alphanumeric string
`C.Mask.repeat`: Generates a repeating char/string pattern, e.g., `XXXX`
`C.Mask.REDACTED`: The literal 'REDACTED'
`C.Mask.md5`: Generates a MD5 hash of given value
`C.Mask.sha1`: Generates a SHA1 hash of given value
`C.Mask.sha256`: Generates a SHA256 hash of given value

Almost all methods have an optional `len` parameter which can be used to control the length of the replacement. `len` can be either a number or string. If it's a string, its length will be used. For example:

![Defining the replacement length](masking-1.b1d01213f3.png)
{border="true"}

## Masking Examples

Let's look at the various ways that we can mask a string like this one: `cardNumber=214992458870391`. The **Regex Match** we'll use is: `/(cardNumber=)(\d+)/g`. In this example:

  * `g0` = `cardNumber=214992458870391`
  * `g1` = `cardNumber=`
  * `g2` = `214992458870391`

### **Random Masking** with default character length (4): 
   * **Replace Expression**: `` `${g1}${C.Mask.random()}` ``  
   * **Result**: `cardNumber=HRhc`
  
### **Random Masking** with defined character length: 
  * **Replace Expression:** `` `${g1}${C.Mask.random(7)}` ``   
  * **Result**: `cardNumber=neNSm8r`

### **Random Masking** with length preserving replacement: 
  * **Replace Expression**: `` `${g1}${C.Mask.random(g2)}` ``  
  * **Result**: `cardNumber=DroJ73qmyaro51u3`

### **Repeat Masking**  with default character length (4):
  * **Replace Expression**: `` `${g1}${C.Mask.repeat()}` ``  
  * **Result**: **Result**: `cardNumber=XXXX`

### **Repeat Masking**  with defined character choice and length:
  * **Replace Expression**: `` `${g1}${C.Mask.repeat(6, 'Y')}` ``  
  * **Result**: `cardNumber=YYYYYY`

### **Repeat Masking** with length preserving replacement: 
  * **Replace Expression**: `` `${g1}${C.Mask.repeat(g2)}` `` 
  * **Result**: `cardNumber=XXXXXXXXXXXXXXX`

### Literal **REDACTED**  masking: 
  * **Replace Expression**: `` `${g1}${C.Mask.REDACTED}` ``  
  * **Result**: `cardNumber=REDACTED`

### **Hash Masking** (applies to: **md5**, **sha1** and **sha256**): 
  * **Replace Expression**: `` `${g1}${C.Mask.md5(g2)}` ``
  * **Result**: `cardNumber=f5952ec7e6da54579e6d76feb7b0d01f`

### **Hash Masking** with left N-length\* substring (applies to: **md5**, **sha1** and **sha256**):
  * **Replace Expression**: `` `${g1}${C.Mask.md5(g2, 12)}` `` 
  * **Result**: `cardNumber=d65a3ddb2749`
\*Replacement length will **not** exceed that of the hash algorithm output; MD5: 32 chars, SHA1: 40 chars, SHA256: 64 chars. 

### **Hash Masking** with right N-length\* substring (applies to: **md5**, **sha1** and **sha256**):
  * **Replace Expression**: `` `${g1}${C.Mask.md5(g2, -12)}` `` 
  * **Result**: `cardNumber= 933bfcebf992 `
\*Replacement length will **not** exceed that of the hash algorithm output; MD5: 32 chars, SHA1: 40 chars, SHA256: 64 chars. 

### **Hash Masking** with length\* preserving replacement (applies to: **md5**, **sha1** and **sha256**):
  * **Replace Expression**: `` `${g1}${C.Mask.md5(g2, g2)}` `` 
  * **Result**: `cardNumber= d65a3ddb27493f5 `
\*Replacement length will **not** exceed that of the hash algorithm output; MD5: 32 chars, SHA1: 40 chars, SHA256: 64 chars.
