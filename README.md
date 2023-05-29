Dog Finder
==========


Task
----

Your task is to develop the **Dog finder** app in React.  
This app has to:
  - Fetch a list of dogs from a [Fake API](#fake-api-description)
  - Display:
    - a **Title** in a header
    - a **Filter Builder** component
    - a filtered **List** of Dogs (or all the dogs if there are no valid filters)
    - a **Dog Counter** showing how many dogs are found (eg: `5 dogs found`)

![description](https://github.com/Tyrceo/Dog-Finder-Instructions/assets/2552413/660a0771-4cc2-4a5b-8671-0926fae77fb6)

The Filter Builder should allow the creation of filter groups AND'd together, and within each group you can have multiple filters OR'd together to form the final filter to apply on the array of dogs returned by the [Fake API](#fake-api-description) (see [Filtering Algorithm](#filtering-algorithm) )

There are two types of filter:
  - Categorical, for the following fields:
    - gender
    - eyeColor
    - tags
  - Quantitative, for the following fields:
    - price
    - age

Here's a walkthrough of the app:

https://github.com/Tyrceo/Dog-Finder-Instructions/assets/2552413/a848f389-e6a1-4a97-81cc-a04a284c96d3


Getting Started
---------------

### Server

setup the Fake API server
```sh
git clone https://github.com/Tyrceo/Dog-Finder-Server.git
cd Dog-Finder-Server
npm install
npm run server
```

It should launch a server accessible on http://localhost:4000  
This will be the endpoint for the API

If you encounter any issue, you can use your own solution, the important thing is to provide `Dog-Finder-Server/db.json` via http for your client to consume

### Client

- [create a vite project](https://vitejs.dev/guide/#scaffolding-your-first-vite-project)
using your package manager of choice
- name the project however you want
- choose **React** as a framework
- You can choose either typescript or javascript (with or without SWC)

![create_vite](https://github.com/Tyrceo/Dog-Finder-Instructions/assets/2552413/139d0cbf-4151-4894-b987-a8144adcaa94)

You should be ready to start solving this task

You can use any library you think could help you.


Breakdown of the various parts
------------------------------

### Title
should be `Dog Finder`

---

### Dog counter
Shows how many dogs are displayed  
examples:  
  `1 dog found`  
  `5 dogs found`  
  `No dogs found`  

---

### Filter Builder component
This is the interactive part of the application.

This component initially has only one button: `Add Group`

#### Filter Group component

Clicking on `Add Group` will create an Filter Group component initialized with one Filter component  
Thus, it contains:
 - A Filter component
 - A `Add Filter` button to add a new Filter to that group
 - A `Delete Group` button to remove the entire Filter Group


#### Filter component

A filter could be in 2 different states:
- _Undefined_: the user has yet to choose a field to filter
- _Defined_: a field is chosen, displaying the filtering criteria

Thus, a filter should have at least:
 - a field selector
 - a delete button `x`

There are two type of _defined_ filter:

#### Categorical
Used to filter categorical values (ie: gender -> male or female)  
Apart from the field selector and delete button, it should also display a `category` selector

#### Quantitative
Used to filter numerical values (ie: age between 3 and 6 year-old)  
Apart from the field selector and delete button, it should also display a `min` and `max` selector.

`min` and `max` selector should contain numerical options with steps that make sense.
for example, if you have values ranging from 100 to 1000 step of 50 or 100 would make sense whereas a step of 1 would be overkill

---

### List of Dogs
Each time the Filter Builder component is updated (add/remove/modify filter)  
The dog list is filtered using those FilterGroups and the result is displayed  

If there are no valid filtering criteria, the full list is returned


Filtering Algorithm
-------------------
Each Filter in each Filter Group are OR'd together, and each Filter Group are AND'd together

example:

- Filter Group 1:
  -   Filter 1: Age between 1 and 3
  -   Filter 2: Age between 13 and 15
- Filter Group 2:
  -   Filter 1: Price between 1000 and 1100
  -   Filter 2: Tags is calm
- Filter Group 3:
  -   Filter 1: Gender is male
- Filter Group 4:
  -   Filter 1: Eye Color is brown


will do the equivalent of:
```javascript
dogs.filter(
  dog => (
    (dog.age >= 1 && dog.age <= 3)            // quantitative
    || (dog.age >= 13 && dog.age <= 15)       // quantitative
  ) && (    
    (dog.price >= 1000 && dog.price <= 1400)  // quantitative
    || dog.tags.includes('calm')              // categorical
  ) && (
    dog.gender === 'male'                     // categorical   
  ) && (
    dog.eyeColor === 'brown'                  // categorical   
  )
)
```


Fake API Description
--------------------

`Dog-Finder-Server` uses the [serve](https://www.npmjs.com/package/serve) library to fake a simple API.  
This API has only one GET endpoint:  
`http://localhost:4000`

(actually, it's configured to serve the same data for any route)

The response served comes from `./Dog-Finder-Server/db.json`
It's a json object with:
 - a `success` field set to `true`,
 - and a `payload` field whose value is a procedurally generated __array of objects__ discribing dogs

It has the following format (expressed as a [json schema](https://json-schema.org/)):
```json
{
  "title": "API response",
  "type": "object",
  "properties": {
    "success": {
      "type": "boolean"
    },
    "payload": {
      "type": "array",
      "items": {
        "title": "The Dog Object",
        "type": "object",
        "additionalProperties": false,
        "properties": {
          "_id": { "type": "string"},
          "name": { "type": "string"},
          "price": {
            "type": "string",
            "pattern": "^\\$(?:(?:\\d{1,3}),)*(?:\\d{1,3})(?:\\.\\d{2})?$",
            "examples": [
              "$9,999.99", "$999.99", "$9.99",
              "$9,999", "$999", "$9"
            ],
          },
          "picture": { "type": "string"},
          "age": { "type": "number"},
          "eyeColor": { "type": "string"},
          "gender": { "type": "string"},
          "tags": {
            "type": "array",
            "items": {"type": "string"},
            "uniqueItems": true
          }
        },
        "required": [
          "_id", "name", "price",
          "picture", "age",
          "eyeColor", "gender",
          "tags"
        ],
      }
    }
  }
}
 ```


Submission
----------

To submit your solution, upload your code on any Git-based source code repository hosting service of your choice (ie: github, bitbucket, gitlab, ect)
And send us a link to it.

To be valid, we should be able to run it by:
  - `git clone` your repository 
  - `cd` into it 
  - and run `npm install && npm run dev`

And by following the [Getting Started / Server](#server) section
we should be able to use your app.

If, for some reason, we need to do anything different, make sure to provide a README with the necessary instructions.

Advice
------

You can use either javascript or typescript. Whatever you're most comfortable with.  
Typescript may be more challenging if you're not used to it.

Try to match our design, don't waste time trying to come up with yours, as the point of this task is about that Filter Builder.

Matching our design is a less important part of this assignement but will still weight a bit, since as a frontend dev you should be comfortable enough to do some HTML and CSS


Assessment
----------

We will mainly assess you on the filtering part:
  - correctness, the filter should work as described
  - code quality, proper organization, naming, formatting and readability

But the following will play a small part:
  - pertinent use of modern CSS (flexbox/grid layout)
  - use of semantic html (we're not fan of bootstrap div purée)


Please contact us if you need any clarification.
