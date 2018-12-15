# final

Everything after midterm (dont forget CAP theorem; no NFS, DNS ...)

60% exam, 40% hw

Material:

- Blockchain
- Replication strategy: Raft, (Multi-) Paxos, Viewstamped Replication
- Election: Bully
- Broadcast: ISIS; ordering of broadcast: total-ordering, FIFO, causal
- Consistency: Eventual, Causal, Sequential, Linearizable
- 2 phase commit (no transaction / 3 phase commit)
- Distributed Hash Table: Finger table
- Byzantine Failure (OM & SM)

# Actor model

Actor: entity exist on computer

Popularized by Erlang => (nicer syntax) Elixir

## fields

Identifier, (execution) stack, message queue;

## rules

two Actors do not share memory (dont have to be on the same computer); can send message;

if one Actor stores a counter, and the other Actor want to query that counter:

- {:Query, process identifier} (to be sent back)
- {:Incre} (increment the counter)

if an Actor doesnt response: assume the Actor dies

=> an Actor could request Monitor over another Actor: the runtime of the system notify the other Actor that are querying on a dead Actor


# Erlang / Elixir

start a process::

self() (current PID)

Process.register self(), :processName

```elixir
/* receive test & remove the message matching that pattern from the message queue */
receive do
    {:TEST} -> IO.puts "Got test"
    /* x: the second argument */
    {:TEST, x} -> IO.puts(x)
end
```

```elixir
/* send message to a named process */
/* foo: instance of elixir runtime */
send {:processName, :"foo@userName"}, {:TEST}
```

```elixir
defmodule Counter do
    def counter_loop(val) do
        receive do
            {:QUERY, pic} ->
                send pid, val
                counter_loop(val)
            {:INCRE} ->
                counter_loop(val+1)
        end
    end

    def start_counter_loop() do
        /* fn: lambda function */
        /* return last line of the function */
        spawn fn -> counter_loop(0) end
    end
end

```


spawn: new process (Actor), returns a pid

functional, no loop (tend to recurse instead of mutate things)

tail-recursive: recognize recurse that doesnt come back to previous loop (so tail down the previous function invocation)

all marshaling is default

hot (still running) operate; upgrade consent: runtime from outdated to newer version of function, without stopping the entire program (like database migration)

OTP library: infrastructure on top of messaging system; Erlang: originally built for digital telephone (almost never fail?) => supervision trees (relation of processes)

chat:
```
{:DOWN, ^server_ref, _, _, _}: sent by monitor for that monitored process
    ^server_ref: the second argument must be unique server_ref the monitor provides
```



bittorrent (networking); torque (asymmetric...); map reducing; dynamo; gpu programming; 
