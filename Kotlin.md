- [1. Button Color in Kotlin](#1-button-color-in-kotlin)
- [2. Box with Gradient Color](#2-box-with-gradient-color)
- [3. Span Item in Lazy Vertical Grid](#3-span-item-in-lazy-vertical-grid)

## 1. Button Color in Kotlin

```kt
 Button(
    onClick = {
        // Handle click
    },
    colors = ButtonDefaults.buttonColors(
        backgroundColor = colorResource(id = R.color.my_orange_1)
    )
) {
    Text(text = "Done")
}
```

## 2. Box with Gradient Color

```kt
Box(
    modifier = Modifier
        .background(
            brush = Brush.verticalGradient(
                colors = listOf(
                    Color(0xFF003366), // Dark Blue
                    Color(0xFF001a33) // Darker Blue
                ),
                startY = 0f,
                endY = 600f
            )
        )
        .padding(16.dp)
)
```

## 3. Span Item in Lazy Vertical Grid

```kt
 LazyVerticalGrid(
    GridCells.Fixed(2),
    modifier = Modifier.padding(top = 130.dp, start = 8.dp, end = 8.dp, bottom = 72.dp)
) {
    item (span = { GridItemSpan(2) })
}
```
