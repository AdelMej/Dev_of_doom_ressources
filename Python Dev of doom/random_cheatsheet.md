# Python `random` Cheat Sheet

## 1. Importing

``` python
import random
```

## 2. Basic Random Values

``` python
random.random()        # 0.0 <= x < 1.0
random.uniform(a, b)   # a <= x <= b
random.randint(a, b)   # a <= n <= b
random.randrange(start, stop, step)
```

## 3. Random Choice Operations

``` python
random.choice(seq)
random.choices(seq, k=3)
random.sample(seq, k=3)
```

## 4. Shuffling

``` python
random.shuffle(my_list)
```

## 5. Random Booleans

``` python
random.choice([True, False])
random.choices([True, False], weights=[0.3, 0.7])[0]
```

## 6. Random Distributions

``` python
random.gauss(mu, sigma)
random.normalvariate(mu, sigma)
random.expovariate(lambd)
random.lognormvariate(mu, sigma)
random.triangular(low, high, mode)
```

## 7. Seeding

``` python
random.seed(123)
```
