---
layout:     post
title:      The Usage of SuperStr
categories: Java
---

## slice
Just like the **slice** methon in JavaScript

```java
	// delete characters
	assertEquals(SuperStr.slice("mazhiyuan", 0, 1), "azhiyuan");
    assertEquals(SuperStr.slice("mazhiyuan", 0, 10), "");
    assertEquals(SuperStr.slice("mazhiyuan", 0, 0), "mazhiyuan");
    assertEquals(SuperStr.slice("mazhiyuan", 3, 2), "mazyuan");

    //insert or replace characters
    assertEquals(SuperStr.slice("mazhiyuan", 4, 3, "b"), "mazhban");
    assertEquals(SuperStr.slice("mazhiyuan", 4, 3, "b", "d", "w"), "mazhbdwan");
    assertEquals(SuperStr.slice("mazhiyuan", 4, 0, "b", "d"), "mazhbdiyuan");
```

## append

```java
	assertEquals(SuperStr.append("m", "a", "r", "k"), "mark");
    assertEquals(SuperStr.append("", "m", "a", "r", "k"), "mark");
```


## appendArray

```java
	assertEquals(SuperStr.append("m", "a", "r", "k"), "mark");
    assertEquals(SuperStr.append("", "m", "a", "r", "k"), "mark");
```

## at

```java
    assertTrue(SuperStr.at("mark", 2).get().equals('r'));
    assertSame(SuperStr.at("mark", 5), Optional.empty());
    assertSame(SuperStr.at("mark", -3).get(), 'a'); //Support negative index
```

## between

```java
    assertArrayEquals(SuperStr.between("[abc][cde]", "[", "]"), new String[]{"abc", "cde"});
    assertArrayEquals(SuperStr.between("[abc]f[cde]", "[", "]"), new String[]{"abc", "cde"});
```

## collapseSpace

zip the n spaces into just 1 space

```java
	assertEquals(SuperStr.collapseSpace("mark     ma"), "mark ma");
    assertEquals(SuperStr.collapseSpace("    mark     ma   zhiyuan"), " mark ma zhiyuan");
```

## contains

```java
	assertTrue(SuperStr.contains("mark", "m"));
	assertFalse(SuperStr.contains("mark", "M"));  // case sensitive
	assertTrue(SuperStr.contains("mark", "M", false)); // non case sensitive
```

## countStr

```java
 	assertSame(SuperStr.countStr("maaaaaaaaaark", "Aa", false, false), 5); // non case sensitive and without allow overlapping
    assertSame(SuperStr.countStr("maaaaaaaaaark", "Aa", false, true), 9); // non case sensitive and with allow overlapping
```

## isBlank

String contains spaces only

```java
	assertFalse(SuperStr.isBlank(" a "));
	assertTrue(SuperStr.isBlank(" "));
	assertFalse(SuperStr.isBlank(""));
	assertFalse(SuperStr.isBlank(null));
```


## insert

```java
	assertEquals(SuperStr.insert("mk ma", "ar", 1), "mark ma");
	assertEquals(SuperStr.insert("mk ma", "ar", 6), "mk ma"); // if index > value.length, return the value 
```

## left, mid and right Trim

```java
	//left
	assertEquals(SuperStr.leftTrim("      mark"), "mark");
	assertEquals(SuperStr.leftTrim("      mark    "), "mark    ");
	assertEquals(SuperStr.leftTrim("      mar k"), "mar k");

	//right
	assertEquals(SuperStr.rightTrim("mark    "), "mark");
	assertEquals(SuperStr.rightTrim("      mark    "), "      mark");
	assertEquals(SuperStr.rightTrim("mar k      "), "mar k");

	//mid
	assertEquals(SuperStr.midTrim("mark    "), "mark    ");
	assertEquals(SuperStr.midTrim("mar k      "), "mark      ");

```
## isNotBlank

```java
	assertTrue(SuperStr.isNotBlank("  a"));
	assertFalse(SuperStr.isNotBlank("  "));
	assertFalse(SuperStr.isNotBlank(null));
	assertFalse(SuperStr.isNotBlank(""));
```

## trims

remove all the non-blank String in an Array

```java
	assertArrayEquals(SuperStr.trims(new String[]{"ma", "rk", null, "", "  "}), new String[]{"ma", "rk"});
```

## reverse

```java
	assertEquals(SuperStr.reverse("mark"), "kram");
```

## removeNonWords

```java
	assertEquals(SuperStr.removeNonWords("ma^%$rk"), "mark");
```