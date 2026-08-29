Counter Module – README
Description

I designed a counter module which counts up by 1 on every clock cycle when it is in the running state. The counter can be started using the start input and stopped using the stop input. The reset input clears the counter to 0 and also stops the counter.

The priority of the inputs is:

Reset > Stop > Start

I used a separate running variable to remember whether the counter is currently running or stopped. This is necessary because start and stop are pulses and do not remain active.

When start is pulsed, running becomes 1 and the counter starts counting. When stop is pulsed, running becomes 0 and the counter stops without changing its current value. When reset is pulsed, both count and running are set to 0.

I also added a condition to check whether the count has reached MAX. If count reaches MAX, it wraps around to 0 on the next cycle and continues counting.

I used always_ff @(posedge clk) because the counter and running state are sequential logic controlled by the clock.

Reference Code Comparison

The reference implementation uses additional variables such as temp, next_temp, state, and next_state. It separates the current values from their next values.

My implementation is simpler and directly uses count and running. I felt this was easier to understand because the running variable directly represents whether the counter should be counting or stopped.

The basic logic of my design is:

Start → running = 1 → count increases every clock

Stop → running = 0 → count holds

Reset → count = 0 and running = 0

count = MAX → count becomes 0 and continues

Performance and Area

My synthesis results were:

Wires: 60
Cells: 229
Area: 728.20 µm²
Maximum Frequency: 543.5 MHz
Critical Path: 1.840 ns
Performance: Beats 73.5%
Area: Beats 9.6%

The reference implementation had:

Wires: 59
Cells: 181
Area: 645.62 µm²
Maximum Frequency: 520.8 MHz
Critical Path: 1.920 ns
Performance: Beats 65.2%
Area: Beats 57.1%

Compared with the reference, my design uses slightly more area, with an area of 728.20 µm² compared to 645.62 µm². It also uses more cells, 229 compared to 181.

However, my design has better performance. Its maximum frequency is 543.5 MHz, compared with 520.8 MHz for the reference. The critical path is also lower at 1.840 ns, compared with 1.920 ns.

Therefore, my implementation has a trade-off: it uses more area but provides better performance.

Overall, I used a simpler state-based approach with running, while the reference used separate next-state and next-value variables. My approach was easier for me to understand and still achieved better timing performance in the synthesis results.
