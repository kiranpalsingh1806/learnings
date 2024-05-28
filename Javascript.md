- [1. Convert Sentence and Join with dash](#1-convert-sentence-and-join-with-dash)
- [2. Generate random number](#2-generate-random-number)

## 1. Convert Sentence and Join with dash

```js
function convertSentence(sentence) {
    const cleanSentence = sentence.replace(/[^a-z\s]/gi, '')
    const words = cleanSentence.split(/\s+/)
    let convertedSentence = words.map((word) => word.toLowerCase()).join('-')
    if (convertedSentence.endsWith('-')) {
        convertedSentence = convertedSentence.slice(0, -1)
    }

    return convertedSentence
}
```

## 2. Generate random number

```js
function generateRandomNumber() {
    const randomNumber = Math.random()
    const randomNumberBetween1And100 = Math.floor(randomNumber * 1000) + 1
    return randomNumberBetween1And100
}
```
