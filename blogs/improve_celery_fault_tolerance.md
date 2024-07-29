## How to improve fault tolerance in celery with redis/rabbitMQ

We have been using celery with redis for expensive and complex tasks such as derived data analytics, pdf generation, large file uploads/validations. Although celery provides `retry_task` option to retry failed task. But recently I thought of a scenario - where  redis(broker) goes down for whatever reason - how to preserve tasks from celery. Following are few workarounds that I could think of.

# Redis Sentinel
- Use redis sentinel to promote replica(secondary) node to master node.
- cons - Sentinel requires an additional node to run, additional cloud/server cost.

# My solution
- Custom configure your application to switch to secondary node - this can be achieved by writing a wrapper in your application health `API` procedure/function, since this endpoint is hit every few seconds. We might still loose some tasks but that would be for a very short duration - few seconds.
  
Solution 2 in my opinion is better without running any additional node or incurring extra cost.
