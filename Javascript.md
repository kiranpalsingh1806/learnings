- [1. Convert Sentence and Join with dash](#1-convert-sentence-and-join-with-dash)
- [2. Generate random number](#2-generate-random-number)
- [3. Split and Capitalize](#3-split-and-capitalize)

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

## 3. Split and Capitalize

```js
function splitAndCapitalize(input) {
    // Split the string by underscores
    const words = input.split('_');
    
    // Capitalize the first letter of each word
    const capitalizedWords = words.map(word => word.charAt(0).toUpperCase() + word.slice(1));
    
    // Join the words back together with a space in between
    return capitalizedWords.join(' ');
}

// Example usage:
const input = 'you_have_a_microsoft_sharepoint_online_site';
const result = splitAndCapitalize(input);
console.log(result); // Output: "You Have A Microsoft Sharepoint Online Site"

```