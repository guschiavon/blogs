# Gatsby Development: GraphQL Playground Set Up

## Installation

Run the following command to install Playground:
```
$ npm install --save-dev env-cmd
```
This will set-up Playground for the development environment only.

## File set-up

Create a `.env.development` file on the `root` directory of the project. Add the key value pair:
```
GATSBY_GRAPHQL_IDE=playground
```
Save and close.


On `package.json` make the following modifications:
```
"develop": "env-cmd -f .env.development gatsby develop",
```
Alternatively declare it directly on the `"develop"` array:
```
"develop": "GATSBY_GRAPHQL_IDE=playground gatsby develop",
```
Both solutions work the same.
