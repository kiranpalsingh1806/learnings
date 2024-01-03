- [PostgreSQL](#postgresql)
  - [1. Create Quiz Table](#1-create-quiz-table)
  - [2. GET - Questions List](#2-get---questions-list)
  - [3. POST - Add New Question](#3-post---add-new-question)
  - [4. PUT - Edit Question Data](#4-put---edit-question-data)
  - [5. DELETE - Delete Question by ID](#5-delete---delete-question-by-id)

# PostgreSQL

## 1. Create Quiz Table

```sh
CREATE TABLE quiz (
    id INT IDENTITY(1,1) PRIMARY KEY,
    question VARCHAR(255),
    answer VARCHAR(50),
    options VARCHAR(MAX),
    answer_id INTEGER,
    topic VARCHAR(100),
    subtopic VARCHAR(100),
    added_at BIGINT
);
```

## 2. GET - Questions List

```js
const sqlQuery = `SELECT *
        FROM quiz
        `
const result = await db.query(sqlQuery)
```

## 3. POST - Add New Question

```js
{
    "question": "This is my question",
    "answer": "thisanswer",
    "options": [
        "Building",
        "Animal",
        "Nature",
        "Pride"
    ],
    "answer_id": 1,
    "topic": "Great",
    "subtopic": "Very Great"
}
```

```js
const optionsString = options.join(', ')
const insertQuery = `
          INSERT INTO quiz (question, answer, options, answer_id, topic, subtopic, added_at )
          VALUES ($1, $2, $3, $4, $5, $6, $7)
        `
await db.none(insertQuery, [question, answer, optionsString, answer_id, topic, subtopic, added_at])
```

## 4. PUT - Edit Question Data

```js
{
    "quizId": 5,
    "topic": "Gruuuuuut"
}
```

```js
const updateQuery = `
            UPDATE quiz
            SET 
                question = COALESCE($1, question),
                answer = COALESCE($2, answer),
                options = COALESCE($3, options),
                answer_id = COALESCE($4, answer_id),
                topic = COALESCE($5, topic),
                subtopic = COALESCE($6, subtopic)
            WHERE id = $7
        `

await db.none(updateQuery, [question, answer, optionsString, answer_id, topic, subtopic, quizId])
```

## 5. DELETE - Delete Question by ID

```js
const sqlQuery = `DELETE FROM quiz
        where id = $1
        `
await db.query(sqlQuery, [quizId])
```
