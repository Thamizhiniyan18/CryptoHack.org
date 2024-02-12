# Modular Arithmetic

{% embed url="https://cryptohack.org/courses/modular/course_details/" %}

{% embed url="https://brilliant.org/wiki/modular-arithmetic/" %}

## Greatest Common Divisor

### Question

There are many tools to calculate the GCD of two integers, but for this task we recommend looking up [Euclid's Algorithm](https://en.wikipedia.org/wiki/Euclidean\_algorithm).\
Try coding it up; it's only a couple of lines. Use `a = 12, b = 8` to test it.\
Now calculate `gcd(a,b)` for `a = 66528, b = 52920` and enter it below.

{% embed url="https://brilliant.org/wiki/greatest-common-divisor/" %}

### Using Repititive Subtraction

{% embed url="https://brilliant.org/wiki/greatest-common-divisor/#euclidean-algorithm-and-more" %}

{% code lineNumbers="true" %}
```python
def gcd(a, b):
    if a == b:
        return a
    if a > b:
        return gcd(a - b, b)
    if a < b:
        return gcd(a, b - a)


print(gcd(84, 132))
```
{% endcode %}

### Using Modulo

{% embed url="https://brilliant.org/wiki/euclidean-algorithm/#computing-the-greatest-common-divisor" %}

```python
def gcd(a, b):
    return b if a % b == 0 else gcd(b, a % b)


print(gcd(66528, 52920))
```

### Using math.gcd()

```python
from math import gcd

print(gcd(66528, 52920))
```

***

## Extended GCD

Using the two primes `p = 26513, q = 32321`, find the integers `u,v` such that\
\
`p * u + q * v = gcd(p,q)`\
\
Enter whichever of `u` and `v` is the lower number as the flag.\
\
&#x20;Knowing that `p,q` are prime, what would you expect `gcd(p,q)` to be? For more details on the extended Euclidean algorithm, check out [this page](https://web.archive.org/web/20230511143526/http://www-math.ucdenver.edu/\~wcherowi/courses/m5410/exeucalg.html).

{% embed url="https://brilliant.org/wiki/modular-arithmetic/#modular-arithmetic-multiplicative-inverses" %}

{% code lineNumbers="true" %}
```python
def extended_gcd(x, y):
    # Initializing Values
    a, b = x, y
    if y > x:
        a, b = y, x
    current_m, previous_m = 0, 1
    current_n, previous_n = 1, 0

    while b != 0:
        q = a // b
        r = a % b
        m = current_m - previous_m * q
        n = current_n - previous_n * q

        a, b = b, r
        current_m, previous_m = previous_m, m
        current_n, previous_n = previous_n, n

    gcd = a

    return gcd, current_m, current_n
```
{% endcode %}

***

## Modular Arithmetic 1

Calculate the following integers:

11 ≡ x mod 6&#x20;

8146798528947 ≡ y mod 17

The solution is the smaller of the two integers.

```python
a = 11 % 6
b = 8146798528947 % 17

print(a if a < b else b)
```

***

## Modular Arithmetic 2

Now take the prime `p = 65537`. Calculate `273246787654 ^ 65536 mod 65537`.\
\
Did you need a calculator?

Answer: <mark style="color:red;">1</mark>

{% hint style="info" %}
According to Fermat's Little Theorem,

If 'p' is a prime number and 'a' is a positive integer and not divisible by 'p', then

$$a^{p-1} \equiv 1 \;\;(\;mod\;\;p\;)$$
{% endhint %}

{% embed url="https://brilliant.org/wiki/fermats-little-theorem/" %}

***

## Modular Inverting

### Question

What is the inverse element: `3 * d ≡ 1 mod 13`?

### Using Fermat's Little Theorem

{% hint style="info" %}
We can derive an equation from the Fermat's Little Theorem, that gives the multiplicative inverse of `3 * d ≡ 1 mod 13. The derivation is given below:`



$$a^{p-1} \equiv 1 \mod p\;(\;Fermat's\;Little\;Theorem\;)$$

$$Multiplicating\;a^{-1}\;on\;both\;sides$$

$$a^{p-1} \cdot a^{-1} \equiv a^{-1} \mod p$$

$$Rewriting\;a^{p-1}\;as\;a^{p-2}$$

$$a^{p-2} \cdot a \cdot a^{-1} \equiv a^{-1} \mod p$$

$$a^{p-2} \equiv a^{-1} \mod p$$



The final implication:

$$a^{p-2} \equiv a^{-1} \mod p \iff a^{p-2} \mod p = a^{-1}$$
{% endhint %}

```python
a = 3
p = 13

# You can use any method ** or pow to find the power.
# Both is going to give the same result.
# But using pow is more efficient
print(a**(p - 2) % p)
# or
print(pow(a, p-2, p))

```

### Using Extended Euclidean Algorithm

```python
def multiplicative_inverse(x, y):
    # Using Extended Euclidean Algorithm / Extended GCD
    a, b = x, y
    if y > x:
        a, b = y, x

    t1, t2 = 0, 1

    while b != 0:
        q = a // b
        r = a % b
        t = t1 - t2 * q

        a, b = b, r
        t1, t2 = t2, t

    if y > x:
        return y + t1  # To get a Positive Value
    else:
        return x + t1  # To get a Positive Value


print(multiplicative_inverse(7, 11))
```

### Using Crypto.Util.number.inverse()

```python
# pip install pycryptodome
from Crypto.Util.number import inverse

print(inverse(3, 13))
```

***

## Quadratic Residues

{% embed url="https://brilliant.org/wiki/quadratic-residues/" %}

Find the quadratic residue and then calculate its square root. Of the two possible roots, submit the smaller one as the flag.

p = 29&#x20;

ints = \[14, 6, 11]

{% code lineNumbers="true" %}
```python
from math import gcd

p = 29
ints = [14, 6, 11]

print(min([i for i in range(1, p) if gcd(i, p) == 1 and pow(i, 2, p) in ints]))
```
{% endcode %}

***

## Properties of Quadratic (Non-) Residues

Quadratic Residue \* Quadratic Residue = Quadratic Residue&#x20;

Quadratic Residue \* Quadratic Non-residue = Quadratic Non-residue&#x20;

Quadratic Non-residue \* Quadratic Non-residue = Quadratic Residue

***

## Legendre Symbol

### Definition

$$(a\; /\;p) \equiv a^{(p-1)\;/\;2}\;mod\;\;p\$$

where,

(a / p) = 1 if a is a quadratic residue and a ≢ 0 mod p&#x20;

(a / p) = -1 if a is a quadratic non-residue mod p&#x20;

(a / p) = 0 if a ≡ 0 mod p

Which means given any integer `a`, calculating `pow(a, (p-1) // 2,p)` is enough to determine if `a` is a quadratic residue.

### Question

Given the following 1024 bit prime and 10 integers, find the quadratic residue and then calculate its square root; the square root is your flag. Of the two possible roots, submit the larger one as your answer.

{% embed url="https://mathcenter.oxford.emory.edu/site/math125/findingSquareRoots/" %}

#### Formula for Finding Square Root

$$x \equiv \pm a^{(p+1)/4}\pmod{p}$$

```python
p = 101524035174539890485408575671085261788758965189060164484385690801466167356667036677932998889725476582421738788500738738503134356158197247473850273565349249573867251280253564698939768700489401960767007716413932851838937641880157263936985954881657889497583485535527613578457628399173971810541670838543309159139

ints = [
    25081841204695904475894082974192007718642931811040324543182130088804239047149283334700530600468528298920930150221871666297194395061462592781551275161695411167049544771049769000895119729307495913024360169904315078028798025169985966732789207320203861858234048872508633514498384390497048416012928086480326832803, 
    45471765180330439060504647480621449634904192839383897212809808339619841633826534856109999027962620381874878086991125854247108359699799913776917227058286090426484548349388138935504299609200377899052716663351188664096302672712078508601311725863678223874157861163196340391008634419348573975841578359355931590555, 
    17364140182001694956465593533200623738590196990236340894554145562517924989208719245429557645254953527658049246737589538280332010533027062477684237933221198639948938784244510469138826808187365678322547992099715229218615475923754896960363138890331502811292427146595752813297603265829581292183917027983351121325,
    14388109104985808487337749876058284426747816961971581447380608277949200244660381570568531129775053684256071819837294436069133592772543582735985855506250660938574234958754211349215293281645205354069970790155237033436065434572020652955666855773232074749487007626050323967496732359278657193580493324467258802863,
    4379499308310772821004090447650785095356643590411706358119239166662089428685562719233435615196994728767593223519226235062647670077854687031681041462632566890129595506430188602238753450337691441293042716909901692570971955078924699306873191983953501093343423248482960643055943413031768521782634679536276233318,
    85256449776780591202928235662805033201684571648990042997557084658000067050672130152734911919581661523957075992761662315262685030115255938352540032297113615687815976039390537716707854569980516690246592112936796917504034711418465442893323439490171095447109457355598873230115172636184525449905022174536414781771,
    50576597458517451578431293746926099486388286246142012476814190030935689430726042810458344828563913001012415702876199708216875020997112089693759638454900092580746638631062117961876611545851157613835724635005253792316142379239047654392970415343694657580353333217547079551304961116837545648785312490665576832987,
    96868738830341112368094632337476840272563704408573054404213766500407517251810212494515862176356916912627172280446141202661640191237336568731069327906100896178776245311689857997012187599140875912026589672629935267844696976980890380730867520071059572350667913710344648377601017758188404474812654737363275994871,
    4881261656846638800623549662943393234361061827128610120046315649707078244180313661063004390750821317096754282796876479695558644108492317407662131441224257537276274962372021273583478509416358764706098471849536036184924640593888902859441388472856822541452041181244337124767666161645827145408781917658423571721,
    18237936726367556664171427575475596460727369368246286138804284742124256700367133250078608537129877968287885457417957868580553371999414227484737603688992620953200143688061024092623556471053006464123205133894607923801371986027458274343737860395496260538663183193877539815179246700525865152165600985105257601565
]


for a in ints:
    legendre_symbol = pow(a, (p - 1) // 2, p)

    if legendre_symbol == 1: # Checking if Valid Residue
        print(pow(a, (p + 1) // 4, p))  # Finding the Positive Square Root
```

***

## Modular Square Root
