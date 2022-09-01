[![Xybor founder](https://img.shields.io/badge/xybor-huykingsofm-red)](https://github.com/huykingsofm)
[![Go Reference](https://pkg.go.dev/badge/github.com/xybor-x/xylock.svg)](https://pkg.go.dev/github.com/xybor-x/xylock)
[![GitHub Repo stars](https://img.shields.io/github/stars/xybor-x/xylock?color=yellow)](https://github.com/xybor-x/xylock)
[![GitHub top language](https://img.shields.io/github/languages/top/xybor-x/xylock?color=lightblue)](https://go.dev/)
[![GitHub go.mod Go version](https://img.shields.io/github/go-mod/go-version/xybor-x/xylock)](https://go.dev/blog/go1.18)
[![GitHub release (release name instead of tag name)](https://img.shields.io/github/v/release/xybor-x/xylock?include_prereleases)](https://github.com/xybor-x/xylock/releases/latest)
[![Codacy Badge](https://app.codacy.com/project/badge/Grade/e649f578cab845d98f1134050d9cc3ce)](https://www.codacy.com/gh/xybor-x/xylock/dashboard?utm_source=github.com&utm_medium=referral&utm_content=xybor-x/xylock&utm_campaign=Badge_Grade)
[![Codacy Badge](https://app.codacy.com/project/badge/Coverage/e649f578cab845d98f1134050d9cc3ce)](https://www.codacy.com/gh/xybor-x/xylock/dashboard?utm_source=github.com&utm_medium=referral&utm_content=xybor-x/xylock&utm_campaign=Badge_Coverage)
[![Go Report](https://goreportcard.com/badge/github.com/xybor-x/xylock)](https://goreportcard.com/report/github.com/xybor-x/xylock)

# Introduction

Package xylock defines wrapper types of sync mutex, rwmutex, and semaphore.

# Features

Xylock wrapper structs have fully methods of origin structs. For example, `Lock`
is the wrapper struct of `sync.Mutex`, and it has the following methods:

```golang
func (l *Lock) Lock()
func (l *Lock) Unlock()
```

Methods of wrapper structs have an additional features, that is to do nothing if
the receiver pointer is nil. This is helpful when lock is an optional
development.

```golang
// These following commands will not cause a panic. They just do nothing.
var lock *xylock.Lock = nil
lock.Lock()
lock.Unlock()
```

Xylock structs allows to run a function in thread-safe area (with Lock and
Unlock cover the function).

```golang
var lock = xylock.Lock{}
lock.LockFunc(func() {
    // thread-safe area
})
```

Thread-safe methods of Xylog structs with `R` as prefix will support to read
data.

```golang
var foo int
var lock = xylock.Lock{}
var result = lock.RLockFunc(func() any {
    return foo
}).(int)
```

# Example

```golang
func Example() {
	var x int
	var lock = xylock.Lock{}
	for i := 0; i < 10000; i++ {
		go lock.LockFunc(func() {
			x = x + 1
		})
	}

	time.Sleep(time.Millisecond)
	fmt.Println(lock.RLockFunc(func() any { return x }))

	// Output:
	// 10000
}
```
