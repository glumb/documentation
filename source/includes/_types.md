# Trits and Trytes
Before diving into the API, it's important to understand the fundamental data types of IOTA.

IOTA data structures use [balanced ternary](https://en.wikipedia.org/wiki/Balanced_ternary) to represent numbers and strings.
As such, you will see many references to "trits" and "trytes".

## Trits
A **trit** can be thought of as the balanced ternary version of a bit.
Unlike a bit, which can have value 0 or 1, a trit can have value -1, 0 or 1.

Unless you are interfacing with PyOTA's crypto functionality, you probably won't have to deal with trits in your code.
 
## Trytes
A **tryte** can be thought of as the balanced ternary version of a byte.
Unlike a byte, which (usually) contains 8 bits, a tryte contains 3 trits.

# TryteStrings
Just like Python uses the byte string to represent a sequence of bytes, PyOTA defines a special type called `TryteString` to represent a sequence of trytes.

```python
trytes = TryteString(b'LH9GYEMHCF9GWHZFEELHVFOEOHNEEEWHZFUD')
```

- In a byte string, ASCII characters are used to represent individual bytes.
- In a `TryteString`, the characters 'A' through 'Z' and '9' are used to represent individual trytes.

As you go through the API documentation, you will see many references to `TryteString` and its subclasses:

- `Hash`: An object identifier.  Hashes are always 81 trytes long.
  There are many different types of hashes:
  - `Address`: Identifies an address on the Tangle.
  - `BundleHash`: Identifies a bundle on the Tangle.
  - `TransactionHash`: Identifies a transaction on the Tangle.
- `Seed`: A TryteString that is used for crypto functions such as generating addresses, signing inputs, etc.
  Seeds can be any length, but 81 trytes offers the best security.
- `Tag`: A tag used to classify a transaction.  Tags are always 27 trytes long.
- `TransactionTrytes`: A TryteString representation of a transaction on the Tangle.
  `TransactionTrytes` are always 2673 trytes long.
