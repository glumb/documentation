# utils


## convertUnits

IOTA utilizes the Standard system of Units. See below for all available units:

```
'i'   :   1,
'Ki'  :   1000,
'Mi'  :   1000000,
'Gi'  :   1000000000,
'Ti'  :   1000000000000,
'Pi'  :   1000000000000000
```

### Input
```
iota.utils.convertUnits(value, fromUnit, toUnit)
```

1. **`value`**: `Integer || String` Value to be converted. Can be string, an integer or float.
2. **`fromUnit`**: `String` Current unit of the value. See above for the available units to utilize for conversion.
2. **`toUnit`**: `String` Unit to convert the from value into.

### Returns
`Integer` - returns the converted unit (fromUnit => toUnit).

---

## addChecksum

Takes an 81-trytes address or a list of addresses as input and calculates the 9-trytes checksum of the address(es).

### Input
```
iota.utils.addChecksum(address)
```

1. **`address`**: `String | List` Either an individual address, or a list of addresses.

### Returns
`String | List` - returns the 90-trytes addresses (81-trytes address + 9-trytes checksum) either as a string or list, depending on the input.

---

## noChecksum

Takes an 90-trytes address as input and simply removes the checksum.

### Input
```
iota.utils.noChecksum(address)
```

1. **`address`**: `String | List` 90-trytes address. Either string or a list

### Returns
`String | List` - returns the 81-tryte address(es)

---

## isValidChecksum

Takes an 90-trytes checksummed address and returns a true / false if it is valid.

### Input
```
iota.utils.isValidChecksum(addressWithChecksum)
```

1. **`addressWithChecksum`**: `String` 90-trytes address  

### Returns
`Bool` - True / False whether the checksum is valid or not

---

## transactionObject

Converts the trytes of a transaction into its transaction object.

### Input
```
iota.utils.transactionObject(trytes)
```

1. **`trytes`**: `String` 2673-trytes of a transaction  

### Returns
`Object` - Transaction object

---

## transactionTrytes

Converts a valid transaction object into trytes. Please refer to [TODO] for more information what a valid transaction object looks like.

### Input
```
iota.utils.transactionTrytes(transactionObject)
```

1. **`transactionObject`**: `Object` valid transaction object  

### Returns
`trytes` - converted trytes of

---

## categorizeTransfers

Categorizes a list of transfers into `sent` and `received`. It is important to note that zero value transfers (which for example, is being used for storing addresses in the Tangle), are seen as `received` in this function.

### Input
```
iota.utils.categorizeTransfers(transfers, addresses)
```

1. **`transfers`**: `List` A list of bundles. Basically is an array, of arrays (bundles), as is returned from getTransfers or getAccountData
2. **`addresses`**: 'List' List of addresses that belong to you. With these addresses as input, it's determined whether it's a sent or a receive transaction. Therefore make sure that these addresses actually belong to you.

### Returns
`object` - the transfers categorized into `sent` and `received`
