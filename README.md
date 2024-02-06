
# React Testing Library and Jest in Next.js project: Beginner Guide


- The goal of today's lesson is to help you get set up and going with jest in the react testing
library in an Next.js project using typescript.

- At the beginning lets create a new next js project. Create a folder with your project name. Go to address bar and write cmd. Then in the command prompt write

```javascript
  npx create-next-app@latest
```
- Then create the project. Then open the project in vs code. 

- Open the terminal and paste the following code for installing testing library.

```javascript
npm i -D @testing-library/jest-dom @testing-library/react @testing-library/user-event jest jest-environment-jsdom ts-jest
```
- Check the package.json and inside the devDependencies following 6 files should be present.
```javascript
"devDependencies": {
    "@testing-library/jest-dom": "^5.16.5",
    "@testing-library/react": "^14.0.0",
    "@testing-library/user-event": "^14.4.3",
    "jest": "^29.6.2",
    "jest-environment-jsdom": "^29.6.2",
    "ts-jest": "^29.1.1"
  }
```
- Your version could be different from mine but the property name should be same.

- In the package.json file add some test scripts in the scripts property. Add  "test": "jest" and "test:watch": "jest --watchAll". The second one will allow us to keep watching any changes. The watch all flag which will allow us to continually run the tests if they change fairly quickly. Now the package.json file will look like as follows.

```javascript
 "scripts": {
    "dev": "next dev",
    "build": "next build",
    "start": "next start",
    "lint": "next lint",
    "test": "jest",
    "test:watch": "jest --watchAll"
  },
```
- Now create a new file at the same level of package.json and name it jest.config.js and paste following code:
```javascript
const nextJest = require('next/jest')

const createJestConfig = nextJest({
    // Provide the path to your Next.js app to load next.config.js and .env files in your test environment
    dir: './',
})

// Add any custom config to be passed to Jest
/** @type {import('jest').Config} */
const config = {
    // Add more setup options before each test is run
    setupFilesAfterEnv: ['<rootDir>/jest.setup.js'],
    testEnvironment: 'jest-environment-jsdom',
    preset: 'ts-jest',
}

// createJestConfig is exported this way to ensure that next/jest can load the Next.js config which is async
module.exports = createJestConfig(config)
```

- Now the root level create another file name jest.setup.js and paste the following code

```javascript
import '@testing-library/jest-dom/extend-expect'
```

- Open the terminal and type following and enter

```javascript
  npm i -D eslint-plugin-jest-dom eslint-plugin-testing-library
```

- Now go to the .eslintrc.json file at the root and add following code. 

```javascript
{
  "extends": [
    "next/core-web-vitals",
    "plugin:testing-library/react",
    "plugin:jest-dom/recommended"
  ]
}
```

- Now at the root of your project create a folder with exact following name __tests__

- Now inside __tests__ folder create a new file with the name of Home.test.tsx. Note that the test file should also end with .tsx or .jsx extension. Paste the following code in the file

```javascript
import { render, screen } from '@testing-library/react'
import Home from '@/app/page'

    it('should have Docs text', () => {
        render(<Home />) // ARRANGE 

        const myElem = screen.getByText('Docs') // ACT 

        expect(myElem).toBeInTheDocument() // ASSERT
    })
    
```
- If you find any error in the it and expect then go to the terminal and type following and press enter.

```javascript
npm i -D @testing-library/jest-dom@5.16.5
```

- We imported render and screen from react testing library. We also imported Home component from the app directory of root home page because we are going to test it. 

- There is a pattern for writing tests and it is called triple a pattern (Arrange, Act & Assert). First you arrange everything for the test, then you take an action and at last we check everything and provide the assertion. We will demonstrate them in code later.  

- We will begin to write test by the word of "it". Then inside perenthesis we will describe it as "should have Docs text", and now we need a function to check that and it's a Anonymous Arrow function. 
- Inside the arrow function first we call render and it will render the Home component. So it will fall under the arrange.

- In the action we created myElem variable. from screen we call getByText and pass "Docs" word that we want to search from the home component. 

- In the assertion we called expect and pass myElem to it. Then we also called toBeInTheDocument.

- Now save the file and open terminal and type npm test and press enter. The result will show pass as in the default Home component there is a word Docs so our test passed. 

- How it work? At the beginning inside the package.json file we wrote "test":"jest". So when we will write npm test in the terminal it will go to the __tests__ folder and run test on every file inside that folder. 

- The home.test.tsx is called test suite. If we want to write more test in the same suite we can wrap the test by describe anonymous function. And inside the describe function we will write all the tests. The test suite will look like as follows

```javascript
import { render, screen } from '@testing-library/react'
import Home from '@/app/page'

describe('Home', () => {
    it('should have Docs text', () => {
        render(<Home />) // ARRANGE 

        const myElem = screen.getByText('Docs') // ACT 

        expect(myElem).toBeInTheDocument() // ASSERT
    })
})
```

- Now we will create another test that will check whether `information` word is included in the Home component.  The code will be like following. 

```javascript
import { render, screen } from '@testing-library/react'
import Home from '@/app/page'

describe('Home', () => {
    it('should have Docs text', () => {
        render(<Home />) // ARRANGE 

        const myElem = screen.getByText('Docs') // ACT 

        expect(myElem).toBeInTheDocument() // ASSERT
    })

    it('should contain the text "information"', () => {
        render(<Home />) // ARRANGE 

        const myElem = screen.getByText(/information/i) // ACT 

        expect(myElem).toBeInTheDocument() // ASSERT
    })

})
```
- The test will be almost same as previous test but we will use regex function inside the getByText function like (/information/i). The i flag will make sure that it doesn't matter if it's all caps or lowercase it's case insensitive.

- After saving the file open the terminal and type npm test and press enter. It should show one suite and two tests passed. 

- Let's add a third test in the test suite and we want to search for heading the Home component. The code will be look like following

```javascript
import { render, screen } from '@testing-library/react'
import Home from '@/app/page'

describe('Home', () => {
    it('should have Docs text', () => {
        render(<Home />) // ARRANGE 

        const myElem = screen.getByText('Docs') // ACT 

        expect(myElem).toBeInTheDocument() // ASSERT
    })

    it('should contain the text "information"', () => {
        render(<Home />) // ARRANGE 

        const myElem = screen.getByText(/information/i) // ACT 

        expect(myElem).toBeInTheDocument() // ASSERT
    })

    it('should have a heading', () => {
        render(<Home />) // ARRANGE 

        const myElem = screen.getByRole('heading', {
            name: 'Learn'
        }) // ACT 

        expect(myElem).toBeInTheDocument() // ASSERT
    })
})
```
- Here we used getByRole instead of getByText and mention the role as heading. We can also be more specific by putting the heading name in an object. In the above example we search Learn heading. 

- Now if we run the test by typing npm test in the terminal it will show fail result for third test. Because if you go to the Home component then you will see there are other things inside the heading with Learn text. So the test failed. If we remove everything from the heading except Learn text and run the test again then we will get pass result.


# React Testing Library and Jest in Next.js & React project: Intermediate Guide



