# Introduction of musicpy algorithms module

There are some music analysis algorithms I am developing based on musicpy, you can find them in the algorithms module in musicpy package.

The algorithms module is imported in musicpy as `alg` as default, so you can use the musicpy algorithms module like:

```python
import musicpy as mp

chord_type = mp.alg.detect(chord('C,E,G,B'))
>>> print(chord_type)
Cmaj7
# detect function is one of the music analysis algorithms in the algorithms module
```

Most of the music analysis algorithms in the algorithms module are still under development, and I will try to document some of the relatively developed algorithms in my spare time.

