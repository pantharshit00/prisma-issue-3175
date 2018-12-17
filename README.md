# Introduction

This is a reproduction of https://github.com/prisma/prisma/issues/3715

## To Reproduce

1. Clone and fill your local postgres info in docker-compose file
2. Run these sql queries
```sql
CREATE TABLE table_a (
  id uuid NOT NULL PRIMARY KEY,
  name text
);

CREATE TABLE table_b (
  id uuid NOT NULL PRIMARY KEY,
  a_id uuid references table_a(id)
);

insert into table_a (id, name) values ('ab6981ee-02e8-4d3c-87a1-08d45ebd8b03', 'test');
insert into table_b (id, a_id) values ('5ac8367d-702e-4eff-b3fe-edfd9490c967', null);
```
3. Start the container and deploy the datamodel
4. Run the following query
```graphql
query {
  tableBs {
    id
    a {
      id
    }
  }
}
```


## Stacktrace

```
java.lang.NullPointerException
	at com.prisma.logging.LogDataWrites$.$anonfun$anyWrites$1(LogData.scala:24)
	at play.api.libs.json.Writes$$anon$6.writes(Writes.scala:143)
	at play.api.libs.json.DefaultWrites.$anonfun$mapWrites$2(Writes.scala:243)
	at scala.collection.MapLike$MappedValues.$anonfun$foreach$3(MapLike.scala:253)
	at scala.collection.TraversableLike$WithFilter.$anonfun$foreach$1(TraversableLike.scala:789)
	at scala.collection.immutable.HashMap$HashMap1.foreach(HashMap.scala:231)
	at scala.collection.immutable.HashMap$HashTrieMap.foreach(HashMap.scala:462)
	at scala.collection.TraversableLike$WithFilter.foreach(TraversableLike.scala:788)
	at scala.collection.MapLike$MappedValues.foreach(MapLike.scala:253)
	at scala.collection.MapLike.toSeq(MapLike.scala:335)
	at scala.collection.MapLike.toSeq$(MapLike.scala:330)
	at scala.collection.AbstractMap.toSeq(Map.scala:59)
	at play.api.libs.json.DefaultWrites.$anonfun$mapWrites$1(Writes.scala:243)
	at play.api.libs.json.OWrites$$anon$3.writes(Writes.scala:112)
	at play.api.libs.json.OWrites$$anon$3.writes(Writes.scala:111)
	at play.api.libs.json.PathWrites.$anonfun$nullable$2(JsConstraints.scala:185)
	at play.api.libs.json.OWrites$$anon$3.writes(Writes.scala:112)
	at play.api.libs.json.OWrites$MergedOWrites$.mergeIn(Writes.scala:76)
	at play.api.libs.json.OWrites$MergedOWrites$$anon$1.writeFields(Writes.scala:68)
	at play.api.libs.json.OWrites$MergedOWrites$$anon$1.writeFields(Writes.scala:64)
	at play.api.libs.json.OWrites$OWritesFromFields.writes(Writes.scala:96)
	at play.api.libs.json.OWrites$OWritesFromFields.writes$(Writes.scala:94)
	at play.api.libs.json.OWrites$MergedOWrites$$anon$1.writes(Writes.scala:64)
	at play.api.libs.json.OWrites$$anon$2.$anonfun$contramap$1(Writes.scala:107)
	at play.api.libs.json.OWrites$$anon$3.writes(Writes.scala:112)
	at play.api.libs.json.OWrites$$anon$3.writes(Writes.scala:111)
	at play.api.libs.json.Json$.toJson(Json.scala:189)
	at com.prisma.sangria.utils.ErrorHandler.com$prisma$sangria$utils$ErrorHandler$$logError(ErrorHandler.scala:70)
	at com.prisma.sangria.utils.ErrorHandler$$anonfun$handler$lzycompute$1.applyOrElse(ErrorHandler.scala:34)
	at com.prisma.sangria.utils.ErrorHandler$$anonfun$handler$lzycompute$1.applyOrElse(ErrorHandler.scala:25)
	at scala.runtime.AbstractPartialFunction.apply(AbstractPartialFunction.scala:34)
	at sangria.execution.ResultResolver$ErrorRegistry.createErrorPaths(ResultResolver.scala:145)
	at sangria.execution.ResultResolver$ErrorRegistry.add(ResultResolver.scala:120)
	at sangria.execution.ResultResolver$ErrorRegistry$.apply(ResultResolver.scala:161)
	at sangria.execution.Resolver$$anonfun$$nestedInanonfun$resolveActionsPar$4$1.applyOrElse(Resolver.scala:615)
	at sangria.execution.Resolver$$anonfun$$nestedInanonfun$resolveActionsPar$4$1.applyOrElse(Resolver.scala:614)
	at scala.runtime.AbstractPartialFunction.apply(AbstractPartialFunction.scala:34)
	at scala.util.Failure.recover(Try.scala:230)
	at scala.concurrent.Future.$anonfun$recover$1(Future.scala:390)
	at scala.concurrent.impl.Promise.liftedTree1$1(Promise.scala:29)
	at scala.concurrent.impl.Promise.$anonfun$transform$1(Promise.scala:29)
	at scala.concurrent.impl.CallbackRunnable.run(Promise.scala:60)
	at akka.dispatch.BatchingExecutor$AbstractBatch.processBatch(BatchingExecutor.scala:55)
	at akka.dispatch.BatchingExecutor$BlockableBatch.$anonfun$run$1(BatchingExecutor.scala:91)
	at scala.runtime.java8.JFunction0$mcV$sp.apply(JFunction0$mcV$sp.java:12)
	at scala.concurrent.BlockContext$.withBlockContext(BlockContext.scala:81)
	at akka.dispatch.BatchingExecutor$BlockableBatch.run(BatchingExecutor.scala:91)
	at akka.dispatch.TaskInvocation.run(AbstractDispatcher.scala:40)
	at akka.dispatch.ForkJoinExecutorConfigurator$AkkaForkJoinTask.exec(ForkJoinExecutorConfigurator.scala:43)
	at akka.dispatch.forkjoin.ForkJoinTask.doExec(ForkJoinTask.java:260)
	at akka.dispatch.forkjoin.ForkJoinPool$WorkQueue.runTask(ForkJoinPool.java:1339)
	at akka.dispatch.forkjoin.ForkJoinPool.runWorker(ForkJoinPool.java:1979)
	at akka.dispatch.forkjoin.ForkJoinWorkerThread.run(ForkJoinWorkerThread.java:107)
```
