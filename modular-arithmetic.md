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

Find the quadratic residue and then calculate its square root. Of the two possible roots, submit the smaller one as the flag.

p = 29&#x20;

ints = \[14, 6, 11]

{% embed url="https://brilliant.org/wiki/quadratic-residues/" %}

{% code lineNumbers="true" %}
```python
from math import gcd

p = 29
ints = [14, 6, 11]

print(min([i for i in range(1, p) if gcd(i, p) == 1 and pow(i, 2, p) in ints]))
```
{% endcode %}
