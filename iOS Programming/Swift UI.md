# 0 Reference

This article is referenced from [Official Documentation](https://developer.apple.com/documentation/swiftui).

# 1 User Interface

## 1.1 View and Controls

## 1.2 View Layout and Presentation

### 1.2.1 Stacks

In Swift UI, there are there kinds of stack that arranges its children in a horizonal or vertical line or overlays them, namely `HStack`, `VStack` and `ZStack`.

``` swift
// Decline in Standard way, take HStack as an example
HStack(spacing: 0, content: { ... })
// Or in a simplify way
HStack(spacing: 0) { ... }
```

