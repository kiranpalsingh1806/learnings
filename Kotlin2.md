- [1. Extract Drawable Name](#1-extract-drawable-name)
- [2. Icon button](#2-icon-button)

## 1. Extract Drawable Name

```kt
fun extractDrawableName(fullPath: String): String {
    return fullPath.substringAfter("drawable/")
}
```

## 2. Icon button
```kt
IconButton(onClick = onPrivacyClick) {
    Icon(
        imageVector = Icons.Default.List,
        contentDescription = "Privacy Policy",
        tint = Color.White
    )
}
```