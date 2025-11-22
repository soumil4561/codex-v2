
## Microservices

Traditional applications were monolithic, they were large, self-contained applications with tightly coupled and often complex code, difficult to maintain and debug

Service Oriented Architecture (SOA) was an architecture style designed around reusable components called services, each executing a discrete business function. Communication between them happened by ESB (Enterprise Service Bus) which led to a bottleneck and a critical piece of the arch.

Microservices are simple loosely coupled services each exposing an interface (typically an API) via which other services can talk to it. 

### Benefits
- smaller codebase
- easier to understand
- easier to test
- easy to deploy
- each service can be independently operated on different tech stack
- can be scaled separately

### Challenges
- more services means more deployments and more points of points of failure
- inter-serivce communication can quickly become complex (spider web) and increase latency
- Integration testing is difficult
- debugging can be hard (telemetry required)


## Event Driven Architecture

**Event** is a record of something that happened. An event:
- captures an immutable fact that should not be modified
- occurs whether or not a service consumes it
- can be consumed as many times as necessary

Point to point microservices can often introduce coupling b/w services, while an event driven arch. inserts an event intermediary (an event bus) between the service. It isnt necessary for the consumer to know about the producer, all they care about is the event coming to them.

### Benefits:
- a centralized event service simplifies auditing and control
- producers and consumers of event are decoupled
- no spider web of communication
- new consumer can be added without modifying the producers
- they are resilient ie health of producer is not determined by the health of the consumer unlike in microservice arch.

> When services are asynchronous, push-based messaging allows clients to receive updates without needing to continuously poll remote services. 
> When using a polling model, consumers must continuously poll to determine when there's work to be done.
> Polling leads to increased network I/O and unnecessary delays
> Push Based model allows consumers to be notified when an event occured (like webhooks)

There are two basic approaches to coordination in an event-driven architecture: service choreography and service orchestration.

### Service Choreography
Service choreography is a decentralized approach where multiple services interact to complete a business process without a central coordinator

Instead, each service is responsible for its own logic and communicates with others by publishing and subscribing to events, often through a message or event broker.

The event structures that are exchanged between services are specified, and each service must generate and expect services in the correct formats.

**Tools in Google Cloud for service choreography:**
#### Pub/Sub
- fully managed real time messaging service
- publisher sends message to pub/sub topic
- pub/sub then delivers to each of the subscriber queues
- each subscriber receives the messages and then acknowledges it to remove from the queue

![[Service Orchestration and Choreography-1763734216143.webp]]

#### EventArc
- GCP managed events system
- Event producers: Google Cloud sources, Cloud Audit Logs, third party providers. Can also be generated from Pub/Sub Messages.
- Triggers are used to filter event for a specific target
- Pub/Sub is the transport layer ie eventarc is an abstraction over it
- Events use the cloudevents format (open source)

`The cloudevent spec:`
```json
{
  "data":{ EVENT_DATA },
  "datacontenttype": "application/json; charset=utf-8",
  "id": "MESSAGE_ID",
  "source": "//cloudaudit.googleapis.com/projects/PROJECT_ID/logs/data_access",
  "specversion": "1.0",
  "type": "google.cloud.audit.log.v1.written",
  "time": "EVENT_GENERATION_TIME",
  "dataschema": "https://googleapis.github.io/google-cloudevents/jsonschema/google/events/cloud/audit/v1/LogEntryData.json",
  "methodName": "jobservice.jobcompleted",
  "resourceName": "projects/my-project/jobs/bqjob_r3ac45813612fa2d6_0000017d591922c9_1",
  "serviceName": "bigquery.googleapis.com",
  "subject": "bigquery.googleapis.com/projects/my-project/jobs/bqjob_r3ac45813612fa2d6_0000017d591922c9_1"
}
```

User Eventarc when:
- want to select events from a wider range of sources
- use a simple rule based interface to select source, filter and destination instead of code to ingest events and manage topic/subscription
- want to use the standard cloud-events format
![[Service Orchestration and Choreography-1763734713688.webp]]
### Service Orchestration
With service orchestration, each service performs its own tasks, but the central orchestrator controls all interactions between the services. Services do not need to know about or communicate with other services in the orchestration.

A benefit of orchestration is that it provides a high-level view of business processes which helps with understanding the application, tracking execution, and troubleshooting issues (*something which is harder to track in choreography*)

Unlike the fully distributed services in the service choreography pattern, service orchestration has a single point of failure. If the orchestrator is not operable, the orchestrated processes cannot run.

**Tools in Google Cloud for service orchestration:**

#### Workflows
- fully managed orchestration platform which acts as the central orchestrator
- It orchestrates Google Cloud services and API calls, to build stateful / automated processes.
- provides a central source-of-truth for the application flow.
- A workflow can hold state, retry, poll, or wait for up to a year.

When to use:
- when you want to chain HTTP-based microservices into durable and stateful workflows.
- implement long-running processes and maintain observability for each execution.
- performing operations on a set of items or batch data.
![[Service Orchestration and Choreography-1763735063200.webp]]



### When to use which?

| Choreography                                                                                                                                                                    | Orchestration                                                                                                                        |
| ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | ------------------------------------------------------------------------------------------------------------------------------------ |
| Choreography provides decentralized control, where each of the services or applications connects to the others using events.                                                    | Orchestration is a strong pattern when you want to control a complex process that you can manage centrally.                          |
| Choreography is more appropriate when combining separate, decentralized services and applications using events, or when you want to leverage events from Google Cloud services. | using Workflows and orchestration provides the visibility, error handling, and reliability required to create your complex services. |

