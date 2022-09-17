# graphql-vscode-bug

https://github.com/graphql/graphiql/issues/2767

This bug is probably related to defining `graphql-config.load.rootDir` in a different directory than the `schema.graphql` and the schema watcher not firing.

## Steps to reproduce

1. Open project in vscode
1. Open `src/query.graphql` notice that the language service works fine
1. `git checkout feat/add-create-user-mutation`
1. Open `src/mutation.graphql`
1. Notice error on `CreateUserInput!` even though the schema has it defined
1. Restart graphql language server
1. Notice language service is fine now

In this example we see:

```
.
├── .vscode
│  └── settings.json
├── configs
│  └── .graphqlrc.yml
├── README.md
├── schema.graphql
└── src
   ├── mutation.graphql
   └── query.graphql
```

Since `.graphqlrc.yml` and `schema.graphql` are in separate folders, we see that the `GetUser` query is fine because of the stale schema stored in the language server. But updating the schema does not update the schema stored in the language server. This is shown since `CreateUserInput!` errors out.
