# ESLint Config

When starting a new React project, it is helpful to have a consistent way of
writing of code to improve readability and maintainability.

Copy and paste this to your `package.json`.

```json
  "eslintConfig": {
    "extends": "react-app",
    "rules": {
      "semi": [
        "error",
        "never"
      ],
      "quotes": [
        "error",
        "single"
      ]
    }
  },
```
