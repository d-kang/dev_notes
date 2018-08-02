- [ ] Unique fields require you to do less checks? for example, on login, you dont have to check if the user first exists because graphql will know from the @unique directive?
- [ ] when the client communicates with graphql application layer and receives an error, how do use that error, edit the error to be used on the front end. For example, if someone is trying to create an account with an email that is already in use?
- [ ] You can send an error message by throwing a new Error('Some Message'). How do you select a code, if at all?
- [ ] How to remove a query field / after deleting a resolver?
- [ ] is there a way to look at the actual nodes in the primsa database?
- [ ] How to purge the prisma db?
- [ ] when adding a new feature, the past data, so a normal query wont work.
- [ ] do resolvers decide what arguments the query or mutation take?
- [ ] how/when is the jwt being stored in the db
- [ ] how do we retrieve a User
- [ ] can we specify what the db returns?
- [ ] there must be a prisma api context.db.exists context.db.request
- [ ] where does createVote come from in Mutations.js Vote
- [ ] can i have more than one subscription watcher on a single subscription {}
- [ ] why is the count still the entire collection of feed?





Answers
possibly, you dont have to purge the db when you add a new field. you were just accessing the data wrong. postedBy and user needed to be postedBy {} and user {} with subfields


to add args you need to add them to the schema and then to the resolvers


### orderBy
enum LinkOrderByInput {
  id_ASC
  id_DESC
  description_ASC
  description_DESC
  url_ASC
  url_DESC
  updatedAt_ASC
  updatedAt_DESC
  createdAt_ASC
  createdAt_DESC
}