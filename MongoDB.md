# MongoDB

- [MongoDB](#mongodb)
  - [1. Date and Time Aggregation](#1-date-and-time-aggregation)
  - [2. First and Last Document in Group Stage](#2-first-and-last-document-in-group-stage)
  - [3. Project one of valid options using $ifNull](#3-project-one-of-valid-options-using-ifnull)
  - [4. Set Custom Keys in MongoDB](#4-set-custom-keys-in-mongodb)
  - [5. Match Documents with Array elements](#5-match-documents-with-array-elements)
  - [6. Regular Expression in MongoDB](#6-regular-expression-in-mongodb)
  - [7. Adding Fields based on conditions](#7-adding-fields-based-on-conditions)
  - [8. Using substrings](#8-using-substrings)
  - [9. Convert String to ObjectId in $lookup](#9-convert-string-to-objectid-in-lookup)
  - [10. Access ith index of array in $addFields](#10-access-ith-index-of-array-in-addfields)
  - [11. Convert ObjectId to string in $lookup](#11-convert-objectid-to-string-in-lookup)
  - [12. Group based on scores](#12-group-based-on-scores)
  - [13. Add Rank field using $setWindowFields](#13-add-rank-field-using-setwindowfields)

## 1. Date and Time Aggregation

```js
;[
    {
        $group: {
            _id: {
                month: {
                    $month: {
                        $toDate: '$endTime'
                    }
                },
                monthDay: {
                    $dayOfMonth: {
                        $toDate: '$endTime'
                    }
                },
                weekDay: {
                    $dayOfWeek: {
                        $toDate: '$endTime'
                    }
                },
                week: {
                    $week: {
                        $toDate: '$endTime'
                    }
                }
            },
            total: {
                $sum: '$noOfDrinks'
            }
        }
    },
    {
        $project: {
            _id: 0,
            month: '$_id.month',
            monthDay: '$_id.monthDay',
            week: '$_id.week',
            weekDay: '$_id.weekDay',
            total: '$total'
        }
    },
    {
        $sort: {
            week: -1
        }
    }
]
```

## 2. First and Last Document in Group Stage

```js
;[
    {
        $match: {
            uid: uid
        }
    },
    {
        $addFields: {
            totalTime: {
                $subtract: ['$endTime', '$startTime']
            }
        }
    },
    {
        $sort: {
            totalTime: -1
        }
    },
    {
        $group: {
            _id: null,
            longestStreak: {
                $first: '$$ROOT'
            },
            currentStreak: {
                $last: '$$ROOT'
            }
        }
    }
]
```

## 3. Project one of valid options using $ifNull

```sh
[
    {
        '$match': {
            '_id': 'uid'
        }
    }, {
        '$project': {
            '_id': 0,
            'country': {
                '$ifNull': [
                    '$ios.deviceDetails.deviceCountryIos', '$android.deviceDetails.deviceCountryAndroid'
                ]
            }
        }
    }
]
```

## 4. Set Custom Keys in MongoDB

```js
const filterUser = {
    _id: uid
}

const KEY = `forcedSwitches.${switchName}`

const updateSwitch = {
    $set: {
        [KEY]: flag
    }
}

User.updateOne(filterUser, updateSwitch)
```

## 5. Match Documents with Array elements

```js
const aggregationLogic = [
    {
        $match: {
            'postTag.0': {
                $exists: true
            }
        }
    },
    {
        $match: {
            $expr: {
                $in: ['#ChristmasContest', '$postTag']
            }
        }
    },
    {
        $count: 'count'
    }
]
```

## 6. Regular Expression in MongoDB

```js
const aggregationLogic = [
    {
        $match: {
            userEmail: /nopotest/
        }
    },
    {
        $count: 'count'
    }
]
```

```js
const aggregationLogic = [
    {
        $match: {
            email: {
                $not: /test/
            }
        }
    }
]
```

```js
// Will find documents in which question contains Play, plaY, PLAY, play
const aggregationLogic = [
    {
        $match: {
            question: {
                $regex: /.*play.*/i
            }
        }
    }
]
```

## 7. Adding Fields based on conditions

```js
;[
    {
        $project: {
            _id: 0,
            partner: '$accountability_partner.partner'
        }
    },
    {
        $addFields: {
            partner: {
                $cond: {
                    if: {
                        $gt: ['$partner', null]
                    },
                    then: '$partner',
                    else: ''
                }
            }
        }
    }
]
```

## 8. Using substrings

```js
;[
    {
        $project: {
            item: 1,
            yearSubstring: { $substr: ['$quarter', 0, 2] },
            quarterSubtring: { $substr: ['$quarter', 2, -1] }
        }
    }
]
```

## 9. Convert String to ObjectId in $lookup

```js
;[
    {
        $lookup: {
            from: 'Questions',
            let: {
                questionId: {
                    $toObjectId: '$questionId'
                }
            },
            pipeline: [
                {
                    $match: {
                        $expr: {
                            $eq: ['$_id', '$$questionId']
                        }
                    }
                }
            ],
            as: 'Question'
        }
    }
]
```

## 10. Access ith index of array in $addFields

```js
;[
    {
        $addFields: {
            question: {
                $arrayElemAt: ['$Question', 0]
            }
        }
    }
]
```

## 11. Convert ObjectId to string in $lookup

```js
;[
    {
        $lookup: {
            from: 'Submissions',
            let: {
                contestId: '$_id',
                userId: '$userId'
            },
            pipeline: [
                {
                    $match: {
                        $expr: {
                            $and: [
                                {
                                    $eq: [
                                        '$contestId',
                                        {
                                            $toString: '$$contestId'
                                        }
                                    ]
                                },
                                {
                                    $eq: [uid, '$uid']
                                }
                            ]
                        }
                    }
                }
            ],
            as: 'submissions'
        }
    }
]
```

## 12. Group based on scores

```js
;[
    {
        $group: {
            _id: '$uid',
            score: {
                $sum: {
                    $cond: [
                        {
                            $eq: ['$userAnswer', '$answer']
                        },
                        '$correctScore',
                        {
                            $subtract: [0, '$incorrectScore']
                        }
                    ]
                }
            }
        }
    }
]
```

## 13. Add Rank field using $setWindowFields

```js
;[
    {
        $sort: {
            score: -1
        }
    },
    {
        $setWindowFields: {
            sortBy: {
                score: -1
            },
            output: {
                rank: {
                    $rank: {}
                }
            }
        }
    }
]
```
