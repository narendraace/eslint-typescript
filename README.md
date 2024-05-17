# ESLinting with TypeScript
This is eslint understanding and sample to use for your typescript projects.


## What is Style Guides
When developing a Next-NestJS application, selecting an appropriate style guide is crucial for maintaining code quality and consistency. Not all style guides cater to the unique requirements of a Next-NestJS application. For instance, while Google's style guide is renowned for its thoroughness, it may not be the best fit for Next-NestJS due to compatibility issues. Similarly, if you prefer to include semicolons in your code, using the Standard style guide may not be ideal since it omits them.

In essence, the choice of a style guide should be based on more than just its reputation. It should align with the specific needs and preferences of your project. Despite the differences among style guides, they typically enforce fundamental rules to ensure code readability and maintainability, such as:
- Consistent indentation and spacing
- Proper use of quotes (single or double)
- Enforcing function and variable naming conventions
- Mandating or disallowing semicolons
- Ensuring the use of trailing commas where appropriate

Understanding these aspects helps in selecting a style guide that not only promotes best practices but also complements the development framework and your coding preferences.

## Compare Style Guides
Before delving into the differences among style guides, it is important to note that all of them enforce several fundamental rules to ensure code quality and readability:
- Tabs - 2 spaces [indent](https://eslint.org/docs/rules/indent)
- Quotes - Single [quotes](https://eslint.org/docs/rules/quotes)
- Brace style for control blocks - Same line [brace-style](https://eslint.org/docs/rules/brace-style)
- Prefer const/let over var - True [no-var](https://eslint.org/docs/rules/no-var)
- No trailing spaces -True [no-trailing-spaces](https://eslint.org/docs/rules/no-trailing-spaces)
- Array bracket spacing - No spaces [array-bracket-spacing](https://eslint.org/docs/rules/array-bracket-spacing)

**Note:** The rules mentioned above are distinct to specific rule sets and are not included in the default [ESLint-recommended](https://www.npmjs.com/package/eslint-config-recommended) configuration. However, they are essential for maintaining code quality and consistency. Therefore, it is important to incorporate these rules into your chosen style guide to ensure they meet the specific needs of your Next-NestJS application.


Below are some differences between themTo fully grasp the importance of style guides, it is essential to consider a few additional rules:

| Rule         | Google       | AirBnB      | Standard    |
| ------------ | ------------ |------------ |------------ |
| Semicolons (semi) | Required | Required | No |
| Trailing Commas (comma-dangle) | Required | Required | Not Allowed |
| Template Strings (prefer-template) | No Stance | Prefered | No Stance |
| Space Before Function Parentheses (space-before-function-paren) | No Space | No Space | Space Required |
| Import Extensions (import/extensions) | Allowed | Not Allowed | Allowed |
| Object Curly Spacing (object-curly-spacing) | No Space Allowed | Space Required | Space Required |
| Console Statements (no-console) | No Stance | None | No Stance |
| Arrow Functions Return Assignment (no-return-assign) | No Stance | No | No |
| React Prop Ordering (react/sort-prop-types) | N/A | No Stance | No Stance |
| React Prop Validation (react/prop-types) | N/A | Required | Not Required |
| Object Property Shorthand (object-shorthand) | No Stance | Prefer | No Stance |
| Object Destructuring | No Stance | Prefer | No Stance |


These rules are just a subset of the extensive array of guidelines that these rule sets enforce. A style guide can impose more than a hundred rules, with some being universally enforced across all major guides, while others are unique to specific guides. Ultimately, the choice of style guide depends on the specific requirements and preferences of your project. Check more rules [here](https://eslint.org/docs/rules/).

It is also important to note that if you find a style guide that generally meets your preferences but has certain rules you would like to modify, you can always customise it by specifying particular rules in your .eslintrc configuration. This allows you to tailor the style guide to better suit the specific needs of your project.

## How to configure Style Guide
To enable a style guide, you have two options. You can reinitialize ESLint, though it is recommended to manually add the dependency using pnpm. Alternatively, you can manually add the necessary dependency and then edit the .eslintrc file accordingly, For Example:

- [ESLint Standard Style Config](https://www.npmjs.com/package/eslint-config-standard)
``` bash
pnpm add eslint-config-standard --D
“extends”: [“eslint:recommended”, “standard”]
```

- [ESLint Google Style Configuration](https://www.npmjs.com/package/eslint-config-google)
```bash
pnpm add eslint-config-google --D
“extends”: [“eslint:recommended”, “google”]
```

- Recommended [ESLint Airbnb Style Configuration](https://www.npmjs.com/package/eslint-config-airbnb)
```bash
pnpm add eslint-config-airbnb --D
“extends”: [“eslint:recommended”, “airbnb”]
```

In conclusion, the most important factor is selecting a style guide that aligns with your project requirements. If you prefer a straightforward and widely accepted option, Airbnb's style guide is highly recommended for Nest/Next development so, we will use AirBnB as style guide along with ESLint.

## How to install ESLint and AirBnB style guide for NextJS
To install ESLint  + AirBnB in a Next.js project, follow these steps:

- Install necessary packages:
```bash
pnpm install eslint @typescript-eslint/parser @typescript-eslint/eslint-plugin eslint-config-airbnb-base eslint-plugin-import eslint-plugin-prettier eslint-config-prettier --save-dev
```

- Update `.eslintrc.json`, Rules and basic setup things has been added so anyone can reuse it for NextJS. Take copy from `sample/nextjs-eslint-airbnb-sample/.eslintrc.json` and use into your project.

- Edit `package.json` for `script > lint` command, will it look like:
```bash
"scripts": {
    "lint": "next lint"
}
```
- Once it done, run your command inside your NextJS setup:
```bash
pnpm lint
```

**NOTE:** You will have a list of `Error` which inspected by our defined rules inside `.eslintrc.json`. These rules are defined the standards so do not change these rules.

## How to disable ESLint for production build
**NOTE:** This is not recommanded to disable ESLint for production OR development even. But PM can take approval from CTO, if wanted to make it disable, still this is temporary bases and not forever.

- To disable ESLint, update `next.config.js|mjs` with:
```bash
const nextConfig = {
  eslint: {
    // Warning: This allows production builds to successfully complete even if
    // your project has ESLint errors.
    ignoreDuringBuilds: true,
  },
};
```


## Auto linting while code commit to GIT
To prevent bad OR non-linted code commit, we will use tool [Husky](https://www.npmjs.com/package/husky). This tool will help us to auto lint our code before code going to commit at git repository.

### Install Husky at pre-commit git hook
- Install Husky:
```bash
pnpm install husky lint-staged --save-dev
```

- Configure, Edit `package.json` like:
```
"scripts": {
   .
   .
   .
    "precommit": "lint-staged"
},
"lint-staged": {
    "*.{js,jsx,ts,tsx,css,scss}": [
      "eslint --fix", //THIS WILL AUTO FIX POSSIBLE ERRORS BUT NOT ALL
    ]
},
"husky": {
    "hooks": {
      "pre-commit": "lint-staged",
    }
}
```

Now try to commit your bad code for testing and you can see that list of error/warning are there and may some of are fixed automatically. But fews you need to fixed it manually as per standard.