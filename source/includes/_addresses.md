# Generating Addresses
In IOTA, addresses are generated deterministically from seeds.  This ensures
that your account can be accessed from any location, as long as you have the
seed.

Note that this also means that anyone with access to your seed can spend your
IOTAs!  Treat your seed(s) the same as you would the password for any other
financial service.

<aside class="notice">
  PyOTA's crytpo functionality is currently very slow; on average it takes 8-10
  seconds to generate each address.

  <p>
    These performance issues will be fixed in a future version of the library;
    please bear with us!
  </p>
</aside>

PyOTA provides two methods for generating addresses:

## Using the API
```python
from iota import Iota

api = Iota('http://localhost:14265', b'SEED9GOES9HERE')

# Generate 5 addresses, starting with index 0.
gna_result = api.get_new_addresses(count=5)
addresses = gna_result['addresses']

# Generate 1 address, starting with index 42:
gna_result = api.get_new_addresses(start=42)
addresses = gna_result['addresses']

# Find the first unused address, starting with index 86:
gna_result = api.get_new_addresses(start=86, count=None)
addresses = gna_result['addresses']
```

To generate addresses using the API, invoke its `get_new_addresses` method,
using the following parameters:

- `start: int`: The starting index (defaults to 0).  This can be used to skip
  over addresses that have already been generated.
- `count: Optional[int]`: The number of addresses to generate (defaults to 1).
  - If `None`, the API will generate addresses until it finds one that has not
    been used (has no transactions associated with it on the Tangle).  It will
    then return the unused address and discard the rest.

`get_new_addresses` returns a dict with the following items:

- `addresses: List[Address]`: The generated address(es).
  Note that this value is always a list, even if only one address was generated.

<aside class="notice">
  For better performance, it is recommended that you install an address cache
  (see below).
</aside>

## Using AddressGenerator
```python
from iota.crypto.addresses import AddressGenerator

generator = AddressGenerator(b'SEED9GOES9HERE')

# Generate a list of addresses:
addresses = generator.get_addresses(start=0, count=5)

# Generate a list of addresses in reverse order:
addresses = generator.get_addresses(start=42, count=10, step=-1)

# Create an iterator, advancing 5 indices each iteration.
iterator = generator.create_iterator(start=86, step=5)
for address in iterator:
  ...
```

If you want more control over how addresses are generated, you can use the
`AddressGenerator` class.

`AddressGenerator` can create iterators, allowing your application to generate
addresses as needed, instead of having to generate lots of addresses up front.

You can also specify an optional `step` parameter, which allows you to skip over
multiple addresses between iterations... or even iterate over addresses in
reverse order!

`AddressGenerator` provides two methods:

- `get_addresses: (int, int, int) -> List[Address]`: Returns a list of
  addresses.  This is the same method that the `get_new_addresses` API command
  uses internally.
- `create_iterator: (int, int) -> Generator[Address]`: Returns an iterator that
  will create addresses endlessly.  Use this if you have a feature that needs
  to generate addresses "on demand".

# Address Caches
```python
from iota.crypto.addresses import AddressGenerator, MemoryAddressCache

# Install a global address cache.
AddressGenerator.cache = MemoryAddressCache()

cache = MemoryAddressCache()

# Activate the address cache just for this generator instance.
generator = AddressGenerator(b'SEED9GOES9HERE')
generator.cache = cache
```

Generating addresses can be a CPU-intensive task, especially if you find
yourself generating the same addresses over and over.  To eliminate all of this
redundant work, you can install an address cache.

PyOTA currently only defines a single type of address cache, which stores
addresses in memory.  If desired, you can write a custom address cache that
uses a more permanent storage backend (database, filesystem, etc.).

To install the address cache globally, attach it to the `AddressGenerator`
class.  This will activate the address cache for all API calls, as well as
any addresses created by `AddressGenerator`.

If desired, you can activate the cache only for specific `AddressGenerator`
instances.

Note that API methods will only use the global address cache; there is no way to
specify an address cache that will be used just by API methods.

## Writing Your Own Address Cache
```python
class BaseAddressCache(with_metaclass(ABCMeta)):
  @abstract_method
  def get(self, seed, index):
    # type: (Seed, int) -> Optional[Address]
    """
    Retrieves an address from the cache.
    Returns ``None`` if the address hasn't been cached yet.
    """

  @abstract_method
  def set(self, seed, index, address):
    # type: (Seed, int, Address) -> None
    """
    Adds an address to the cache, overwriting the existing address if
    set.
    """
```

To write your own address cache, extend the `BaseAddressCache` command.  You
will need to implement its `get` and `set` methods, used to retrieve and store
addresses, respectively.

<aside class="notice">
  <p>
    <code>BaseAddressCache</code> also defines
    <code>_gen_cache_key(seed, index)</code> to hash the seed before using it as
    part of the cache key.  Be sure to take advantage of this method when
    writing your own address cache class.
  </p>

  <p>
    This is especially important when using a permanent storage backend such as
    a database or filesystem.
  </p>

  <p>
    Please do not store your users' seeds in cleartext!
  </p>
</aside>

For more information, refer to the implementation of
`iota.crypto.addresses.MemoryAddressCache`.
