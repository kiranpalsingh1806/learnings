- [1. Button Color in Kotlin](#1-button-color-in-kotlin)

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
