## Coursework

A game component might be made of multiple behaviours interacting.

## Guard A.I.

Made from the following behaviours:
* patrol - moves along a path
* detect - detects suspicious object
* chase - chases intruder
* fight - attacks intruder
* investigate - moves to suspicious object and search
* alarm - moves to an alarm point / uses radio

The A.I. will be in some number of states:
### patrolling 
run patrol and detect behaviours
if detect behaviour detects something suspicious, switch to alarm state

### alarm
run alarm behaviour only
move to alarm point, raise alarm, then go into investigate or chase state

### investigate
run investigate and detect behaviour
go to the last location of the suspicious object and look around
if detect behaviour detects the player, switch to chase state
if nothing detected within a time limit, reset alarm, switch to patrol

### chase
run chase and detect behaviours
if detect loses target, switch to investigate behaviour
if within range switch to fight behaviour

### fight
run fight and detect behaviours
if not within range switch to chase behaviour
