# validate

## isAddress

Checks if the provided input is a valid 81-tryte (non-checksum), or 90-tryte (with checksum) address.

### Input
```
iota.validate.isAddress(address)
```

1. **`address`**: `String` A single address

---

## isTrytes

Determines if the provided input is valid trytes. Valid trytes are: `ABCDEFGHIJKLMNOPQRSTUVWXYZ9`. If you specify the length parameter, you can also validate the input length.

### Input
```
iota.validate.isTrytes(trytes [, length])
```

1. **`trytes`**: `String`
2. **`length`**: `int || string` optional

---

## isValue

Validates the value input, checks if it's integer.

### Input
```
iota.validate.isValue(value)
```

1. **`value`**: `Integer`

---

## isDecimal`

Checks if it's a decimal value

### Input
```
iota.validate.isDecimal(value)
```

1. **`value`**: `Integer || String`

---

## isHash

Checks if correct hash consisting of 81-trytes.

### Input
```
iota.validate.isHash(hash)
```

1. **`hash`**: `String`

---

## isTransfersArray

Checks if it's a correct array of transfer objects. A transfer object consists of the following values:
```
{
    'address': // STRING (trytes encoded, 81 or 90 trytes)
    'value': // INT
    'mesage': // STRING (trytes encoded)
    'tag': // STRING (trytes encoded, maximum 27 trytes)
}
```

### Input
```
iota.validate.isTransfersArray(transfersArray)
```

1. **`transfersArray`**: `array`

---

## isArrayOfHashes

Array of valid 81 or 90-trytes hashes.

### Input
```
iota.validate.isArrayOfHashes(hashesArray)
```

1. **`hashesArray`**: `Array`

---

## isArrayOfTrytes

Checks if it's an array of correct 2673-trytes. These are trytes either returned by prepareTransfers, attachToTangle or similar call. A single transaction object is encoded 2673 trytes.

### Input
```
iota.validate.isArrayOfTrytes(trytesArray)
```

1. **`trytesArray`**: `Array`

---

## isArrayOfAttachedTrytes

Similar to `isArrayOfTrytes`, just that in addition this function also validates that the last 243 trytes are non-zero (meaning that they don't equal 9). The last 243 trytes consist of:  `trunkTransaction` + `branchTransaction` + `nonce`. As such, this function determines whether the provided trytes have been attached to the tangle successfully. For example this validator can be used for trytes returned by `attachToTangle`.

### Input
```
iota.validate.isArrayOfAttachedTrytes(trytesArray)
```

1. **`trytesArray`**: `Array`

---

## isUri

Work in progress. If this is still here while you're reading the documentation, tell either Dominik or someone else from Core to move their asses.

### Input
```
iota.validate.isUri(uris)
```

1. **`uris`**: `Array`

---

## isInputs

Validates if it's an array of correct input objects. These inputs are provided to either `prepareTransfers` or `sendTransfer`. An input objects consists of the following:

```
{
    'keyIndex': // INT
    'address': // STRING
}
```

### Input
```
iota.validate.isInputs(inputsArray)
```

1. **`inputsArray`**: `Array`

---

## isString

Self explanatory.

### Input
```
iota.validate.isString(string)
```

---

## isInt

Self explanatory.

### Input
```
iota.validate.isInt(int)
```

---

## isArray

Self explanatory.

### Input
```
iota.validate.isArray(array)
```

---

## isObject

Self explanatory.

### Input
```
iota.validate.isObject(array)
```

---

## isUri

Validates a given string to check if it's a valid IPv6, IPv4 or hostname format. The string must have a `udp://` prefix, and it may or may not have a port. Here are some example inputs:

```
udp://[2001:db8:a0b:12f0::1]:14265
udp://[2001:db8:a0b:12f0::1]
udp://8.8.8.8:14265
udp://domain.com
udp://domain2.com:14265
```

### Input
```
iota.utils.isUri(node)
```

1. **`node`**: `String` IPv6, IPv4 or Hostname with or without a port.

### Returns
`bool` - true/false if valid node format.

---
