# Process Tasks Using Servers

LeetCode: https://leetcode.com/problems/process-tasks-using-servers/

Difficulty: Medium

## Problem

You are given two 0-indexed integer arrays `servers` and `tasks` of lengths `n`
and `m`. `servers[i]` is the weight of the `i`th server, and `tasks[j]` is the
time needed to process the `j`th task in seconds.

Task `j` is inserted into a queue at second `j`. While there are free servers
and the queue is not empty, the task at the front of the queue goes to the free
server with the smallest weight (ties broken by smallest index). If no server is
free, wait until one becomes free and assign the next task immediately. A server
assigned task `j` at second `t` is free again at second `t + tasks[j]`.

Return an array `ans` of length `m`, where `ans[j]` is the index of the server
that processes task `j`.

### Example 1

```
Input: servers = [3,3,2], tasks = [1,2,3,2,1,2]
Output: [2,2,0,2,1,2]
```

### Example 2

```
Input: servers = [5,1,4,3,2], tasks = [2,1,2,4,5,2,1]
Output: [1,4,1,4,1,3,2]
```

### Constraints

- `servers.length == n`, `tasks.length == m`
- `1 <= n, m <= 2 * 10^5`
- `1 <= servers[i], tasks[j] <= 2 * 10^5`

## Approach

Simulate with two priority queues:

1. `free`: idle servers, ordered by `(weight, index)`.
2. `used`: busy servers, ordered by `(freeTime, weight, index)`.

For each task `i` (the current time is `i`):

1. Move every server in `used` with `freeTime <= i` into `free`.
2. If `free` is empty, take the server that frees up earliest from `used`, and
   extend its `freeTime` by `tasks[i]`. The task starts when that server frees
   up, not at time `i`.
3. Otherwise take the best server from `free` and set its
   `freeTime = i + tasks[i]`.
4. Record the server's index in `ans[i]` and put it back in `used`.

## Complexity

- Time: `O((n + m) log n)`
- Space: `O(n)`

## Files

- `Solution.java` — Java solution
