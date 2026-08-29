Counter Module – Design Notes

Overview

I designed a clock-controlled counter using a running state to remember whether the counter should continue counting.

The counter has four main behaviors:

Start: starts the counter.

Stop: stops the counter without clearing the current count.

Reset: clears the count to 0 and stops the counter.

MAX: when the count reaches MAX, it wraps back to 0 and keeps counting.

The priority is:

reset > stop > start

How My Logic Works

I used a separate 1-bit running signal because start and stop are pulses. The pulse changes the stored state instead of having to stay high.

start  → running = 1
stop   → running = 0
reset  → running = 0 and count = 0

When running is active, the counter increments on every clock cycle.

For the MAX condition:

if count == MAX
    count = 0
else
    count = count + 1

I used:

always_ff @(posedge clk)

because the counter and running state are sequential signals controlled by the clock.

My Approach vs Reference

My implementation keeps the design simple using mainly:

count

running

start

stop

reset

I did not use separate temp, next_temp, or next_state signals.

The reference implementation uses:

state

temp

next_state

next_temp

The reference separates next-state/next-value logic from the stored values, while my implementation directly uses the running state and count.

Conceptually:

My design:
start / stop / reset
        ↓
     running
        ↓
      count

Reference:
current state/value
        ↓
next state/value
        ↓
registered state/value

Synthesis Results

My Design

Metric

Result

Wires

60

Cells

229

Area

728.20 µm²

Max Frequency

543.5 MHz

Critical Path

1.840 ns

Area Score

Beats 9.6%

Performance Score

Beats 73.5%

Reference Design

Metric

Result

Wires

59

Cells

181

Area

645.62 µm²

Max Frequency

520.8 MHz

Critical Path

1.920 ns

Area Score

Beats 57.1%

Performance Score

Beats 65.2%

Direct Comparison

Metric

My Design

Reference

Difference

Wires

60

59

+1

Cells

229

181

+48

Area

728.20 µm²

645.62 µm²

+82.58 µm²

Max Frequency

543.5 MHz

520.8 MHz

+22.7 MHz

Critical Path

1.840 ns

1.920 ns

-0.080 ns

Analysis

My design uses more hardware than the reference:

Area: 728.20 µm² vs 645.62 µm²

Cells: 229 vs 181

Wires: 60 vs 59

However, my design has better timing performance:

Maximum frequency: 543.5 MHz vs 520.8 MHz

Critical path: 1.840 ns vs 1.920 ns

Performance score: 73.5% vs 65.2%

Therefore, my implementation makes a trade-off of higher area for better performance.

Conclusion

I chose a simple running state-based approach instead of the reference's separate next-state and next-value signals. This made the logic easier for me to understand while still implementing the required behavior.

The synthesis results show that my design is faster than the reference, but it uses more area.

My Design   → More area, better performance
Reference   → Less area, lower performance
