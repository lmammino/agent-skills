# Detection signals

Each signal points to a possible implicit state machine. Confirm it by
reading the writers and the readers. A signal alone is not a finding.

## Field signals

### Boolean pair or group

Two or more booleans that change together.

```dart
bool gateOpen = false;
bool gateShown = false; // (gateOpen: true, gateShown: true) is invalid
```

Count the combinations. Name the invalid ones.

### Nullable as a state

A nullable value whose presence means a state.

```ts
let resumeNotice: string | null = null; // non-null means "resume"
```

Check it against the fields next to it. A pair such as
`complete: boolean` + `resumeNotice: string | null` often has an invalid
combination.

### Text as a state

Text that is compared to decide behavior.

```dart
if (error == launcherError) error = null;
```

The text is a message and a state at the same time. A changed message
breaks the check.

### One slot for many messages

One field that holds many unrelated messages. The last write wins, so a
minor message replaces an important one. A page that shows "any message"
also treats a minor message as a failure.

### Three-meaning nullable boolean

A `bool?` (or `Boolean | null`) where null, false and true mean three
states, for example "old install", "pending" and "seen".

### Reused flag

One flag that decides two unrelated things.

```dart
playHint = setupBack && !playHintSeen; // a Back-button flag also decides a hint
```

### Order of checks as priority

A chain of `if` checks that returns early. The order sets the priority
of the states, and nothing names the states.

### Declaration order as behavior

Behavior that depends on the order of enum members.

```dart
bool get failed => index >= httpError.index;
```

A new member in the wrong place changes the behavior.

### Copy that drops a field

A constructor or `copyWith` that builds a new value and leaves out a
state field, so the field resets to its default.

### Mirrored copies

A screen that keeps `late` or local copies of values that another
object owns. The copies can drift apart.

### Boolean parameter for a mode

A function or widget parameter `bool` that selects a mode, often next
to a second boolean for the same mode (`adding` and `setup: !adding`).

## Flow signals

### Writer without a guard

A listener, callback or stream handler that writes state with no check
of who owns the state now. Example: a player listener that changes the
main screen state while a preview uses the player.

### Two meanings of one condition

Several guards for the same idea, each with a different check. Example:
`previewSession != null` in some places and `!briefingLoaded` in
others. Count the guards. A branch with no guard often hides a bug.

### Paired clears

Two or more fields that each writer must clear together. Count the
places. A missing clear in one place is a bug.

### Flag set in many places, cleared in few

Count the writes that set the flag and the writes that clear it. Count
the readers. A flag with 19 set writes, 1 clear and no doc comment has
an unclear meaning.

### Phase in local variables

A coordinator that tracks its phase in several private fields and local
variables (`_running`, `starting`, `announced`), and exposes it as
booleans.

## Where state lives

Look at the places that hold mutable state in the project's framework,
for example UI state classes, stores, view models, reducers,
controllers and coordinators. Rank them by the number of mutable
fields and writes.

## Already explicit

Note the state holders that are already explicit, so the report can
name them:

- enums and sealed types with exhaustive switches;
- state classes with one field for the phase;
- schedules and latches with documented transitions.
