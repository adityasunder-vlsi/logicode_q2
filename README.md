# logicode_q2
I had to Design a module that controls a counter. Pulsing start makes count increase by 1 every clock cycle; pulsing stop halts it. Pulsing reset clears count to 0 and stops counting.  If count reaches MAX, it wraps to 0 on the next cycle and keeps counting.  Priority was reset wins over stop, which wins over start.
