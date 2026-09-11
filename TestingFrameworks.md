# Detox vs Jest
  Detox : End-to-end Test , Grey Box and Blackbox.
  Jest : Unit Test & Integration , White Box(test component in isolation and at code-level)

  # Detox 
   - Runs the compiled app binary on a real device/simulator/emulator, uses jest-cli.
   - Slower

    # FACTS
      - App reload on Physical device faces issues.
      - {newInstance : true} - restarts the app, if all in-memory data cleared including Async Storage. (like force-quit)
      - ByPass Login : 1. Deep Linking, 2. Launch Arguments. 3. Mock Login - simply setting pushtoken and usertoken in Async Storage. 
      - waitFor : This ensures Detox only taps after the button is actually rendered and visible.
  
  # Jest
   - 
   - Faster - To test components in isolation, mocking API and responses for faster test - In case of unit and integration.

  # If Detox is black-box and simulates the real user flow, why is Jest + virtual rendering considered more suitable for integration testing? Why not just always use Detox?
    Integration test goal - Do given parts of my app work together as expected?
    Example - Login Screen - UI inputs (React Native components), State handling (useState, useCallback), API call (api.get), Navigation (navigation.reset), Notifications (showMessage)
    We don't need the entire native environment to test these — just the JS environment where they interact.
    - Speed - Jest tests run in milliseconds. Detox needs to build and launch the app in an emulator for every test run (seconds → minutes).
    - Control - In Jest you can mock APIs, navigation, storage, timers, etc. → perfect for integration (where you want to test partly real, partly mocked).
 
  
 # Jest

Test runner & assertion library. Provides test structure, mocks, and matchers.
test(), it(), describe(), expect(), beforeEach(), afterEach()
js\ntest('adds numbers', () => {\n expect(2 + 3).toBe(5);\n});\n

1. react-test-renderer
   Render components into a virtual JSON tree for snapshot & structural testing.
   renderer.create(), .toJSON()
   js\nimport renderer from 'react-test-renderer';\nimport App from '../App';\n\nit('renders correctly', () => {\n const tree = renderer.create(<App />).toJSON();\n expect(tree).toMatchSnapshot();\n});\n

2. @testing-library/react-native (RNTL)
   User-focused testing for React Native. Lets you query components & simulate interactions.
   render(), getByText(), getByTestId(), fireEvent.press(), fireEvent.changeText()
   js\nimport { render, fireEvent } from '@testing-library/react-native';\nimport Counter from '../Counter';\n\ntest('increments on press', () => {\n const { getByText } = render(<Counter />);\n fireEvent.press(getByText('Click me'));\n expect(getByText('Clicked 1 times')).toBeTruthy();\n});\n

3. @testing-library/jest-native
   Extends Jest’s expect() with React Native–specific matchers.
   toHaveTextContent(), toBeDisabled(), toBeVisible(), toHaveProp()
   js\nimport { render } from '@testing-library/react-native';\nimport { Text, Button } from 'react-native';\n\ntest('button is disabled', () => {\n const { getByText } = render(<Button title=\"Save\" disabled />);\n expect(getByText('Save')).toBeDisabled();\n});\n

4. jest-fetch-mock Mock fetch API calls in Jest tests (network mocking). fetch.mockResponseOnce(), fetch.resetMocks(), fetch.mockRejectOnce() js\nimport fetchMock from 'jest-fetch-mock';\nfetchMock.enableMocks();\n\nbeforeEach(() => fetch.resetMocks());\n\ntest('mocks API call', async () => {\n fetch.mockResponseOnce(JSON.stringify({ data: 'hello' }));\n const res = await fetch('https://api.com/data');\n const json = await res.json();\n expect(json.data).toBe('hello');\n});\n

Babel dependency - @babel/preset-react - help render jsx using render method inside test.




