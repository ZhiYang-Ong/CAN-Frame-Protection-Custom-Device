# Cyclic Scheduler

The cyclic transmission engine uses a bucket-based scheduler to eliminate continuous polling of cyclic CAN frames.

## Architecture

```text
Timed Loop Scheduler
        ↓
    Period Buckets
        ↓
      RT FIFO
        ↓
Payload Processing
        ↓
     XNET Write
```

## Operation

Frames are grouped into buckets based on their configured cycle time.

Example:

```text
10 ms Bucket
50 ms Bucket
100 ms Bucket
```

The scheduler only processes buckets that are due, significantly reducing execution time compared to checking every cyclic frame on each iteration.

## Configuration

### Bucket Size

Defines the number of cyclic frames grouped within the same cycle-time bucket.

Frames in the same bucket are evaluated together for counter updates and transmission requests before being enqueued into the RT FIFO.

### Phase Offset

Applies an offset to each newly created bucket to distribute workload across scheduler ticks.

This reduces the likelihood of multiple buckets with different cycle times becoming due simultaneously and rolling over together, helping to smooth RT FIFO, payload processing, and transmission workload.

## Benefits

- Eliminates continuous polling of cyclic frames.
- Reduces CPU utilization and execution time.
- Improves scalability for large databases.
- Preserves cyclic timing accuracy.
- Reduces workload peaks through bucket phase distribution.
- Provides a simple and deterministic scheduling mechanism for cyclic CAN transmission.