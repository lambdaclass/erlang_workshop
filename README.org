# Erlang Process

## Process creation
How to create (spawn) a process:

```erlang
spawn(Module, Fun, Args) -> pid()
```

`spawn` creates a new process and returns its PID. The new process will start executing in `Module:Fun(Arg0, ..., ArgN)`

## Registering a process
Instead of addressing a process by its PID we can register it with a name and refer to it using that. The name must be an atom and will be automatically unregistered when the process terminates.

```erlang
%% Associates name Name with process Pid
register(Name, Pid) -> true

%% Returns a list of names that have been registered
registered()

%% Returns the Pid registered under Name, or undefined if the name is not registered
whereis(Name) -> Pid | undefined
```

## Sending and receiving messages
Processes can communicate by sending messages between them. Each process has a mailbox (queue) from which it will look for new messages that match the receiving pattern or for timeout to happen.

To send a message we use the send operator `!`:

```erlang
%% Send by PID
Pid ! Msg.
self() ! [blob].
pid(0, 118, 0) ! bar.

%% Send by registered name
Name ! {Stuff1, Stuff2}.
proc1 ! foo.
```

To receive a message (pop it from mailbox) we call `receive`, optionally we can also use  `after` for a timeout:

```erlang
receive
  Pattern1 [when Guard1] ->
    Body1;
  .
  .
  .
  PatternN [when GuardN] ->
    BodyN
after
  60000 ->
    BodyAfter
end
```

## Process dictionary
Each process has its own dictionary, which you can access using the following BIFs:

```erlang
%% Returns the entire process dictionary.
get() -> [{Key1, Val1}, ...]

%% Returns the item associated with Key or `undefined`
get(Key) -> Item | undefined

%% Returns a list of all keys whose associated value is Value.
get_keys(Value) -> [...]

%% Associate Value with Key. Returns the old value associated with Key or `undefined` if no such association exists.
put(Key, Value) -> OldValue | undefined

%% Erases the entire process dictionary. Returns the entire process dictionary before it was erased.
erase() -> [{Key1, Val1}, ...]

%% Erases the value associated with Key. Returns the old value associated with Key or undefined if no such association exists.
erase(Key) -> OldValue | undefined
```
