- [1. Extract Drawable Name](#1-extract-drawable-name)

## 1. Extract Drawable Name

```kt
fun extractDrawableName(fullPath: String): String {
    return fullPath.substringAfter("drawable/")
}
```
