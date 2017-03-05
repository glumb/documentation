# Basic Concepts
Before diving into the API, it's important to understand the fundamental data
types of IOTA.

:todo: Link to IOTA docs

# PyOTA Types
PyOTA defines a few types that will make it easy for you to model objects like
Transactions and Bundles in your own code.

## TryteString
A `TryteString` is an ASCII representation of a sequence of trytes.
In many respects, it is similar to a Python `bytes` object (which is an ASCII
representation of a sequence of bytes).

In fact, the two objects behave very similarly; they support concatenation,
comparison, can be used as dict keys, etc.

However, unlike `bytes`,  a `TryteString` can only contain uppercase letters and
the number 9 (as a regular expression: `^[A-Z9]*$`).

```python
from iota import TryteString

trytes_1 = TryteString(b'RBTC9D9DCDQAEASBYBCCKBFA')
trytes_2 = TryteString(b'LH9GYEMHCF9GWHZFEELHVFOEOHNEEEWHZFUD')

if trytes_1 != trytes_2:
  trytes_combined = trytes_1 + trytes_2

index = {
  trytes_1: 42,
  trytes_2: 86,
}
```

As you go through the API documentation, you will see many references to
`TryteString` and its subclasses:

- `Fragment`: A signature or message fragment inside a transaction.
  Fragments are always 2187 trytes long.
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

### Encoding
To encode character data into trytes, use the `TryteString.from_string` method.

```python
from iota import TryteString

message_trytes = TryteString.from_string('Hello, IOTA!')
```

You can also convert a tryte sequence into characters using
``TryteString.as_string``.  Note that not every tryte sequence can be converted;
garbage in, garbage out!

```python
from iota import TryteString

trytes = TryteString(b'RBTC9D9DCDQAEASBYBCCKBFA')
message = trytes.as_string()
```

<aside class="notice">
  <p>
    PyOTA also supports encoding non-ASCII characters, but this functionality is
    <strong>experimental</strong> and has not yet been evaluated by the IOTA
    community!
  </p>

  <p>
    Until this feature has been standardized, it is recommended that you only
    use ASCII characters when generating <code>TryteString</code> objects from
    character strings.
  </p>
</aside>

## Transaction Types
PyOTA defines two different types used to represent transactions:

### Transaction
`Transaction` is a transaction that has been loaded from the Tangle.

```python
from iota import Address, ProposedTransaction, Tag, Transaction

txn_1 =\
  Transaction.from_tryte_string(
    b'GYPRVHBEZOOFXSHQBLCYW9ICTCISLHDBNMMVYD9JJHQMPQCTIQAQTJNNNJ9IDXLRCC'
    b'OYOXYPCLR9PBEY9ORZIEPPDNTI9CQWYZUOTAVBXPSBOFEQAPFLWXSWUIUSJMSJIIIZ'
    b'WIKIRH9GCOEVZFKNXEVCUCIIWZQCQEUVRZOCMEL9AMGXJNMLJCIA9UWGRPPHCEOPTS'
    b'VPKPPPCMQXYBHMSODTWUOABPKWFFFQJHCBVYXLHEWPD9YUDFTGNCYAKQKVEZYRBQRB'
    b'XIAUX9SVEDUKGMTWQIYXRGSWYRK9SRONVGTW9YGHSZRIXWGPCCUCDRMAXBPDFVHSRY'
    b'WHGB9DQSQFQKSNICGPIPTRZINYRXQAFSWSEWIFRMSBMGTNYPRWFSOIIWWT9IDSELM9'
    b'JUOOWFNCCSHUSMGNROBFJX9JQ9XT9PKEGQYQAWAFPRVRRVQPUQBHLSNTEFCDKBWRCD'
    b'X9EYOBB9KPMTLNNQLADBDLZPRVBCKVCYQEOLARJYAGTBFR9QLPKZBOYWZQOVKCVYRG'
    b'YI9ZEFIQRKYXLJBZJDBJDJVQZCGYQMROVHNDBLGNLQODPUXFNTADDVYNZJUVPGB9LV'
    b'PJIYLAPBOEHPMRWUIAJXVQOEM9ROEYUOTNLXVVQEYRQWDTQGDLEYFIYNDPRAIXOZEB'
    b'CS9P99AZTQQLKEILEVXMSHBIDHLXKUOMMNFKPYHONKEYDCHMUNTTNRYVMMEYHPGASP'
    b'ZXASKRUPWQSHDMU9VPS99ZZ9SJJYFUJFFMFORBYDILBXCAVJDPDFHTTTIYOVGLRDYR'
    b'TKHXJORJVYRPTDH9ZCPZ9ZADXZFRSFPIQKWLBRNTWJHXTOAUOL9FVGTUMMPYGYICJD'
    b'XMOESEVDJWLMCVTJLPIEKBE9JTHDQWV9MRMEWFLPWGJFLUXI9BXPSVWCMUWLZSEWHB'
    b'DZKXOLYNOZAPOYLQVZAQMOHGTTQEUAOVKVRRGAHNGPUEKHFVPVCOYSJAWHZU9DRROH'
    b'BETBAFTATVAUGOEGCAYUXACLSSHHVYDHMDGJP9AUCLWLNTFEVGQGHQXSKEMVOVSKQE'
    b'EWHWZUDTYOBGCURRZSJZLFVQQAAYQO9TRLFFN9HTDQXBSPPJYXMNGLLBHOMNVXNOWE'
    b'IDMJVCLLDFHBDONQJCJVLBLCSMDOUQCKKCQJMGTSTHBXPXAMLMSXRIPUBMBAWBFNLH'
    b'LUJTRJLDERLZFUBUSMF999XNHLEEXEENQJNOFFPNPQ9PQICHSATPLZVMVIWLRTKYPI'
    b'XNFGYWOJSQDAXGFHKZPFLPXQEHCYEAGTIWIJEZTAVLNUMAFWGGLXMBNUQTOFCNLJTC'
    b'DMWVVZGVBSEBCPFSM99FLOIDTCLUGPSEDLOKZUAEVBLWNMODGZBWOVQT9DPFOTSKRA'
    b'BQAVOQ9RXWBMAKFYNDCZOJGTCIDMQSQQSODKDXTPFLNOKSIZEOY9HFUTLQRXQMEPGO'
    b'XQGLLPNSXAUCYPGZMNWMQWSWCKAQYKXJTWINSGPPZG9HLDLEAWUWEVCTVRCBDFOXKU'
    b'ROXH9HXXAXVPEJFRSLOGRVGYZASTEBAQNXJJROCYRTDPYFUIQJVDHAKEG9YACV9HCP'
    b'JUEUKOYFNWDXCCJBIFQKYOXGRDHVTHEQUMHO999999999999999999999999999999'
    b'999999999999999999999999999999999999999999999999999999999999999999'
    b'999999999999999999999999999999999999999999999999999999999999999999'
    b'999999999999999999999999999999999999999999999999999999999999999999'
    b'999999999999999999999999999999999999999999999999999999999999999999'
    b'999999999999999999999999999999999999999999999999999999999999999999'
    b'999999999999999999999999999999999999999999999999999999999999999999'
    b'999999999999999999999999999999999999999999999999999999999999999999'
    b'999999999999999999999999999999999999999999999999999999999999999999'
    b'999999999999999999999999999999999999999999999999999999999999999999'
    b'999999999999999999999999999999999999999999999999999999999999999999'
    b'999999999999RKWEEVD99A99999999A99999999NFDPEEZCWVYLKZGSLCQNOFUSENI'
    b'XRHWWTZFBXMPSQHEDFWZULBZFEOMNLRNIDQKDNNIELAOXOVMYEI9PGTKORV9IKTJZQ'
    b'UBQAWTKBKZ9NEZHBFIMCLV9TTNJNQZUIJDFPTTCTKBJRHAITVSKUCUEMD9M9SQJ999'
    b'999TKORV9IKTJZQUBQAWTKBKZ9NEZHBFIMCLV9TTNJNQZUIJDFPTTCTKBJRHAITVSK'
    b'UCUEMD9M9SQJ999999999999999999999999999999999999999999999999999999'
    b'999999999999999999999999999999999'
  )
```

Generally, you will never need to create `Transaction` objects; the API will
build them for you, as the result of various API methods.

### ProposedTransaction
`ProposedTransaction` is a transaction that was created locally and hasn't
been broadcast yet.

```python
txn_2 =\
  ProposedTransaction(
    address =
      Address(
        b'TESTVALUE9DONTUSEINPRODUCTION99999XE9IVG'
        b'EFNDOCQCMERGUATCIEGGOHPHGFIAQEZGNHQ9W99CH'
      ),

    message = TryteString.from_string('thx fur cheezburgers'),
    tag     = Tag(b'KITTEHS'),
    value   = 42,
  )
```

`ProposedTransaction` is useful when creating new transactions to broadcast to
the Tangle.
