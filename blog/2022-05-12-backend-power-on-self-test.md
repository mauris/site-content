---
title: "Backend Services: Power-on Self-test"
date: "2022-05-12"
coverImage: https://images.unsplash.com/photo-1458007683879-47560d7e33c3?ixlib=rb-1.2.1&ixid=MnwxMjA3fDB8MHxwaG90by1wYWdlfHx8fGVufDB8fHx8&auto=format&fit=crop&w=1920&q=80
coverImageCaption: Photo by [Thomas Kelley](https://unsplash.com/@thkelley) on Unsplash
tags:
  - webapp
  - fault-tolerance
  - backend
---

Backend services are the procesing blocks that empower the logic of an application served well. They do all sorts of things: talk to databases, keep things in caches, process data from message queues, serve webhooks for external systems, process images, or even send push notifications to users' mobile devices - indispensible part to a well designed software architecture, whether for a web or mobile app.

To make services more reusable and being able to change behaviour without changing code, configuration parameters are typically provided to tell the services what kind of environment it is operating in. For example:

- `NODE_ENV=production`: to inform that a service is running in production environment
- `DB_HOST, DB_PORT`: which database server to connect to
- `DB_USERNAME, DB_PASSWORD`: what user account to use when talking to database
- `JWT_SECRET`: what JWT shared secret to use
- `REQ_TIMEOUT_LIMIT=5000`: what timeout in milliseconds to use when sending requests to other services

Sometimes backend services deployment fail way past deployment because of misconfiguration. It is possible to enter the wrong `DB_HOST` hostname, missed out on setting the value for `JWT_SECRET` or even have a typo in the key or value of any configuration parameters. As services grow larger, we tend not to encounter these errors early on until the particular section of code gets executed under some specific conditions.

For example, if your database connection uses lazy loading and/or connection pooling, chances are that your backend service doesn't establish a connection to the database at start up (or shortly after deployment) until it is needed while serving some request much later.

> We end up celebrating too early at a successful deployment only to find the services failing when it starts serving workload.

## Enter Power-on Self-test

In the world of hardware and embedded systems, it is typical for engineers to embed a set of self-testing routines called [Power-on Self-test](https://en.wikipedia.org/wiki/Power-on_self-test) (POST). Simply put, when the system starts up

Power-on Self-test is much more common than we know it. They are commonly found in

- airplanes
- medical equipment
- computers
- phones
- gaming consoles
- TVs

> Can you imagine yourself in a plane up in the air, only to find the plane stalling mid-air because of fuel amount was in reserves during take-off?

Could the plane's software system checked on the amount of fuel remaining before allowing the pilot to start the engine?

Likewise, to build resilient backend services, it's important to include some form of POST routine at service pre-initialization. By allowing backend services to fail early on shortly after deployment, services wouldn't fail while serving requests or processing important data.

In an Express.js HTTP API service, I'd write a POST routine like this:

```typescript
// post.ts

/// .... imports goes here

const { JWT_SECRET } = process.env;

export async function powerOnSelfTest() {
  // check if JWT secret was configured and can be used to sign and verify payload
  assert(JWT_SECRET);
  const token = jwt.sign("testData", JWT_SECRET);
  const payload = jwt.verify(token, JWT_SECRET);
  assert(payload === "testData");

  // check if database connection and credentials are OK
  const dbConn = createDbConnection();
  try {
    await dbConn.authenticate();
  } catch (e) {
    console.error("Error connecting to DB", e);
    throw e;
  }
}
```

Before starting up the HTTP listener, I'd run the POST routine:

```typescript
// index.ts

/// .... other imports
import { powerOnSelfTest } from "./post";

async function startApp() {
  const app = express();

  const db = await initDbModels(createDbConnection());
  app.locals.db = db;

  /// ... setup routes

  const port = process.env.PORT || 3333;
  const server = app.listen(port, () => {
    console.log(`Listening at port ${port}`);
  });
}

// start up the application
powerOnSelfTest()
  .then(startApp)
  // catch POST and startup errors here
  .catch((err) => console.error(err));
```

Even if all the environment variables provided to the application were correct, the POST routine would at least check for connectivity between the service and dependent servers like the database. Problems like version incompatibility, network connectivity or incorrect infrastructure setup, can also be caught in the POST - _at very little cost_.

Having POST routines in place, I can be _slightly more_ confident that my backend services would work beyond post-deployment.

POST routines work as well as they are written. As Edsger W. Dijkstra puts it, POST routines - like any other tests - can prove the presence of bugs, **not** the absence of bugs. Instead, you can see POST routines as an additional tool to catch potential failures early on and thus building services that are more resilient.
