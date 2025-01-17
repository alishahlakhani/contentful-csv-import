# How to import a CSV file into Contentful as entries using the Contentful Content Management API and JavaScript

This currently assumes your content type is already set up and ready to import entries.

## Install dependencies

```bash
npm i
```

## Add your Contentful credentials.

Add a credentials.js file at the root of the project with the following:

```javascript
// credentials.js
module.exports.CMA_TOKEN = "your cma token";
module.exports.SPACE_ID = "your space id";
module.exports.CONTENT_TYPE_NAME = "content type to add to";
// You can set this to null to push directly to array. By default Contentful sets it to "en-US". Change it to push content to other langs
module.exports.DEFAULT_LOCALE = "en-US";
// CSV file location
module.exports.FILE_PATH = "./resources/animals.csv";
// Set this to TRUE to run a dry run
module.exports.DRY_RUN = true;
// Do you want to create a new environment?
// This will remove the EXPERIMENTAL_ENV_NAME environment and create a new one
module.exports.OVERRIDE_ENV = false;
module.exports.ENV_NAME = "your env name";
```

## To run this script, navigate to the project root and run:

```bash
node index.js
```

The script will perform the following actions:

1. Read the contents of ./resources/animals.csv
1. Convert the contents of the csv file to an array of JavaScript objects
1. Connect to a Contentful space and get a list of environments
1. Check for an experimental environment and delete and recreate one as necessary
1. For each item in the array, create a new 'animal' entry
1. Publish the entries


## [Optional] transformations 
1. Create `actions.js`
2. Added transformation functions for each column
```javascript
// actions.js
module.exports.transformations = {
    // Use the key to transform followed by a function which has value as input
    description: (value) => {
        var separateWord = value.toLowerCase().split(' ');
        for (var i = 0; i < separateWord.length; i++) {
            separateWord[i] = separateWord[i].charAt(0).toUpperCase() +
                separateWord[i].substring(1);
        }
        return separateWord.join(' ');
    }
}
```
