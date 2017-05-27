# screeps-typescript-profiler
> A light-weight TypeScript profiler for tagging classes and methods


This project is made as a lightweight alternative to [screeps-profiler](https://github.com/gdborton/screeps-profiler)

#### Example Output:
```
Profiler.output()

Function                                  Tot Calls    CPU/Call  Calls/Tick    CPU/Tick   % of Tot
MigrantWorkerProcess:doWork                    1594      0.29ms        5.69      1.66ms       24 %
CreepProcess:moveTo                            2629      0.16ms        9.39      1.48ms       22 %
MigrantWorkerProcess:doCharging                 966      0.22ms        3.45      0.77ms       11 %
MigrantWorkerProcess:doScavenging               534      0.38ms        1.91      0.73ms       11 %
ConstructionWorkerProcess:doWork                267      0.48ms        0.95      0.46ms        7 %
NavigatorProcess:getPath                        169      0.38ms        0.60      0.23ms        3 %
MigrantWorkerProcess:doUnloading                121      0.42ms        0.43      0.18ms        3 %
MigrantWorkerProcess:getJob                      42      1.07ms        0.15      0.16ms        2 %
NavigatorProcess:calculateNewPath                63      0.63ms        0.23      0.14ms        2 %
DesirePathProcess:run                           261      0.14ms        0.93      0.13ms        2 %
CreepProcess:isEmpty                           1458      0.02ms        5.21      0.11ms        2 %
MigrantWorkerProcess:doRefilling                 20      1.36ms        0.07      0.10ms        1 %
NavigatorProcess:getCostMatrix                   63      0.42ms        0.23      0.09ms        1 %
CreepProcess:isFull                             908      0.03ms        3.24      0.08ms        1 %
DesirePathProcess:decayPaths                     12      1.86ms        0.04      0.08ms        1 %
ArchitectProcess:run                             14      1.51ms        0.05      0.08ms        1 %
ArchitectProcess:supervise                       14      1.45ms        0.05      0.07ms        1 %
ArchitectProcess:placeRoad                       14      1.17ms        0.05      0.06ms        1 %
DesirePathProcess:growPaths                     261      0.05ms        0.93      0.05ms        1 %
DesirePathProcess:findRoad                       14      0.82ms        0.05      0.04ms        1 %
280 total ticks measured			6.87 average CPU profiled per tick
```

## Setup

- Add the `Profiler` folder to your project directory somewhere. 

- Import `Profiler` in your `main.ts`
  ```typescript
  // main.ts
  import * as Profiler from "./path/to/Profiler";
  ```

- Initialize the profiler in `main.ts` outside your main loop, and assign to `global` under whatever name you want to use as the CLI command

  ```typescript
  // main.ts
  import * as Profiler from "./path/to/Profiler";

  global.Profiler = Profiler.init();
  ```

- In any module you wish to profile, import the decorator, and either tag the class to profile everything, or tag specific class methods.  (Getters and Setters are specifically disabled to not interfere with other commonly used decorators)

  ```typescript
  // MyClass.ts
  import { profile } from "./path/to/Profiler";

  @profile
  export class myClass {
    /* */
  }

  // or

  export class myClass {
    @profile
    public someMethod() { /* */ }
  }
  ```
  
- The script also expects a global constant `__PROFILER_ENABLED__` to be set.  This is meant to be configured by build scripts.  If this value is set to `false`, **Profiler** will be completely disabled on the build.

## Usage

> The following assumes you attached **Profiler** to `global.Profiler`

From the command line:

**`Profiler.clear()`** - Clears the profiler data in memory.  Useful after refactoring code. (*NOTE: this doesn't stop the profiler*)

> *(The profiler is hardcoded to write data to `Memory.profiler`, so make sure you aren't already using this location)*

**`Profiler.start()`** - Starts the profiler

**`Profiler.stop()`** - Stops/Pauses the profiler

**`Profiler.status()`** - Returns whether is profiler is currently running or not

**`Profiler.output()`** - Pretty-prints the collected profiler data to the console
