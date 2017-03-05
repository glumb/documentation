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
