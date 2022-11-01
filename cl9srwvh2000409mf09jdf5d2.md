# How to Use Sequelize in a Loopback 4 Project?

> You can watch a ***video demo*** of this extension by [clicking here](https://youtu.be/tHg5ZAj29YQ).

In this article I will shows how you can plug an open source [loopback4-sequelize extension](https://github.com/sourcefuse/loopback4-sequelize) and start utilizing the capabilities of Sequelize under the hood in your loopback repositories.

To use it install the `loopback4-sequelize` package inside your loopback 4 project.
```sh
npm i loopback4-sequelize
```


Both newly developed and existing projects can benefit from the extension. By just changing the parent classes in the target Data Source and Repositories.

### Step 1: Configure DataSource

Change the parent class from `juggler.DataSource` to `SequelizeDataSource` like below.

```ts
// ...
import {SequelizeDataSource} from 'loopback4-sequelize';

// ...
export class PgDataSource
  extends SequelizeDataSource
  implements LifeCycleObserver {
  // ...
}
```

### Step 2: Configure Repository

Change the parent class from `DefaultCrudRepository` to `SequelizeRepository` like below.

```ts
// ...
import {SequelizeRepository} from 'loopback4-sequelize';

export class YourRepository extends SequelizeRepository<
  YourModel,
  typeof YourModel.prototype.id,
  YourModelRelations
> {
  // ...
}
```

And that is it. Now your function call to the configured repository from controller will internally be handled by Sequelize, which will be responsible for preparing the queries and executing them.

Note: The current version (v1.0.0) does not support relational queries. If you're reading this later than November 2022, it likely supports it already. Check out the official repository for the latest guide.

%[https://github.com/sourcefuse/loopback4-sequelize]

Thank you for reading :)