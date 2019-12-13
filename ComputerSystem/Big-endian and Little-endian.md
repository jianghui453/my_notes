## Example

In a big-endian computer, the wo bytes required for the hexadecimal number 4F52 would be stored as 4F52 in storage (if 4F is stored at storage address 1000, for example, 52 will be at address 1001). In little-endian system, it would be stored as 524F (52 at address 1000, 4F at 1001).

## Big-endian's advantage

1. Readable
1. Comparison
1. Sign checking (by checking the byte at offset 0)

## Little-endian's advantage

Easer to increase a numeric value, you may need to add digits to the left (a higher non-exponential number has more digits). Thus, an addition of two numbers often requires moving all the digits of a big-endian ordered number in storage, moving everything to the righ. In a number stored in little-endian fashion, the least significant bytes can stay where they are and new digits can be added to the right at a high address. This means that some computer operation may be simpler and faster to perform.

## Summary

Little-endian is used more widely