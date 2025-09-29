# Ultimate Shell Madness Concept

## Command Struct

```c
typedef struct s_command {
    char **args;       // Tokenized arguments for this command
    uint8_t ops;       // Bitmask: pipe | ; & etc.
    int line_no;       // Original line number
} t_command;
```

Bitmask Ideas
Bit	Meaning
0x01	Pipe `
0x02	Background &
0x04	Sequence ;
0x00	Normal execution

Parsing Logic
Split input by \n → each line.

For each line:

Tokenize by spaces/tabs → get char **args.

Scan for operators (|, &, ;):

If found, set corresponding bit in ops.

Skip operator in final args array.

Wrap each line in a t_command struct.

Store all t_command* in an array → easy iteration/execution.

Execution Logic
```text
for each command in array:
    if command->ops & PIPE:
        setup pipe
    if command->ops & BACKGROUND:
        fork & execute in background
    else:
        execute normally
```

Advantages
No messy char *** spaghetti.

Operators are tracked cleanly via bitmask.

Lines and execution order are trivial to handle.

Extensible for future operators.

```bash
Input String (raw)
┌─────────────────────────────┐
│ ls -l | grep txt ; echo hi &│
└─────────────────────────────┘
            │
            ▼
Split by \n (lines)
┌──────────────────┐
│ ls -l | grep txt │
│ echo hi &        │
└──────────────────┘
            │
            ▼
Tokenize each line (spaces/tabs)
┌─────────────────┐
│ ["ls", "-l"]    │ → PIPE bit set (0x01)
│ ["grep", "txt"] │ → PIPE bit continues
│ ["echo", "hi"]  │ → BACKGROUND bit set (0x02)
└─────────────────┘
            │
            ▼
Wrap in t_command struct
┌─────────────────────────────┐
│ t_command {                 │
│   args: ["ls", "-l"]        │
│   ops: PIPE                 │
│   line_no: 1                │
│ }                           │
│ t_command {                 │
│   args: ["grep", "txt"]     │
│   ops: PIPE                 │
│   line_no: 1                │
│ }                           │
│ t_command {                 │
│   args: ["echo", "hi"]      │
│   ops: BACKGROUND           │
│   line_no: 2                │
│ }                           │
└─────────────────────────────┘
            │
            ▼
Execution Loop
for each t_command:
   check ops → fork/pipe/background/normal execution
```
