## Fail fast:

_**do now allow modification in object, within the iteration loop**

Ex: HashMap gives concurrent modification exception when try to modify map within the iteration.
Data structure: HashMap, Array list
Suitable for Single threaded system 
provides strong consistency
Iterator works on original object.
fast processing

## Fail Fast
Ex: ConcurrentHashMap does not give concurrent modification exception when try to modify map within the iteration.
`Data structure:  ConcurrentHashMap
Suitable for multi threaded system 
provides weak consistency
Iterator works on copy of object.
slow processing