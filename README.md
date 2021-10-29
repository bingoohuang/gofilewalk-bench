# Go File Walk Test

Quick comparison between different Go walk implementations. [blog](https://engineering.kablamo.com.au/posts/2021/quick-comparison-between-go-file-walk-implementations), 
fork from [KablamoOSS/gofilewalk](https://github.com/KablamoOSS/gofilewalk)

1. build: `go install ./...`
1. confirm the output is the same for all: `gofilewalk-filepath-walk && gofilewalk-filepath-walkdir && gofilewalk-iafan && gofilewalk-karrick && gofilewalk-michealtjones && gofilewalk-readdir`
1. use [hyperfine](https://github.com/sharkdp/hyperfine) to test: `hyperfine gofilewalk-filepath-walk gofilewalk-filepath-walkdir gofilewalk-iafan gofilewalk-karrick gofilewalk-michealtjones gofilewalk-readdir`


The result show that:

1. In a few files' directory, `filepath-walkdir` won.
2. In a large number of files directory, [iafan](https://github.com/iafan/cwalk) won.

## for a few files

```bash
🕙[2021-10-28 22:27:59.780] ❯ gofilewalk-filepath-walk && gofilewalk-filepath-walkdir && gofilewalk-iafan && gofilewalk-karrick && gofilewalk-michealtjones && gofilewalk-readdir
18
18
18
18
18
18

~/Downloads/gofilewalk-bench via 🐹 v1.17.2 via C base
🕙[2021-10-28 22:28:12.546] ❯ hyperfine gofilewalk-filepath-walk gofilewalk-filepath-walkdir gofilewalk-iafan gofilewalk-karrick gofilewalk-michealtjones gofilewalk-readdir
Benchmark #1: gofilewalk-filepath-walk
  Time (mean ± σ):       3.4 ms ±   0.3 ms    [User: 1.2 ms, System: 1.1 ms]
  Range (min … max):     2.9 ms …   4.9 ms    495 runs

  Warning: Command took less than 5 ms to complete. Results might be inaccurate.

Benchmark #2: gofilewalk-filepath-walkdir
  Time (mean ± σ):       3.3 ms ±   0.3 ms    [User: 1.2 ms, System: 1.0 ms]
  Range (min … max):     2.7 ms …   5.4 ms    459 runs

  Warning: Command took less than 5 ms to complete. Results might be inaccurate.

Benchmark #3: gofilewalk-iafan
  Time (mean ± σ):       3.5 ms ±   0.2 ms    [User: 1.5 ms, System: 1.4 ms]
  Range (min … max):     3.1 ms …   4.9 ms    451 runs

  Warning: Command took less than 5 ms to complete. Results might be inaccurate.

Benchmark #4: gofilewalk-karrick
  Time (mean ± σ):       3.5 ms ±   0.3 ms    [User: 1.2 ms, System: 1.2 ms]
  Range (min … max):     3.0 ms …   4.8 ms    462 runs

  Warning: Command took less than 5 ms to complete. Results might be inaccurate.

Benchmark #5: gofilewalk-michealtjones
  Time (mean ± σ):       3.5 ms ±   0.3 ms    [User: 1.5 ms, System: 1.4 ms]
  Range (min … max):     3.1 ms …   5.3 ms    464 runs

  Warning: Command took less than 5 ms to complete. Results might be inaccurate.

Benchmark #6: gofilewalk-readdir
  Time (mean ± σ):       3.3 ms ±   0.6 ms    [User: 1.2 ms, System: 1.1 ms]
  Range (min … max):     2.8 ms …   8.7 ms    449 runs

  Warning: Command took less than 5 ms to complete. Results might be inaccurate.
  Warning: Statistical outliers were detected. Consider re-running this benchmark on a quiet PC without any interferences from other programs. It might help to use the '--warmup' or '--prepare' options.

Summary
  'gofilewalk-filepath-walkdir' ran
    1.01 ± 0.20 times faster than 'gofilewalk-readdir'
    1.03 ± 0.14 times faster than 'gofilewalk-filepath-walk'
    1.06 ± 0.14 times faster than 'gofilewalk-karrick'
    1.07 ± 0.13 times faster than 'gofilewalk-iafan'
    1.07 ± 0.13 times faster than 'gofilewalk-michealtjones'

~/Downloads/gofilewalk-bench via 🐹 v1.17.2 via C base took 18s
🕙[2021-10-28 22:28:38.979] ❯
```

## for much more files

```bash
🕙[2021-10-29 07:38:14.489] ❯ hyperfine gofilewalk-filepath-walk gofilewalk-filepath-walkdir gofilewalk-iafan gofilewalk-karrick gofilewalk-michealtjones gofilewalk-readdir
Benchmark #1: gofilewalk-filepath-walk
  Time (mean ± σ):     44.376 s ±  1.546 s    [User: 2.836 s, System: 20.368 s]
  Range (min … max):   40.395 s … 46.282 s    10 runs

Benchmark #2: gofilewalk-filepath-walkdir
  Time (mean ± σ):     19.435 s ±  0.641 s    [User: 1.689 s, System: 5.352 s]
  Range (min … max):   18.770 s … 21.040 s    10 runs

Benchmark #3: gofilewalk-iafan
  Time (mean ± σ):      8.193 s ±  0.688 s    [User: 6.366 s, System: 60.694 s]
  Range (min … max):    6.997 s …  8.977 s    10 runs

Benchmark #4: gofilewalk-karrick
  Time (mean ± σ):     29.753 s ±  0.959 s    [User: 3.314 s, System: 13.321 s]
  Range (min … max):   27.965 s … 31.209 s    10 runs

Benchmark #5: gofilewalk-michealtjones
  Time (mean ± σ):      8.312 s ±  0.820 s    [User: 6.628 s, System: 74.094 s]
  Range (min … max):    6.616 s …  9.804 s    10 runs

Benchmark #6: gofilewalk-readdir
  Time (mean ± σ):     18.167 s ±  0.692 s    [User: 1.246 s, System: 4.935 s]
  Range (min … max):   16.278 s … 18.749 s    10 runs

Summary
  'gofilewalk-iafan' ran
    1.01 ± 0.13 times faster than 'gofilewalk-michealtjones'
    2.22 ± 0.20 times faster than 'gofilewalk-readdir'
    2.37 ± 0.21 times faster than 'gofilewalk-filepath-walkdir'
    3.63 ± 0.33 times faster than 'gofilewalk-karrick'
    5.42 ± 0.49 times faster than 'gofilewalk-filepath-walk'

~/github via 🐍 v3.8.3 via C base took 21m23s
🕙[2021-10-29 22:36:46.510] ❯ gofilewalk-filepath-walk && gofilewalk-filepath-walkdir && gofilewalk-iafan && gofilewalk-karrick && gofilewalk-michealtjones && gofilewalk-readdir
767668
767668
767668
767668
767668
767668

~/github via 🐍 v3.8.3 via C base took 2m
```

## difference between fileapth.Walk and filepath.WalkDir 

```go
// Walk is less efficient than WalkDir, introduced in Go 1.16,
// which avoids calling os.Lstat on every visited file or directory.
```
