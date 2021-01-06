# Hashtables

## What is a Hashtable

**Hash:** A hash is the result of some algorithm taking an incoming string and converting it into a value that could be used for either security or some other purpose. In the case of a hashtable, it is used to determine the index of the array.

**Buckets:** A bucket is what is contained in each index of the array of the hashtable. Each index is a bucket. An index could potentially contain multiple key/value pairs if a collision occurs.

**Collisions:** A collision is what happens when more than one key gets hashed to the same location of the hashtable.

**Why do we use them:**

Hold unique values - Dictionary - Library

## What Are they

Hashtables are a data structure that utilize key value pairs. This means every Node or Bucket has both a key, and a value.

The basic idea of a hashtable is the ability to store the key into this data structure, and quickly retrieve the value. This is done through what we call a `hash`.

> Since we are able to hash our key and determine the exact location where our value is stored, we can do a lookup in an O(1) time complexity

## Structure

### Hashing

a hash code turns a key into an integer. It’s very important that hash codes are deterministic: their output is determined only by their input.

### Creating a Hash

A hashtable traditionally is created from an array. I always like the size 1024. this is important for index placement. After you have created your array of the appropriate size, do some sort of logic to turn that “key” into a numeric number value. Here is a possible suggestion:

Add or multiply all the ASCII values together.
Multiply it by a prime number such as 599.
Use modulo to get the remainder of the result, when divided by the total size of the array.
Insert into the array at that index.
Example:

```
Key = "Cat"
Value = "Josie"

67 + 97 + 116 = 280

280 * 599 = 69648

69648 % 1024 = 16

Key gets placed in index of 16. 
```

We now know that our key Cat maps to index 16 of the array. We can view into this index and find “Josie”, our value quickly.

Each index of the array can hold many types of values. The trick is how the values are stored in comparison to efficiency. Each Index of the array contain “buckets”. Each of these “buckets” holds one key/value pair combination. When there is no entry in a specific bucket, the buckets (each index of the array) all start null. The hash table starts each bucket empty and overwrites their value once a key generates a hashCode that corresponds with an index.

### Collisions

A collision occurs when more than one key hashes to the same index in an array. As mentioned earlier, a “perfect hash” will never have any collisions. To put this into perspective, the worst possible hash is one that hashes every single key to the same exact index of an array. The more keys you have hashed to a specific index, the more key/value pair combos you can potentially have.

What would happen if two different keys resolved to be the same index of the array? This is called a collision.

If two keys ever ultimately resolved to the same index, then two calls to .Add(key, val) with different keys would overwrite each other.

Here’s an actual example of just one bucket in a real hash map. In this example the two different keys "Pioneer Square" and "Alki Beach" happen to ultimately resolve to the same bucket

```
hashMap.Add("Pioneer Square", 98104);
hashMap.Add("Alki Beach", 98116);

Bucket 92: [{Pioneer Square: 98104} --> {Alki Beach: 98116}]
```

If we didn’t store the key, the bucket would look like this. Accessing .get("Pioneer Square") or .get("Alki Beach") would hash the keys and still lead to bucket 92, but it would be impossible to tell which of the zip code values there to return.

`Bucket 92: [{98104} --> {98116}]`

### Bucket Sizes

Hash Maps can have any number of buckets. If a hash map has only a few buckets it will be densely full and have many collisions. If a hash map has more buckets it will be more sparsely populated, there will be less collisions, but there may be a lot of extra empty space.

**Recognize:** calculating load factors and choosing the optimal number of buckets, and determining the best hash functions is not within the scope of this class. This class intends to introduce you to what a hash table is, how it’s implemented, what hash codes are, how to handle collisions and how to reason generally about what it means for a hash table to be more empty or more full

## Internal Methods

**Add()**
When adding a new key/value pair to a hashtable:

- send the key to the GetHash method.
- Once you determine the index of where it should be placed, go to that index
- Check if something exists at that index already, if it doesn’t, add it with the key/value pair.
- If something does exist, add the new key/value pair to the data structure within that bucket.

**Find()**
The Find takes in a key, gets the Hash, and goes to the index location specified. Once at the index location is found in the array, it is then the responsibility of the algorithm the iterate through the bucket and see if the key exists and return the value.

**Contains()**
The Contains method will accept a key, and return a bool on if that key exists inside the hashtable. The best way to do this is to have the contains call the GetHash and check the hashtable if the key exists in the table given the index returned.

**GetHash()**
The GetHash will accept a key as a string, conduct the hash, and then return the index of the array where the key/value should be placed.
