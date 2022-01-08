# timefolderWIP
App to automatically sort your smartphone camera's files into YYYY/MM/DD directories (or folders -- use whichever word you prefer, I chose folder in the title because of the pun on time travel gone wrong, but on \*nix systems directory is the term usually used)

This will be my first real Android app, so don't expect too much. The idea is simple: Since my smartphone puts all photos and videos taken into the `DCIM/Camera` directory and names them `IMG_YYYYMMDD.jpg` or `VID_YYYYMMDD.mp4`, I ended up with hundreds to thousands of files in that one path, making it quite tedious to browse. While that makes it easier to locate old photos visually when not remember the approximate date it was taken, I need to improve performance and decided to sort the files into a `YYYY/YYYY-MM` structure (the redundant year is on purpose for better distinction since my gallery app only uses the directory name itself). In the beginning I just did so manually every now and then, but of course that quickly became tedious and I ran a simple script via `adb` or `ssh`:

```bash
for year in 2019 2020 2021; do
  for month in $(seq -w 12); do
    mkdir -p $year/$month
    for file in "*$year-$month*"; do
      mv $file $year/$month
    done
  done
done
```

which of course can be improved a lot by e.g. using `$(seq 2019 $(date +%Y))` for the `year` loop and only `mkdir`ing directories for actually existing files. Using `touch` to fix the file modification date (which unfortunately seems to get updated by `mv`) would also be great, though IIRC that requires `su`. Anyway, even this is unnecessary work, and since I want to program an app myself anyway I might just as well use this simple project.

So the app functions, approximately in order of importance, are:

* manually trigger the sorting process as in the script above
* fix the file modification date if it still gets messed up by that relocation
* automatically trigger the sorting process as soon as files get written (if that doesn't mess up things, so it should be an option)
* more versatility concerning paths and date encoding in the filenames
