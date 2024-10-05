
|      One-Liner Code       |                         Function                          |       Example        |
| :-----------------------: | :-------------------------------------------------------: | :------------------: |
|           n & 1           |                  Check if number is odd                   |                      |
|         n & 1 ^ 1         |                  Check if number is even                  |                      |
|        n & (n - 1)        |              Clears the lowest set bit of n               |                      |
|       n & ~(n - 1)        | Extracts the lowest set bit of n (all others are cleared) |                      |
|  n & ~((1 << i+1 ) – 1)   |         Clears all bits of n from LSB to ith bit          |                      |
|    n & ((1 << i) – 1)     |         Clears all bits of n from MSB to ith bit          |                      |
|          n >> 1           |                      Divides n by 2                       |                      |
|          n << 1           |                     Multiplies n by 2                     |                      |
|         ch \| ‘ ‘         |       Upper case English alphabet ch to lower case        |                      |
|         ch & ‘_’          |       Lower case English alphabet ch to upper case        |                      |
|      n && !(n & n-1)      |      Checking if given 32-bit integer is power of 2       |                      |
|      log2(n & -n)+1       |                   Find the last set bit                   |                      |
|    n & ~(1 << (k - 1))    |               Turn Off kth bit in a number                |                      |
|    n \| (1 << (k - 1))    |                Turn On kth bit in a number                |                      |
| (n & (1 << (k - 1))) != 0 |           Check if kth bit is set for a number            |                      |
|    n ^ (1 << (k - 1))     |                    Toggle the kth bit                     |                      |
|        n & (n + 1)        |                 Clears all trailing ones                  | 00110111 -> 00110000 |
|       n \| (n + 1)        |                 Sets the last cleared bit                 | 00110101 -> 00110111 |
|         n & (-n)          |                 Extracts the last set bit                 | 00110100 -> 00000100 |
