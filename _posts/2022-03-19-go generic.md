---
layout: post
title: go generic 
published: true
categories: post
---
 

```go
package main

import (
	"fmt"
	"golang.org/x/exp/constraints"
	"hash/fnv"
	"strconv"
	"strings"
	"testing"
)

type Integer interface {
	int | int8 | int16 | int32 | int64
}

func min[T Integer](a, b T) T {
	if a < b {
		return a
	}

	return b
}

func min2[T constraints.Ordered](a, b T) T {
	if a < b {
		return a
	}

	return b
}

func isSame[T comparable](a, b T) bool {
	if a == b {
		return a == b
	} else if a != b {
		return b != a
	}

	return false
}

type Stringer interface {
	Integer
	String() string
}

func Print[T Integer](a T) T {

	return a
}

func String[T Integer](a T) T {

	return a
}

func TestA(t *testing.T) {
	m1 := min(3, 5)

	fmt.Println(m1)

	m2 := min2(3, 5)

	fmt.Println(m2)

	m3 := isSame(3, 5)

	fmt.Println(m3)

	s1 := String(4)

	fmt.Println(s1)

	var ms1 MyString = "Hello"
	var ms2 MyString = "World"

	fmt.Println(Equal(ms1, ms2))

	mapTest()

}

type ComparableHasher interface {
	comparable // ==, !=
	Hash() uint32
}

type MyString string

func (s MyString) Hash() uint32 {
	h := fnv.New32()
	h.Write([]byte(s))
	return h.Sum32()
}

func Equal[T ComparableHasher](a, b T) bool {
	if a == b {
		return true
	}
	return a.Hash() == b.Hash()
}

func Map[F, T any](s []F, f func(F) T) []T {
	rst := make([]T, len(s))

	for i, v := range s {
		rst[i] = f(v)
	}

	return rst
}

func mapTest() {
	doubled := Map([]int{1, 2, 3}, func(v int) int {
		return v * 2
	})

	fmt.Println(doubled)

	uppered := Map([]string{"hello", "world", "abc"}, func(v string) string {
		return strings.ToUpper(v)
	})

	fmt.Println(uppered)

	toString := Map([]int{1, 2, 3}, func(v int) string {
		return "str" + strconv.Itoa(v)

	})

	fmt.Println(toString)
}

type Node[T any] struct {
	val  T
	next *Node[T]
}

func (n *Node[T]) Push(v T) *Node[T] {
	node := NewNode(v)
	n.next = node
	return node
}

func NewNode[T any](v T) *Node[T] {
	return &Node[T]{val: v}
}

func TestNode(t *testing.T) {

	node1 := NewNode(1)
	node1.Push(2).Push(3).Push(4)

	for node1 != nil {
		fmt.Print(node1.val, " - ")
		node1 = node1.next
	}
	fmt.Println()

	fmt.Println()

	node2 := NewNode("Hi")
	node2.Push("hello").Push("How are you")

	for node2 != nil {
		fmt.Print(node2.val, " - ")
		node2 = node2.next
	}
}
```



generic slices package 
-------------

```go

package main

import (
	"fmt"
	"testing"

	"golang.org/x/exp/slices"
)

func TestSlice1(t *testing.T) {
	a := []int{1, 2, 3, 4, 5, 6, 7}

	contains := slices.Contains(a, 5)
	fmt.Printf("slices.Cotains => %t", contains)

}

func Test_BinarySearch(t *testing.T) {

	//constraints.Ordered
	var ordered = []int{1, 2, 3, 4, 5, 6, 7}

	//인덱스 번호가 반환된다
	idx := slices.BinarySearch(ordered, 6)

	fmt.Println(idx) //5

	var orderedStrings = []string{"a", "b", "c", "d", "e", "f", "g"}

	idx2 := slices.BinarySearch(orderedStrings, "c")

	fmt.Println(idx2) //2
}
func Test_BinarySearchFunc(t *testing.T) {

}

//Unused Capacity from slice
func Test_Clip(t *testing.T) {

	src := []string{"a", "b", "c", "d", "e", "f", "g"}

	var strs = make([]string, 10)

	for i, s := range src {
		strs[i] = s
	}

	fmt.Printf("strs => %s,  capacity => %d  length => %d \n", strs, cap(strs), len(strs))

	strs = strs[0:3]

	cliped := slices.Clip(strs)

	fmt.Printf("cliped => %s,  capacity => %d length => %d \n", cliped, cap(cliped), len(cliped))

}
func Test_Clone(t *testing.T) {

}
func Test_Compact(t *testing.T) {
	list := []string{"a", "b", "a", "b", "c", "d"}

	intList := []int{1, 2, 3, 4, 5, 1, 4, 7}

	slices.Compact(list)
	fmt.Println(list)

	slices.Compact(intList)

	fmt.Printf("intList ==> %v \n", intList)

	intList2 := []int{1, 1, 2, 2, 3, 3, 4, 7}

	slices.Compact(intList2)
	fmt.Printf("intList2 ==> %v \n", intList2)

	intList3 := []int{1, 1, 1, 3, 3, 3, 4, 4, 8}

	slices.Compact(intList3)
	fmt.Printf("intList3 ==> %v \n", intList3)
}

func Test_CompactFunc(t *testing.T) {}
func Test_Compare(t *testing.T)     {}
func Test_CompareFunc(t *testing.T) {}
func Test_Contains(t *testing.T)    {}
func Test_Delete(t *testing.T)      {}
func Test_Equal(t *testing.T)       {}
func Test_EqualFunc(t *testing.T)   {}
func Test_Grow(t *testing.T)        {}
func Test_Index(t *testing.T) {

	list := []string{"a", "b", "c", "d", "e", "f", "g"}

	//list 에서 "g" 의 인덱스를 구한다.
	idx := slices.Index(list, "g")

	fmt.Println(idx) // 6
}
func Test_IndexFunc(t *testing.T)      {}
func Test_Insert(t *testing.T)         {}
func Test_IsSorted(t *testing.T)       {}
func Test_IsSortedFunc(t *testing.T)   {}
func Test_Sort(t *testing.T)           {}
func Test_SortFunc(t *testing.T)       {}
func Test_SortStableFunc(t *testing.T) {}



```



generic constraints
-------------




```golang

package main

import (
	"golang.org/x/exp/constraints"
	"testing"
)

type aaa interface {
	constraints.Ordered // < <= >= >.
}

type b interface {
	comparable
}

type c interface {
	constraints.Ordered
}

type d interface {
	constraints.Ordered
}

func TestSlice1(t *testing.T) {

}




```
