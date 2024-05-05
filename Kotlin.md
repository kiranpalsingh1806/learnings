- [1. Button Color in Kotlin](#1-button-color-in-kotlin)
- [2. Box with Gradient Color](#2-box-with-gradient-color)
- [3. Span Item in Lazy Vertical Grid](#3-span-item-in-lazy-vertical-grid)
- [4. Retrofit](#4-retrofit)
- [5. Custom Card](#5-custom-card)
- [6. Circular Progress](#6-circular-progress)

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

## 4. Retrofit

```kt
@GET("api/gkinfact/v2/facts/daily")
suspend fun getGeneralKnowledgeFactsV2(@Query("page") page: Int): Response<ApiResponse>
```

```kt
@GET("api/user/contests/new/{uid}")
suspend fun getNewUpcomingListForUserByID(@Path("uid") userId: String): Response<GetContestsListForUserResponse>
```

## 5. Custom Card

![Custom Card Example](/images/kotlin_1.png)

```kt

@Composable
fun CustomCard(
    header: String,
    text1: String,
    text2: String,
    score: Int,
    total: Int,
    timeInMs: Long,
    modifier: Modifier = Modifier
) {
    Card(
        modifier = modifier,
        shape = RoundedCornerShape(8.dp),
        backgroundColor = Color.White,
        elevation = 4.dp
    ) {
        Column(
            modifier = Modifier.padding(16.dp)
        ) {
            Row(
                modifier = Modifier.fillMaxWidth(),
                horizontalArrangement = Arrangement.SpaceBetween,
                verticalAlignment = Alignment.CenterVertically
            ) {
                Text(
                    text = header,
                    style = MaterialTheme.typography.h6
                )
            }
            Spacer(modifier = Modifier.height(8.dp))
            Text(
                text = text1,
                style = MaterialTheme.typography.body1,
                overflow = TextOverflow.Ellipsis,
                maxLines = 2
            )
            Spacer(modifier = Modifier.height(4.dp))
            Text(
                text = text2,
                style = MaterialTheme.typography.body2,
                color = Color.Gray
            )
            Spacer(modifier = Modifier.height(10.dp))
            Text(
                text = "Score - $score / $total",
                style = MaterialTheme.typography.body2,
                color = Color.Gray
            )
            Spacer(modifier = Modifier.height(4.dp))
            LinearProgressIndicator(
                progress = score.toInt().coerceIn(0, total.toInt()) / total.toInt().toFloat(),
                modifier = Modifier
                    .fillMaxWidth()
                    .padding(3.dp)
                    .height(10.dp),
                color = colorResource(id = R.color.shamrockGreen)
            )
            Spacer(modifier = Modifier.height(8.dp))
            Text(
                text = "Submitted ${getTimeAgo(timeInMs)}",
                style = MaterialTheme.typography.body2,
                color = Color.Gray
            )
        }
    }
}
```

```kt
fun getTimeAgo(timeInMillis: Long): String {
    val currentTime = System.currentTimeMillis()
    val diff = currentTime - timeInMillis

    val seconds = TimeUnit.MILLISECONDS.toSeconds(diff)
    val minutes = TimeUnit.MILLISECONDS.toMinutes(diff)
    val hours = TimeUnit.MILLISECONDS.toHours(diff)
    val days = TimeUnit.MILLISECONDS.toDays(diff)

    return when {
        seconds < 60 -> "Just now"
        minutes < 60 -> "$minutes minutes ago"
        hours < 24 -> "$hours hours ago"
        else -> "$days days ago"
    }
}
```

## 6. Circular Progress

![Custom Card Example](/images/kotlin_2.png)

```kt
LazyColumn(
    verticalArrangement = Arrangement.spacedBy(20.dp),
    horizontalAlignment = Alignment.CenterHorizontally,
    modifier = Modifier.padding(horizontal = 8.dp)
) {
    item {
        Spacer(modifier = Modifier.height(20.dp))
    }
    item {
        val completionPercentage = ceil((courseStatus.value.attempted.toDouble() / courseStatus.value.total.toDouble()) * 100)
        Text(
            text = "Course Progress - $completionPercentage%",
            fontWeight = FontWeight.Bold,
            fontSize = 20.sp
        )
    }
    item {
        CustomComponent(
            indicatorValue = courseStatus.value.attempted,
            maxIndicatorValue = courseStatus.value.total
        )
    }
    item {
        Text(
            text = "Remaining Topics - ${courseStatus.value.total - courseStatus.value.attempted}",
            fontWeight = FontWeight.Bold,
            fontSize = 20.sp
        )
    }
}
```

```kt

@Composable
fun CustomComponent(
    canvasSize: Dp = 200.dp,
    indicatorValue: Int = 0,
    maxIndicatorValue: Int = 100,
    backgroundIndicatorColor: Color = MaterialTheme.colors.onSurface.copy(alpha = 0.1f),
    backgroundIndicatorStrokeWidth: Float = 100f,
    foregroundIndicatorColor: Color = colorResource(id = R.color.my_blue_1),
    foregroundIndicatorStrokeWidth: Float = 100f,
    bigTextFontSize: TextUnit = MaterialTheme.typography.body2.fontSize,
    bigTextColor: Color = MaterialTheme.colors.onSurface,
    bigTextSuffix: String = "topics",
    smallText: String = "Completed",
    smallTextFontSize: TextUnit = MaterialTheme.typography.body1.fontSize,
    smallTextColor: Color = MaterialTheme.colors.onSurface.copy(alpha = 0.3f)
) {
    var allowedIndicatorValue by remember {
        mutableStateOf(maxIndicatorValue)
    }
    allowedIndicatorValue = if (indicatorValue <= maxIndicatorValue) {
        indicatorValue
    } else {
        maxIndicatorValue
    }

    var animatedIndicatorValue by remember { mutableStateOf(0f) }
    LaunchedEffect(key1 = allowedIndicatorValue) {
        animatedIndicatorValue = allowedIndicatorValue.toFloat()
    }

    val percentage =
        (animatedIndicatorValue / maxIndicatorValue) * 100

    val sweepAngle by animateFloatAsState(
        targetValue = (2.4 * percentage).toFloat(),
        animationSpec = tween(1000)
    )

    val receivedValue by animateIntAsState(
        targetValue = allowedIndicatorValue,
        animationSpec = tween(1000)
    )

    val animatedBigTextColor by animateColorAsState(
        targetValue = if (allowedIndicatorValue == 0)
            MaterialTheme.colors.onSurface.copy(alpha = 0.3f)
        else
            bigTextColor,
        animationSpec = tween(1000)
    )

    Box(
        modifier = Modifier.fillMaxSize(),
        contentAlignment = Alignment.Center
    ) {
        Column(
            modifier = Modifier
                .size(canvasSize)
                .drawBehind {
                    val componentSize = size / 1.25f
                    backgroundIndicator(
                        componentSize = componentSize,
                        indicatorColor = backgroundIndicatorColor,
                        indicatorStrokeWidth = backgroundIndicatorStrokeWidth,
                    )
                    foregroundIndicator(
                        sweepAngle = sweepAngle,
                        componentSize = componentSize,
                        indicatorColor = foregroundIndicatorColor,
                        indicatorStrokeWidth = foregroundIndicatorStrokeWidth,
                    )
                },
            verticalArrangement = Arrangement.Center,
            horizontalAlignment = Alignment.CenterHorizontally
        ) {
            EmbeddedElements(
                bigText = receivedValue,
                bigTextFontSize = bigTextFontSize,
                bigTextColor = animatedBigTextColor,
                bigTextSuffix = bigTextSuffix,
                smallText = smallText,
                smallTextColor = smallTextColor,
                smallTextFontSize = smallTextFontSize
            )
        }
    }
}

fun DrawScope.backgroundIndicator(
    componentSize: Size,
    indicatorColor: Color,
    indicatorStrokeWidth: Float,
) {
    drawArc(
        size = componentSize,
        color = indicatorColor,
        startAngle = 150f,
        sweepAngle = 240f,
        useCenter = false,
        style = Stroke(
            width = indicatorStrokeWidth,
            cap = StrokeCap.Round
        ),
        topLeft = Offset(
            x = (size.width - componentSize.width) / 2f,
            y = (size.height - componentSize.height) / 2f
        )
    )
}

fun DrawScope.foregroundIndicator(
    sweepAngle: Float,
    componentSize: Size,
    indicatorColor: Color,
    indicatorStrokeWidth: Float,
) {
    drawArc(
        size = componentSize,
        color = indicatorColor,
        startAngle = 150f,
        sweepAngle = sweepAngle,
        useCenter = false,
        style = Stroke(
            width = indicatorStrokeWidth,
            cap = StrokeCap.Round
        ),
        topLeft = Offset(
            x = (size.width - componentSize.width) / 2f,
            y = (size.height - componentSize.height) / 2f
        )
    )
}

@Composable
fun EmbeddedElements(
    bigText: Int,
    bigTextFontSize: TextUnit,
    bigTextColor: Color,
    bigTextSuffix: String,
    smallText: String,
    smallTextColor: Color,
    smallTextFontSize: TextUnit
) {
    Text(
        text = smallText,
        color = smallTextColor,
        fontSize = smallTextFontSize,
        textAlign = TextAlign.Center
    )
    Text(
        text = "$bigText ${bigTextSuffix.take(6)}",
        color = bigTextColor,
        fontSize = bigTextFontSize,
        textAlign = TextAlign.Center,
        fontWeight = FontWeight.Bold
    )
}
```