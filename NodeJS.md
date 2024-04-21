- [1. Add Document](#1-add-document)
- [2. Get Document](#2-get-document)
- [3. Update Document](#3-update-document)
- [4. Delete Document](#4-delete-document)
- [5. Routes](#5-routes)
- [6. Mongoose Model](#6-mongoose-model)
- [7. Auth Controller](#7-auth-controller)
- [8. CRUD](#8-crud)

## 1. Add Document

```js
exports.addQuizQuestion = async (req, res) => {
    const { contestId, question, options, createdAt, answer } = req.body

    try {
        const filteredOptions = options.filter((option) => option && option.value && option.value.trim() !== '').map((option) => ({ ...option, value: option.value.trim() }))
        const newQuestion = new QuestionModel({ contestId, question, options: filteredOptions, createdAt, answer })
        await newQuestion.save()

        res.json({
            status: 200,
            message: 'success',
            data: {
                info: 'Question has been added'
            }
        })
    } catch (error) {
        res.status(500).json({
            status: 500,
            message: 'error',
            data: {
                info: error.message
            }
        })
    }
}
```

## 2. Get Document

```js
exports.getPlayerInfo = async (req, res) => {
    try {
        const { id } = req.params
        const player = await PlayerModel.findById(id)
        res.json({
            status: 200,
            message: 'success',
            data: {
                player,
                info: 'Player data has been fetched'
            }
        })
    } catch (err) {
        console.log('Error : ', err)
        res.status(500).json({
            status: 500,
            message: 'error',
            data: {
                info: 'Internal server error'
            }
        })
    }
}
```

## 3. Update Document

```js
exports.storeQuestionAttemptByUser = async (req, res) => {
    try {
        const { uid, questionId, contestId, userAnswer, correctScore, incorrectScore } = req.body
        await SubmissionModel.updateOne({ uid, questionId, contestId }, { uid, questionId, contestId, userAnswer, correctScore, incorrectScore, submittedAt: new Date().getTime() }, { upsert: true })
        updateParticipantsOfContest(contestId)
        res.json({
            status: 200,
            message: 'success',
            data: {
                info: 'Question has been submitted'
            }
        })
    } catch (error) {
        console.log('Error : ', error)
        res.status(500).json({
            status: 500,
            message: 'error',
            data: {
                info: error.message
            }
        })
    }
}
```

```js
exports.editQuizQuestion = async (req, res) => {
    const { contestId, question, options, createdAt, answer } = req.body

    try {
        // Filter options and parse correctScore and incorrectScore to numbers
        const filteredOptions = options
            .filter((option) => option && option.value && option.value.trim() !== '')
            .map((option) => ({
                value: option.value,
                correctScore: parseInt(option.correctScore),
                incorrectScore: parseInt(option.incorrectScore)
            }))

        // Find and update the question in the database
        const updatedQuestion = await QuestionModel.findByIdAndUpdate(req.params.id, { contestId, question, options: filteredOptions, createdAt, answer }, { new: true })

        // Update answer for submissions related to this question
        updateAnswerForSubmissions(req.params.id, contestId, answer)

        // Check if question was not found
        if (!updatedQuestion) {
            return res.status(400).json({
                status: 400,
                message: 'error',
                data: {
                    info: 'Question not found'
                }
            })
        }

        // Respond with success message
        res.json({
            status: 200,
            message: 'success',
            data: {
                info: 'Question updated successfully'
            }
        })
    } catch (error) {
        console.log('Error : ', error)
        res.status(500).json({
            status: 500,
            message: 'error',
            data: {
                info: error.message
            }
        })
    }
}
```

## 4. Delete Document

```js
exports.deleteQuizQuestion = async (req, res) => {
    try {
        const deletedQuestion = await QuestionModel.findByIdAndDelete(req.params.id)

        if (!deletedQuestion) {
            return res.status(400).json({
                status: 400,
                message: 'error',
                data: {
                    info: 'Question not found'
                }
            })
        }

        res.json({
            status: 200,
            message: 'success',
            data: {
                info: 'Question deleted successfully'
            }
        })
    } catch (error) {
        res.status(500).json({
            status: 500,
            message: 'error',
            datA: {
                info: error.message
            }
        })
    }
}
```

## 5. Routes

```js
const router = require('express').Router()
const seriesController = require('../Controller/seriesController')

router.get('/api/series', seriesController.getSeriesList)
router.post('/api/series', seriesController.addSeries)
router.put('/api/series/:id', seriesController.editSeries)
router.delete('/api/series/:id', seriesController.deleteSeries)

module.exports = router
```

## 6. Mongoose Model

```js
const mongoose = require('mongoose')

const QuestionSchema = new mongoose.Schema({
    contestId: {
        type: mongoose.Schema.Types.ObjectId,
        required: true
    },
    question: {
        type: String,
        required: true
    },
    options: [
        {
            value: {
                type: String,
                trim: true, // Automatically trim whitespace
                required: true
            }
            // Add other option properties if needed
        }
    ],
    createdAt: {
        type: Date,
        default: Date.now
    },
    answer: {
        type: String,
        required: true
    }
})

// Pre-save middleware to trim whitespace from question options
QuestionSchema.pre('save', function (next) {
    this.options.forEach((option) => {
        option.value = option.value.trim()
    })
    next()
})

module.exports = mongoose.model('Question', QuestionSchema)
```

```js
const mongoose = require('mongoose')

const QuestionSchema = new mongoose.Schema({
    contestId: {
        type: mongoose.Schema.Types.ObjectId,
        required: true
    },
    question: {
        type: String,
        required: true
    },
    options: {
        type: [String],
        required: true
    },
    createdAt: {
        type: Date,
        default: Date.now
    },
    answer: {
        type: String,
        required: true
    }
})

QuestionSchema.pre('save', function (next) {
    if (this.options && this.options.length > 0) {
        this.options = this.options.map((option) => option.trim())
    }
    next()
})

module.exports = mongoose.model('Question', QuestionSchema)
```

## 7. Auth Controller

```js
const router = require('express').Router()
const authController = require('../Controller/authController')

router.post('/api/user/auth', authController.signInHandler)
router.post('/api/user/account', authController.handleUserAccount)

module.exports = router
```

```js
const Joi = require('joi')

exports.signInHandler = async (req, res) => {
    try {
        const schema = Joi.object({
            uid: Joi.string().required(),
            name: Joi.string().required(),
            email: Joi.string().email().required(),
            createdAt: Joi.date().iso().required()
        })

        const { error } = schema.validate(req.body)
        if (error) {
            console.log('Joi Error:', error.message)
            return res.status(400).json({
                status: 400,
                message: 'error',
                data: { info: 'Error in request payload.' }
            })
        }

        const { uid, name, email, createdAt } = req.body
        const existingUser = await UserModel.findOne({ uid })

        if (!existingUser) {
            const newUser = new UserModel({
                uid,
                name,
                email,
                createdAt,
                coins: 0,
                className: 'First Year MBBS',
                college: 'None'
            })
            await newUser.save()
            return res.status(200).json({
                status: 200,
                message: 'success',
                data: { info: 'User has been added to records.' }
            })
        } else {
            return res.status(200).json({
                status: 200,
                message: 'success',
                data: { info: 'User already exists.' }
            })
        }
    } catch (error) {
        console.log('Error:', error.message)
        return res.status(500).json({
            status: 500,
            message: 'error',
            data: { info: 'Unexpected error occurred.' }
        })
    }
}

exports.handleUserAccount = async (req, res) => {
    try {
        const { uid, type } = req.body

        if (type === 'clearData') {
            await UserModel.updateOne({ uid }, { $set: { coins: 0, className: '', college: '' } })
        } else if (type === 'deleteAccount') {
            await UserModel.deleteOne({ uid })
            await deleteFirebaseUser(uid)
        }
        return res.json({
            status: 200,
            message: 'success',
            data: {
                info: `Action ${type} performed on ${uid}`
            }
        })
    } catch (err) {
        console.log('Error : ', err)
        return res.json({
            status: 400,
            message: 'error',
            data: { error: err }
        })
    }
}
```

```js
const { admin } = require('../Config/FirebaseAdmin.js')

exports.deleteFirebaseUser = async (uid) => {
    try {
        admin
            .auth()
            .deleteUser(uid)
            .then((response) => {
                console.log('Response : ', response)
                console.log(`Successfully deleted user with UID: ${uid}`)
            })
            .catch((error) => {
                console.error('Error deleting user:', error)
            })
        return true
    } catch (err) {
        console.log('Error : ', err)
        return false
    }
}
```

## 8. CRUD

```js
const mongoose = require('mongoose')

const QuestionSchema = new mongoose.Schema({
    question: {
        type: String,
        trim: true
    },
    type: {
        type: String,
        enum: ['Text', 'Image', 'Video'],
        default: 'text'
    },
    difficulty: {
        type: String,
        enum: ['Easy', 'Medium', 'Hard'],
        default: 'Easy'
    },
    imageURL: {
        type: String,
        default: ''
    },
    topic: {
        type: String,
        default: 'General'
    },
    videoURL: {
        type: String,
        default: ''
    },
    explanation: {
        type: String,
        trim: true
    },
    options: {
        type: [String],
        required: true
    },
    answer: {
        type: String,
        required: true,
        trim: true,
        validate: {
            validator: function (value) {
                return this.options.includes(value)
            },
            message: 'Answer must be one of the options'
        }
    },
    createdAt: {
        type: Number
    },
    updatedAt: {
        type: Number
    }
})

QuestionSchema.pre('save', function (next) {
    if (this.options && this.options.length > 0) {
        this.options = this.options.map((option) => option.trim())
    }
    next()
})

module.exports = mongoose.model('Questions', QuestionSchema, 'Questions')
```

```js
const QuestionModel = require('../Models/QuestionModel')
const Joi = require('joi')

exports.getQuestionsList = async (req, res) => {
    try {
        const questionsList = await QuestionModel.aggregate([
            {
                $sort: {
                    createdAt: -1
                }
            },
            {
                $limit: 100
            }
        ])
        res.json({
            status: 200,
            message: 'success',
            data: {
                questionsList,
                info: 'Question data has been fetched'
            }
        })
    } catch (err) {
        console.log('Error : ', err)
        res.status(500).json({
            status: 500,
            message: 'error',
            data: {
                info: 'Internal server error'
            }
        })
    }
}
exports.addNewQuestion = async (req, res) => {
    const { question, topic, options, answer, type, imageURL, explanation, difficulty, videoURL } = req.body

    try {
        const questionSchema = Joi.object({
            question: Joi.string().required().messages({
                'string.empty': 'Question is required'
            }),
            options: Joi.array().items(Joi.string().required()).min(2).required().messages({
                'array.base': 'Options must be an array',
                'array.min': 'At least two options are required',
                'any.required': 'Options are required'
            }),
            answer: Joi.string().trim().required().messages({
                'string.empty': 'Answer is required'
            }),
            type: Joi.string().valid('Text', 'Image', 'Video').default('text').messages({
                'any.only': 'Type value is not valid'
            }),
            imageURL: Joi.string().allow('').default('').messages({
                'string.empty': 'Image URL must be a string'
            }),
            explanation: Joi.string().trim().allow('').default('').messages({
                'string.empty': 'Explanation must be a string'
            }),
            topic: Joi.string().trim().allow('').default('').messages({
                'string.empty': 'Topic must be a string'
            }),
            difficulty: Joi.string().valid('Easy', 'Medium', 'Hard').required().messages({
                'any.only': 'Difficulty must be one of: Easy, Medium, Hard'
            }),
            videoURL: Joi.string().allow('').default('').messages({
                'string.empty': 'Video URL must be a string'
            })
        })

        const { error } = questionSchema.validate(req.body)
        if (error) {
            return res.status(400).json({
                status: 400,
                message: 'error',
                data: {
                    info: error.details[0].message
                }
            })
        }

        const newQuestion = new QuestionModel({
            question,
            topic,
            options,
            createdAt: new Date().getTime(),
            updatedAt: new Date().getTime(),
            answer,
            type,
            imageURL,
            explanation,
            difficulty,
            videoURL
        })

        await newQuestion.save()

        res.json({
            status: 200,
            message: 'success',
            data: {
                info: 'Question has been added'
            }
        })
    } catch (error) {
        res.status(500).json({
            status: 500,
            message: 'error',
            data: {
                info: error.message
            }
        })
    }
}

exports.editQuestion = async (req, res) => {
    try {
        const { questionId } = req.params
        const { question, options, answer, type, imageURL, explanation } = req.body

        // Validate input
        const questionSchema = Joi.object({
            question: Joi.string().required().messages({
                'string.empty': 'Question is required'
            }),
            options: Joi.array().items(Joi.string().required()).min(2).required().messages({
                'array.base': 'Options must be an array',
                'array.min': 'At least two options are required',
                'any.required': 'Options are required'
            }),
            answer: Joi.string().trim().required().messages({
                'string.empty': 'Answer is required'
            }),
            type: Joi.string().valid('text', 'image').default('text').messages({
                'any.only': 'Type value is not valid'
            }),
            imageURL: Joi.string().allow('').default('').messages({
                'string.empty': 'Image URL must be a string'
            }),
            explanation: Joi.string().trim().allow('').default('').messages({
                'string.empty': 'Explanation must be a string'
            })
        })
        const { error } = questionSchema.validate(req.body)
        if (error) {
            return res.status(400).json({
                status: 400,
                message: 'error',
                data: {
                    info: error.details[0].message
                }
            })
        }

        // Update question
        const updatedQuestion = await QuestionModel.findByIdAndUpdate(
            questionId,
            {
                question,
                options,
                answer,
                type,
                imageURL,
                explanation
            },
            { new: true }
        )

        if (!updatedQuestion) {
            return res.status(404).json({
                status: 404,
                message: 'error',
                data: {
                    info: 'Question not found'
                }
            })
        }

        res.json({
            status: 200,
            message: 'success',
            data: {
                info: 'Question has been updated',
                question: updatedQuestion
            }
        })
    } catch (error) {
        res.status(500).json({
            status: 500,
            message: 'error',
            data: {
                info: error.message
            }
        })
    }
}

exports.deleteQuestion = async (req, res) => {
    try {
        const { id } = req.params
        const deletedQuestion = await QuestionModel.findByIdAndDelete(id)

        if (!deletedQuestion) {
            return res.status(404).json({
                status: 404,
                message: 'error',
                data: {
                    info: 'Question not found'
                }
            })
        }

        res.json({
            status: 200,
            message: 'success',
            data: {
                info: 'Question has been deleted',
                question: deletedQuestion
            }
        })
    } catch (error) {
        res.status(500).json({
            status: 500,
            message: 'error',
            data: {
                info: error.message
            }
        })
    }
}
```
