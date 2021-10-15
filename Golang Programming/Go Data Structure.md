# Slice

Not concurrency safe.

## Declaration

- make([]Type, sice, cap)
- [start:end] from a existing slice / array
- var slice []Type

## Size Grow

- current size not larger than 1024, duplcate (predictSize match with System Setting Size)
- current size larger than 1024, times 1.25

