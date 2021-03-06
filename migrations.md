---
description: >-
  If there are some special features or breaking changes with Libsodium
  versions, they will be displayed here. We try to keep these at a minimum.
---

# Migrations

## From 2.x.x

If you're on Lazysodium version 2 and you want to upgrade to version 3, then there are a few things that have been added which you need to note.

### Key.java added

All `cryptoSecretBoxKeygen` functions now return a `Key`. A `Key` holds a bunch of bytes that represent maybe a master key, or a subkey, or any type of key. The reason why Lazysodium chose to use this was mainly because users of the library were getting confused as to what to put into functions like:

```java
secretBoxLazy.cryptoSecretBoxEasy(String message, byte[] nonce, String key);
```

 As you can see, the above function takes in a String key but does it take a hexadecimal key or a normal plain key? Users may want to input their own keys, or they may want to generate a key using `cryptoSecretBoxKeygen`. In the former scenario, the user may input something like "a\_key\_that\_is\_32\_bytes" but the latter may generate something like "12ABEDAED21". The above function is actually expecting a hexadecimal string, so a user inputting "a\_key\_that\_is\_32\_bytes" would generate something incorrect.

**TL:DR**

So as a result of number 1 above, we replaced the String key with an object `Key`. To create a `Key` object, you now are required to understand what the string is. You should choose one of the following:

```java
// Choose this if the key is a hexadecimal string.
Key.fromHexString("12ABEDAED21");

// Choose this if the key is a plain string.
Key.fromPlainString("a_key_that_is_32_bytes");

// Choose this if the key in byte format
Key.fromBytes(bytes);
```

