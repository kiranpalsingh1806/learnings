- [1. Extract Drawable Name](#1-extract-drawable-name)
- [2. Icon button](#2-icon-button)
- [3. Format Milliseconds - xm ys](#3-format-milliseconds---xm-ys)

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

## 3. Format Milliseconds - xm ys

```kt
fun formatMilliseconds(milliseconds: Long): String {
    val seconds = milliseconds / 1000
    val minutes = seconds / 60
    val remainingSeconds = seconds % 60
    return "${minutes}m ${remainingSeconds}s"
}
```