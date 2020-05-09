# Gatsby Development: GraphQL Playground Set Up

## Installation

Run the following command to install Playground:
```
$ npm install --save-dev env-cmd
```
This will set-up Playground for the development environment only.

## Config

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

## Run
Start the development server with `npm run develop` and open the `GraphQL` by accessing `localhost:8000/___graphql`
That' it!

## Running queries
