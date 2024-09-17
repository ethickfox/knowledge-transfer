---
Interview graded: true
Last edited time: 2023-11-26T15:28
Last recall: 2023-11-26
Needs Rework: true
Status: Not started
Topic:
  - "[[Knowledge Transfer/Backend/Java/Java Core/Java Core\\|Java Core]]"
---
When to use composition, when inheritance

  

What is the point of static and default methods in interface?

  

What will happin if we inherit interfaces with the same default methods

[https://stackoverflow.com/questions/22685930/implementing-two-interfaces-with-two-default-methods-of-the-same-signature-in-ja](https://stackoverflow.com/questions/22685930/implementing-two-interfaces-with-two-default-methods-of-the-same-signature-in-ja)

How many threads are created for parallel streams?

Number of CPU cores

  

Distinct by id in stream

```Java
Function<Function<Car,Long>,Predicate<Car>> distinctById = (keyExtractor) -> {
    Set<Object> seen = new HashSet<>();
    return item -> seen.add(keyExtractor.apply(item));
};
timecardChanges.stream()
        .filter(distinctById.apply(change -> change.getId()))
        .collect(Collectors.toList());
```