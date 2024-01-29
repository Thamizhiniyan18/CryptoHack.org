# Introduction to CryptoHack

## ASCII

Using the below integer array, convert the numbers to their corresponding ASCII characters to obtain a flag.\
\
`[99, 114, 121, 112, 116, 111, 123, 65, 83, 67, 73, 73, 95, 112, 114, 49, 110, 116, 52, 98, 108, 51, 125]`

{% code lineNumbers="true" %}
```python
data = [99, 114, 121, 112, 116, 111, 123, 65, 83, 67, 73, 73, 95, 112, 114, 49, 110, 116, 52, 98, 108, 51, 125]

print(''.join(chr(i) for i in data))
```
{% endcode %}

***

## Hex

Included below is a flag encoded as a hex string. Decode this back into bytes to get the flag.\
\
`63727970746f7b596f755f77696c6c5f62655f776f726b696e675f776974685f6865785f737472696e67735f615f6c6f747d`

{% code lineNumbers="true" %}
```python
data = "63727970746f7b596f755f77696c6c5f62655f776f726b696e675f776974685f6865785f737472696e67735f615f6c6f747d"

print(bytes.fromhex(data))
```
{% endcode %}

***

## Base64

Take the below hex string, _decode_ it into bytes and then _encode_ it into Base64.\
\
`72bca9b68fc16ac7beeb8f849dca1d8a783e8acf9679bf9269f7bf`

{% code lineNumbers="true" %}
```python
import base64

data = "72bca9b68fc16ac7beeb8f849dca1d8a783e8acf9679bf9269f7bf"

print(base64.b64encode(bytes.fromhex(data)))
```
{% endcode %}

***

## Bytes and Big Integers

Convert the following integer back into a message:\
\
`11515195063862318899931685488813747395775516287289682636499965282714637259206269`

{% code lineNumbers="true" %}
```python
from Crypto.Util.number import *

data = 11515195063862318899931685488813747395775516287289682636499965282714637259206269

print(long_to_bytes(data))
```
{% endcode %}

***

## XOR Starter

Given the string `label`, XOR each character with the integer `13`. Convert these integers back to a string and submit the flag as `crypto{new_string}`

### With PwnTools

{% code lineNumbers="true" %}
```python
from pwn import xor

data = "label"

print(xor(data, 13))
```
{% endcode %}

### Without PwnTools

{% code lineNumbers="true" %}
```python
data = "label"

print(''.join(chr(ord(i) ^ 13) for i in data))
```
{% endcode %}

***

## XOR Properties

Below is a series of outputs where three random keys have been XOR'd together and with the flag. Use the above properties to undo the encryption in the final line to obtain the flag.

```
KEY1 = a6c8b6733c9b22de7bc0253266a3867df55acde8635e19c73313
KEY2 ^ KEY1 = 37dcb292030faa90d07eec17e3b1c6d8daf94c35d4c9191a5e1e
KEY2 ^ KEY3 = c1545756687e7573db23aa1c3452a098b71a7fbf0fddddde5fc1 
FLAG ^ KEY1 ^ KEY3 ^ KEY2 = 04ee9855208a2cd59091d04767ae47963170d1660df7f56f5faf
```

Before you XOR these objects, be sure to decode from hex to bytes.

### With PwnTools

{% code lineNumbers="true" %}
```python
from pwn import xor

Key1 = bytes.fromhex("a6c8b6733c9b22de7bc0253266a3867df55acde8635e19c73313")
KEY2_1 = bytes.fromhex("37dcb292030faa90d07eec17e3b1c6d8daf94c35d4c9191a5e1e")
KEY2_3 = bytes.fromhex("c1545756687e7573db23aa1c3452a098b71a7fbf0fddddde5fc1")
FLAG_KEY1_3_2 = bytes.fromhex("04ee9855208a2cd59091d04767ae47963170d1660df7f56f5faf")

Key2 = xor(Key1, KEY2_1) 
Key3 = xor(Key2, KEY2_3)
flag = xor(Key1, Key2, Key3, FLAG_KEY1_3_2)

print(flag)
```
{% endcode %}

### Without PwnTools

{% code lineNumbers="true" %}
```python
Key1 = bytes.fromhex("a6c8b6733c9b22de7bc0253266a3867df55acde8635e19c73313")
KEY2_1 = bytes.fromhex("37dcb292030faa90d07eec17e3b1c6d8daf94c35d4c9191a5e1e")
KEY2_3 = bytes.fromhex("c1545756687e7573db23aa1c3452a098b71a7fbf0fddddde5fc1")
FLAG_KEY1_3_2 = bytes.fromhex("04ee9855208a2cd59091d04767ae47963170d1660df7f56f5faf")

Key2 = [Key1[i] ^ KEY2_1[i] for i in range(0, len(Key1))] 
Key3 = [Key2[i] ^ KEY2_3[i] for i in range(0, len(Key2))]
flag = [FLAG_KEY1_3_2[i] ^ Key1[i] ^ Key2[i] ^ Key3[i] for i in range(0, len(Key3))]

print(''.join(chr(i) for i in flag))
```
{% endcode %}

***

## Favourite Byte

I've hidden some data using XOR with a single byte, but that byte is a secret. Don't forget to decode from hex first.\
\
`73626960647f6b206821204f21254f7d694f7624662065622127234f726927756d`

### With PwnTools

{% code lineNumbers="true" %}
```python
from pwn import xor

data = bytes.fromhex("73626960647f6b206821204f21254f7d694f7624662065622127234f726927756d")
secretByte = xor(data[0], "c") # Since we know the first character of the flag is "c"

print(xor(data, ord(secretByte)))
```
{% endcode %}

### Without PwnTools

{% code lineNumbers="true" %}
```python
data = bytes.fromhex("73626960647f6b206821204f21254f7d694f7624662065622127234f726927756d")
secretByte = data[0] ^ ord("c") # Since we know the first character of the flag is "c"

print(''.join(chr(data[i] ^ secretByte) for i in range(0, len(data))))
```
{% endcode %}

***

## You either know, XOR you don't

I've encrypted the flag with my secret key, you'll never be able to guess it.

`0e0b213f26041e480b26217f27342e175d0e070a3c5b103e2526217f27342e175d0e077e263451150104`

### With PwnTools

{% code lineNumbers="true" %}
```python
from pwn import xor

data = bytes.fromhex("73626960647f6b206821204f21254f7d694f7624662065622127234f726927756d")

flagFormat = "crypto{"

secretKey = [xor(data[i], flagFormat[i]) for i in range(0, len(flagFormat))] * 6 # Since we know the flag format "crypto{}"

print(b''.join(xor(data[i], secretKey[i]) for i in range(0, len(data))))
```
{% endcode %}

### Without PwnTools

{% code lineNumbers="true" %}
```python
data = bytes.fromhex("73626960647f6b206821204f21254f7d694f7624662065622127234f726927756d")

flagFormat = "crypto{"

secretKey = [data[i] ^ ord(flagFormat[i]) for i in range(0, len(flagFormat))] * 6 # Since we know the flag format "crypto{}"

print(''.join(chr(data[i] ^ secretKey[i]) for i in range(0, len(data))))
```
{% endcode %}
