# fat12-notes


That part is definitely the most confusing piece of FAT12 because computers read these bytes **backwards** (this is called Little Endian).

Let's use your exact numbers. Imagine looking at your hexdump and seeing these three bytes:

`12` `34` `56`

Every hex digit (like `1` or `2`) is exactly 4 bits. So a 12-bit entry is exactly **3 hex digits**.

Here is exactly how you rip those three bytes apart to get your two 12-bit clusters.

### The Breakdown

* **Byte 1:** `1` `2`
* **Byte 2 (The Shared Byte):** `3` `4`
* **Byte 3:** `5` `6`

---

### First Cluster (The Even Entry)

The first cluster takes all of **Byte 1**, plus the **right half** of Byte 2.

Because of Little Endian, the piece from the later byte (Byte 2) gets moved to the **front** of the final number:

1. Grab the right half of Byte 2: **`4`**
2. Grab all of Byte 1: **`12`**
3. Mash them together: **`412`**

Your first cluster value is **`0x412`**.

---

### Second Cluster (The Odd Entry)

The second cluster takes the **left half** of Byte 2, plus all of **Byte 3**.

Again, because of Little Endian, the later byte (Byte 3) gets moved to the **front**:

1. Grab all of Byte 3: **`56`**
2. Grab the leftover left half of Byte 2: **`3`**
3. Mash them together: **`563`**

Your second cluster value is **`0x563`**.

---

### The Summary

The middle byte (`34`) literally gets torn in half. The `4` goes to the first cluster, and the `3` goes to the second cluster.

The raw bytes `12 34 56` perfectly decode into cluster **`412`** and cluster **`563`**.
