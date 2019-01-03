# listenToYourself-microservices-pattern-implementation
Implementation of listen to yourself micro services design pattern using Spring cloud stream (Kakfa binder) and couchbase as persistence store 

Pre-requisite:
1. Couchbase running locally
2. Kafka(port 9092) and zookeeper running locally

Once the application is up and running, hit the createOrder POST endpoint(http://localhost:32011/v1/createOrder) using postman or any other tool which allows sending POST requests. The order request body json example is:
{
	"orderId":"123",
	"customerId":"999",
	"fulfilmentGroupId":"FG1",
	"deliveryGroupId":"DG1"
}

On receiving the createOrder request, the service would publish OrderCreated event on Kafka topic: com.example.lty.order. The EventListener class is listening on topic: com.example.lty.order. The OrderCreated event is consumed internally and Order document is created in couchbase DB.


Since, the event is consumed internally which then persists the order in repository and the same event is consumed by external consumers also, this way using Listen To Yourself pattern both: persistence of order to DB and raising event for external consumers happen using one call ensuring atomicity i.e. either both succeed or both fail
