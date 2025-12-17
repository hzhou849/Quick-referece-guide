# Windows reference

## Queues





## Queues

### Concurrent
1. Enqueue
```ps
$queue.Enqueue($item)
```
Purpose: Adds an item to the end of the queue.

3. TryDequeue
```ps
if ($queue.TryDequeue([ref]$item)) {
    # $item now contains the dequeued value
}
```
Purpose: Attempts to remove and return the object at the beginning of the queue.
Returns: True if successful, False if the queue was empty.


3. TryPeek
```ps
if ($queue.TryPeek([ref]$item)) {
    # $item now contains the value at the front of the queue, but it is not removed
}
```
Purpose: Attempts to return the object at the beginning of the queue without removing it.
Returns: True if successful, False if the queue was empty.


4. ToArray
```ps
$array = $queue.ToArray()
```
Purpose: Copies the queue elements to a new array.

5. CopyTo
```ps
$myArray = New-Object string[] $queue.Count
$queue.CopyTo($myArray, 0)
```
Purpose: Copies the elements to an existing array, starting at a particular index.

6. Count (Property)
```ps
$count = $queue.Count
```
Purpose: Gets the number of elements in the queue.

7. IsEmpty (Property)
powershell

$isEmpty = $queue.IsEmpty
Purpose: Checks if the queue is empty.
8. GetEnumerator
powershell

foreach ($item in $queue) {
    # Process each item
}
Purpose: Returns an enumerator that iterates through the queue.
Summary Table
Method/Property	Purpose
Enqueue	Add item to end of queue
TryDequeue	Remove and return item from front (if any)
TryPeek	Return (but don’t remove) item from front (if any)
ToArray	Copy queue to new array
CopyTo	Copy queue to existing array
Count	Get number of items in queue
IsEmpty	Check if queue is empty
GetEnumerator	Enumerate all items in queue
Note:

There are no Dequeue or Peek methods—only TryDequeue and TryPeek, which are safer in concurrent scenarios.
All methods are designed to be thread-safe.
