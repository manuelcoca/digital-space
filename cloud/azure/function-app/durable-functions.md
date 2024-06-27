# Durable functions

Durable Functions are basically extensions of Azure Functions and has additional features:

* State across multiple functions
* Can be long-running
* Can "suspend" while waiting for a long-running API to return
* Functions can call other functions - chaining

A Durable Function consists of 3 elements:

1. Client - The client is the function trigger
2. Orchestrator - For routing the traffic and triggering other functions
3. Activities - An Activity is the actual function doing the work

## Durable Function Design Pattern

Durable Functions allows to implement different Design Patterns:

1. Function-Chaining-Pattern&#x20;
   * Functions are called one after the other
2. Fan out / in Pattern
   * After one function is finished it calls 3 other functions to run in parallel. Only if all 3 functions completed the work, the 4th Function will be triggered
3. Asynchronous Pattern
4. Monitor Pattern
   * Waits for an event to happen until Function is triggered (e.g could wait 1h, 1 day or 2 weeks)
5. Human interaction Pattern
   * Requires human interaction to continue work. Example: Function 1 finish and now waits for an approval to continue with the next function

## Install Durable Functions

1. Set Function Trigger to Durable Function
2. Open function environment with Visual Studio Code Editor (in Portal) or Console, add a package.json and run **`npm install durable-function`**
3. Create all 3 necessary Functions
   1. Durable Function HTTP Starter, which will call the orchestrator function
   2. Durable Functions orchestrator, which will call a defined set of activity functions
   3. Durable Function activities, which runs the jobs
