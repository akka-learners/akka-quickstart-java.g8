## Creating the Actors
 
So far we have looked at the definitions of Actors and their messages. Now let's dive a bit deeper into the power of location transparency and see how to create Actor instances.
 
### The power of location transparency
 
In Akka you *can't* create an instance of an Actor using the `new` keyword. Instead, you create Actor instances using a factory `spawn` methods. 
Spawn does not return an actor instance, but a reference, `akka.actor.typed.ActorRef`, that points to the actor instance. 
This level of indirection adds a lot of power and flexibility in a distributed system. 
 
_In Akka location doesn't matter_. Location transparency means that the `ActorRef` can, while retaining the same semantics, represent an instance of the running actor in-process or on a remote machine.  
If needed, the runtime can optimize the system by changing an Actor's location or the entire application topology while it is running. This enables the "let it crash" model of failure management in which the system can heal itself by crashing faulty Actors and restarting healthy ones.
 
### The Akka ActorSystem

An `ActorSystem` is the intial entry point into Akka, usually only one is created per application.
An `ActorSystem` has a name and a guardian actor. The bootstrap of your application is typically 
done within the guardian actor.

The guardian this sample actor is `GreeterMain`.

@@snip [GreeterMain.java]($g8src$/java/com/lightbend/akka/sample/AkkaQuickstart.java) { #actor-system }

It uses `Behaviors.setup` to bootstrap the application

@@snip [GreeterMain.java]($g8src$/java/com/lightbend/akka/sample/GreeterMain.java)

### Spawning child actors

Other actors are created using `spawn` methods on `ActorContext`. The `GreeterMain` creates a `Greeter` actor
this way on startup as well as a new `GreeterBot` each time it receives a `Start` message.
 
 
@@snip [GreeterMain.java]($g8src$/java/com/lightbend/akka/sample/GreeterMain.java) { #create-actors }
